---
name: ia-delegation-local-to-gpt-chrome
description: >-
  Quand une application IA locale doit faire travailler un projet ChatGPT EXISTANT dans Chrome :
  rejoindre le bon fil par l'extension navigateur, transmettre un mandat borné, vérifier l'envoi,
  suivre le résultat et récupérer la livraison. Triggers : « délègue à GPT », « fais travailler
  ChatGPT dans Chrome », « ouvre ce chat GPT », « récupère son paquet », « local vers GPT Chrome ».
  PAS pour deux tâches locales, pour un agent déjà exécuté dans le navigateur, ni pour créer un
  projet ChatGPT. La cible GPT et le transport Chrome sont tous deux explicites dans le nom.
---

# Application locale → GPT dans Chrome

## Les règles qui se paient cher quand on les oublie

> ⛔ **ChatGPT se pilote par l'extension navigateur, toujours et d'abord — jamais par l'application de
> bureau.** Sous Windows, l'application nommée « ChatGPT » dans le menu Démarrer peut être Codex (paquet
> `OpenAI.Codex`), un agent de code à accès complet, pas le Chat (relevé les 29/09 et 04/10/2026) : y
> envoyer un mandat n'atteint pas le projet visé. **Contrôle, avant tout accès Computer Use ou
> ouverture d'application visant ChatGPT** : *l'extension a-t-elle ÉCHOUÉ dans CE tour (envoi tenté,
> témoin absent après rechargement) ?* Non → interdit.

> 🧠 **Un fil se règle avant son premier envoi : le dernier modèle et l'effort de réflexion le plus
> élevé que ton plan débloque.** Un fil neuf s'ouvre sur le réglage par défaut du compte, pas forcément
> sur le meilleur : le mandat partirait à un modèle plus faible, sans que rien ne le dise. **Contrôle,
> avant le premier message d'un fil neuf (et à la reprise d'un fil resté longtemps fermé)** : *le
> sélecteur porte-t-il le plus récent des modèles proposés, et la réflexion est-elle au cran le plus
> haut disponible ?* Non → on règle, on relit les deux libellés, puis on envoie. Le modèle lu s'écrit sur
> la ligne du registre. Le nom des modèles date, la règle non. Si l'utilisateur signale que la page bugue,
> on n'envoie rien tant qu'il n'a pas dit que ça marche. Relevé du menu (09/10/2026) et ses pièges :
> section « Modèle, mode et effort » de `references/pannes-et-reprise.md`.

> ✍️ **Lire le champ avant d'y écrire : un brouillon de l'utilisateur peut y attendre.** ChatGPT garde le
> texte tapé et non envoyé de chaque fil ; un mandat collé derrière part avec lui. **Contrôle, avant tout
> `insertText`** : *le champ est-il vide ?* Non → ne rien envoyer, ne rien effacer : c'est son texte.
> Attendre, ou passer par un autre canal, et le dire.

> 🌿 **Si le projet ChatGPT est connecté à ton dépôt, la branche prouve la réception, pas la page.** Un
> envoi peut disparaître au rechargement alors que GPT l'a reçu. Avant tout Stop et tout renvoi :
> `git fetch` de la branche du lot ; un commit neuf qui cite le message = reçu, on guette.

> 🚨 **Mode « Chat », pas « Work ».** Le sélecteur en haut de ChatGPT propose plusieurs modes ; la
> délégation se fait en **Chat**. Le bandeau de quota restant que ChatGPT affiche concerne le mode Work
> (sur le compte de l'auteur, il ne freinait pas le Chat) : le lire n'est pas une raison d'attendre.
> Les quotas dépendent de ton plan — à vérifier chez toi.

> 🥇 **GPT d'abord, avant un sous-agent.** Un travail que GPT peut faire (lire GitHub ou le web, trier,
> rédiger, écrire du code sur une branche dédiée) part ici, en mandat, au lieu d'un sous-agent Claude. Un
> sous-agent seulement pour ce que GPT n'atteint pas : un fichier ou un script local hors dépôt, une page
> qu'il n'ouvre pas, un navigateur connecté. **Les règles de ton dépôt priment** : certains projets
> gardent le code côté Claude et ne laissent à GPT que la recherche, la lecture et la critique.

> 🌐 **Tous les fils ChatGPT dans un seul navigateur : celui qui porte l'extension.** Chaque fil se
> pilote par l'extension, dans le groupe d'onglets de la session ; pas dans un second navigateur. Un
> projet joignable seulement dans un autre navigateur ou un autre compte : on le dit à l'utilisateur, on
> ne bascule pas.

> 🧹 **Fermer l'onglet ChatGPT après chaque gros message.** Un fil long ouvert pèse sur la mémoire du poste.
> Un gros envoi vérifié (champ vide, message présent une fois, génération en cours) → fermer l'onglet ;
> GPT continue de travailler sans lui. Pour lire le rendu : rouvrir le fil dans un onglet neuf, lire,
> refermer. Jamais un onglet ChatGPT laissé ouvert entre deux réveils d'une boucle. **Contrôle, à la fin
> de chaque délégation** : *un onglet ChatGPT reste-t-il ouvert ?* Oui → le fermer.

> 🗂️ **La compétence se souvient des fils, pas l'utilisateur.** La mémoire, c'est le **registre des fils**,
> plus bas. Quatre gestes, à chaque invocation :
> 1. **Avant d'écrire** : trouver le fil au registre, par le sujet et par la session Claude qui y
>    travaille. Un sujet inscrit a son fil : ne jamais redemander un lien déjà inscrit.
> 2. **Un fil nouveau** (lien donné par l'utilisateur, ou fil ouvert sur son ordre) : sa ligne entre au
>    registre avant le premier envoi.
> 3. **Après chaque lot rendu** : la ligne se met à jour (dernier lot, résumé en une phrase, qui a la main).
> 4. **Un fil = une seule session Claude.** Une autre session qui a besoin de GPT, même sur un sujet
>    voisin, ouvre son propre fil et l'inscrit avant le premier envoi. Jamais deux sessions sur un fil :
>    leurs lots se croisent, et GPT mélange les contextes.

> 🧵 **Une session, un fil : elle y reprend ses lots, jamais un fil neuf par mandat.** Le fil suit la
> session et son sujet, pas le lot du jour. Un mandat neuf de la même session part dans son fil, ouvert
> par une ligne : « nouveau lot, distinct du précédent ». **Avant d'écrire** : chercher le fil au
> registre, puis dans l'état du dépôt. Introuvable : demander le lien en une ligne. Ne jamais conclure
> seul « c'est un sujet neuf », ne jamais copier le « fil neuf » d'une autre session.

> 🔁 **Le cycle : un gros mandat, GPT exécute, Claude relit tout, un seul retour, jusqu'au bout.** Le gros
> travail (lire beaucoup, écrire un long document, cartographier, comparer) part ici, pas en jetons
> Claude ni en sous-agents.
> 1. Un mandat écrit dans le dépôt, complet : le lot entier, les sources à lire, les règles, le rendu.
> 2. Un envoi, dans le fil du sujet (registre), en mode Chat.
> 3. Attendre par un minuteur ; jamais sonder l'onglet.
> 4. Relire tout le rendu contre les sources. Les écarts partent en **un seul** retour (un fichier de
>    retour dans le dépôt), jamais corrigés en silence à sa place.
> 5. Recommencer jusqu'au correct ; alors installer, publier, commiter.
>
> Claude garde le jugement, la relecture et les gestes d'une seule commande.

## Le registre des fils

La seule liste des fils ChatGPT donnés par l'utilisateur ou ouverts sur son ordre. Une ligne par fil. On
la lit avant d'écrire ; on la met à jour après chaque lot. « non relevé » : l'information manque, elle
s'ajoute au prochain passage dans ce fil.

⚠️ **Ce tableau vit dans la compétence installée : une mise à jour du plugin le remet à zéro.** Pour qu'il
survive, copie-le dans un fichier à toi (par exemple `ia-delegation-registre.md` à la racine de ton
projet) et tiens-le là ; la compétence le lit où tu lui dis.

| Fil | Sujet | Sessions Claude qui y travaillent | Ouvert | Résumé | Dernier lot · qui a la main |
|---|---|---|---|---|---|
| *(aucun pour l'instant : le premier fil piloté s'inscrit ici)* | | | | | |

## Contrat

Cette compétence pilote un **projet ChatGPT existant** depuis une application IA locale. Chrome est le
transport de conversation ; le projet ChatGPT reste le cerveau délégué et le dépôt reste la source de
vérité.

Elle ne couvre pas :

- application locale → application locale : `ia-delegation-local-to-local` ;
- navigateur → navigateur : `ia-delegation-browser-to-browser` ;
- création ou configuration d'un projet ChatGPT : décision explicite de l'utilisateur ;
- lecture passive d'un ensemble de conversations web : un capteur local en lecture seule, si ta machine
  en a un, seulement quand aucune trace native plus étroite n'existe.

## Entrée minimale

Avant d'écrire, disposer de quatre éléments :

1. le **sujet exact** et la décision ou livraison attendue ;
2. le **projet ChatGPT** pertinent — celui dont les instructions portent la méthode du sujet ; si tu tiens
   une carte de tes projets ChatGPT, c'est elle qui désigne la cible ;
3. le **fil existant** à continuer, lu dans le registre des fils ;
4. la **preuve de sortie** attendue : message, fichier, commit, rendu ou verdict.

Si la cible manque, demander en une ligne : « Quel fil ChatGPT dans Chrome : lien ou nom ? ». Ne jamais
prendre la première conversation ressemblante et ne jamais créer un fil neuf pour un sujet déjà en cours.

## Exécution

### 1. Atteindre le bon fil

- Utiliser d'abord le contrôle navigateur par extension, jamais Computer Use tant que l'extension
  fonctionne.
- Lire le projet et les derniers tours avant toute écriture.
- Si le fil travaille encore sur la même demande, ne pas l'interrompre.
- Vérifier l'URL, le projet et le titre juste avant d'envoyer.
- **Fil neuf : lire le modèle et l'effort avant le premier message** (règle 🧠 en tête).

### 2. Donner un mandat, pas une méthode concurrente

Le projet porte déjà sa méthode. Le message apporte seulement :

- le contexte nouveau indispensable ;
- les chemins, liens ou sources qu'il peut lire lui-même ;
- les contraintes et vérités verrouillées ;
- le résultat observable attendu ;
- une demande finale explicite.

Ne pas recopier un fichier que ChatGPT peut ouvrir. Donner son chemin. Ne pas envoyer plusieurs
micro-messages : un seul mandat cohérent évite les réponses contradictoires.

**Un nombre du résultat attendu se compte par script sur sa source, avant l'envoi.** « 27 tâches »,
« 31 écarts » : GPT le prend comme critère de réception et s'y ajuste. Un compte faux dans le mandat
devient un rendu faux, que GPT déclare conforme.

### 3. Prouver que l'envoi est parti

Un geste d'envoi accepté par l'outil ne prouve pas que ChatGPT l'a reçu. Après chaque envoi :

- vérifier que le composer est vide ;
- vérifier qu'un nouveau tour utilisateur apparaît dans le fil, **après un rechargement de la page** :
  l'affichage seul ne prouve rien. Un message affiché, composer vide, peut disparaître au rechargement
  quand le fil est tenu par un tour figé ;
- un tour figé se voit aussi sans le lire : aucun commit neuf sur la branche du lot depuis 20 minutes,
  alors que le mandat en annonçait plusieurs. Alors ouvrir le fil, « Stop », renvoyer, recharger ;
- si le texte reste dans le composer, ne pas attendre une réponse inexistante ;
- ne jamais renvoyer le même message sans relire le fil.

### 4. Suivre par le canal le plus étroit

Lire le DOM ou le texte du fil avant d'utiliser une capture. Pendant une génération, attendre ; quand un
résultat stable apparaît, le relire intégralement. Si le livrable est annoncé dans le dépôt, faire un
`git fetch` puis lire le commit réel. S'il vit seulement dans le fil ou dans un canvas, le récupérer par
Chrome et le confronter à la source active avant de l'installer.

### 5. Boucler sans devenir auteur à sa place

Un feedback de reprise contient : constat précis, conséquence, résultat attendu, éléments à préserver et
preuve demandée. Une fois envoyé, laisser ChatGPT corriger. Ne pas rédiger silencieusement sa réponse à
sa place, sauf si l'utilisateur a explicitement changé de propriétaire.

**Tout message finit par une demande** — unique, précise, actionnable : *« À toi : \<UN livrable
nommé\>. Contraintes : \<les bornes\>. Rendu attendu : \<forme exacte\>. »* Un compte rendu sec ou une
interdiction en dernier mot ne laisse rien à exécuter, et la boucle s'arrête.

### 6. Clore par un reçu

Le reçu indique : fil piloté, demande envoyée, résultat stable, emplacement de la livraison, vérification
effectuée et reste éventuel. Une réponse dans le chat n'est pas une source durable tant qu'elle n'a pas
rejoint son propriétaire documentaire ou le dépôt.

## Récupération d'un paquet

Un paquet produit hors dépôt doit être autosuffisant : manifeste, fichiers complets et destination. Le
télécharger dans un dossier temporaire, vérifier empreinte et chemins, puis installer seulement les
cibles autorisées. Si GPT est connecté au dépôt, il y écrit et pousse lui-même : on relit son commit, on
ne transporte pas de fichiers. Protocole complet (trois voies de récupération, vérification avant
d'écraser, reçu) : [`references/retour-paquet.md`](references/retour-paquet.md).

## Pannes et reprise

L'essentiel ; le détail, les sélecteurs relevés et leurs dates sont dans
[`references/pannes-et-reprise.md`](references/pannes-et-reprise.md).

- Extension déconnectée : le dire précisément et laisser la reconnexion à l'utilisateur. Groupe d'onglets
  perdu après reconnexion : ce n'est pas une panne, on le recrée et on continue.
- « Could not load this ChatGPT conversation » (titre « New chat », aucun champ de saisie) : ce n'est pas
  le fil qui est cassé, ChatGPT bride. Fermer l'onglet, faire autre chose dix minutes, rouvrir une fois ;
  les rechargements en série entretiennent la bride.
- Fil chargé partiellement, flux décroché : **onglet au premier plan, clic dans la conversation** avant de
  conclure à une absence de réponse. Recharger ne fait que relire.
- Tour figé (« Analysis paused », « Connection interrupted », bouton « Stop » toujours là, aucun pas
  neuf entre deux lectures espacées) : « Stop », renvoyer, vérifier après rechargement.
- Un envoi invisible au rechargement n'est pas forcément perdu : la branche le dit (règle 🌿).
- Onglet masqué : la capture d'écran expire, mais le fil se lit et s'écrit par script. Ne pas conclure
  au gel, ni passer par Computer Use.
- Onglet lisible mais envoi inerte : vérifier le composer ; recycler uniquement l'onglet connu, jamais
  créer une nouvelle conversation.
- Computer Use est un repli documenté, pas le chemin normal : il monopolise le poste et n'est pas requis
  pour ce transport quand l'extension navigateur répond.
- Après deux échecs identiques, changer d'instrument ou exposer le blocage ; ne pas répéter le même geste
  en silence.
- Garde-fou de ton environnement qui refuse l'envoi (refus classé « exfiltration de données ») : ne pas le
  contourner, ni le faire refaire par une autre session ou un autre outil. Le dire à l'utilisateur en une
  phrase, avec le texte prêt à coller dans le bon fil : il l'envoie lui-même, ou il autorise l'envoi une
  fois.

## Garde-fous

- **Relais en lecture seule par défaut** : en délégation, Claude transmet et relit ; il ne décide pas du
  contenu que ChatGPT produit. Sous un mode de coopération (ci-dessous), il révise le rendu et tranche —
  dans les bornes du contrat du mode actif, jamais au-delà : réviser un delta n'est pas réécrire le lot à
  la place de GPT.
- **Jamais toucher un livrable final** (site en production, document publié) directement depuis cette
  compétence : il reste derrière la revue de la personne ou de la compétence qui a le contexte métier.
- **Jamais créer un projet ChatGPT** sans demande explicite de l'utilisateur : on pilote l'existant.
- **Un sujet par fil.** Mélanger deux sujets dans un même fil casse la lisibilité pour l'utilisateur et
  pollue le contexte de ChatGPT. (Plusieurs lots d'un même sujet, eux, vivent dans le même fil.)
- **Clôture** : inscrire le fil au registre ci-dessus, et dans le suivi que ton projet utilise déjà s'il
  en a un — sinon une mention dans le fil de la tâche suffit, pas de nouveau registre à inventer. Avant
  d'ouvrir un fil, vérifier que le sujet n'a pas déjà été traité.

## Modes de coopération

- **Délégation pilotée** : produire un résultat borné et vérifiable. Contrat détaillé :
  [`references/delegation-pilotee.md`](references/delegation-pilotee.md).
- **Réflexion croisée** : confronter un raisonnement sans lui déléguer la décision finale. Contrat :
  [`references/reflexion-croisee.md`](references/reflexion-croisee.md).

## Compétences liées

`claude-ia-delegation` (le chapeau qui route) · `ia-delegation-local-to-local` ·
`ia-delegation-codex-to-claude-desktop` (mêmes témoins d'envoi, transport Windows distinct) ·
`ia-delegation-browser-to-browser` · `claude-codex` (la cible qui a le disque, quand le travail touche des
fichiers). Une carte locale de tes projets ChatGPT peut résoudre la cible, et un capteur web en lecture
seule peut accélérer l'attente : aucun de ces compléments n'est requis par ce plugin et aucun ne remplace
la vérification d'envoi dans Chrome.

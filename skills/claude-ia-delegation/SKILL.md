---
name: claude-ia-delegation
description: >-
  Quand un travail peut être confié à un cerveau moins cher ou déjà contextualisé → choisir la
  cible puis le transport exact, sans confondre application locale, GPT dans Chrome, Codex, une
  autre session et navigateur vers navigateur. Triggers : « délègue ça », « économise mes
  tokens », « fais bosser GPT / Codex / Gemini », « parle à l'autre tâche », « quel canal entre
  ces agents », « arbitre les cerveaux ». Chapeau mince : il décide et route ; les compétences
  de transport exécutent. Ne pas déléguer une décision sensible, un jugement fin ni une tâche
  plus courte que son propre handoff.
argument-hint: "[gpt|codex|gemini|hubspot|session|navigateur|desktop|claude]"
---

# Skill — claude-ia-delegation (déléguer au cerveau adapté)

> 🎛️ **LES MODES : UN MOT APRÈS LA COMMANDE CHOISIT LA CIBLE.**
> `/claude-ia-delegation` seul : le chapeau choisit (§1), sans rien demander. Le mot tapé arrive en
> fin de compétence, sous « ARGUMENTS ».
>
> | Mode | Cible | Transport | Dans ce pack |
> |---|---|---|---|
> | *(rien)* | le chapeau choisit | §1 | — |
> | `gpt` | ChatGPT dans Chrome, mode Chat | `ia-delegation-local-to-gpt-chrome` | oui |
> | `codex` | Codex, l'agent OpenAI local qui a le disque | `claude-codex` | oui |
> | `gemini` | Gemini | branche non câblée : le principe du §2, aucune compétence | — |
> | `hubspot` | Breeze, l'IA de HubSpot | `claude-breeze` | **non** : installe le pack HubSpot public <https://github.com/Gremelinn0/sales-hubspot-toolkit> pour ce mode |
> | `session` | une autre session Claude, vivante | `ia-delegation-local-to-local` | oui |
> | `navigateur` | d'un onglet web à un autre | `ia-delegation-browser-to-browser` | oui, expérimental |
> | `desktop` | depuis Codex, une session Claude Desktop | `ia-delegation-codex-to-claude-desktop` | oui |
> | `claude` | personne : Claude fait lui-même | — | — |
>
> Un mot inconnu, ou un mode dont le transport manque à ton installation : le dire en une ligne,
> puis choisir seul (§1). La liste vit ici et dans l'`argument-hint` de l'en-tête, qui l'affiche
> quand on tape la commande ; une branche ajoutée (§4) ajoute son mode aux deux.

> 🗂️ **Chaque transport tient le registre de ses cibles, et on le lit avant toute délégation** : les
> fils ChatGPT (`ia-delegation-local-to-gpt-chrome`), les sessions locales
> (`ia-delegation-local-to-local`), les onglets web (`ia-delegation-browser-to-browser`), les sessions
> Claude Desktop (`ia-delegation-codex-to-claude-desktop`), les travaux Codex (`claude-codex`). Un sujet
> inscrit a sa cible : on ne la redemande pas à l'utilisateur. Ces registres sont des gabarits vides
> dans chaque compétence ; une mise à jour du plugin les remet à zéro, donc copie-les dans un fichier
> à toi (le dossier de ton projet) si tu veux qu'ils survivent.

## ⚖️ Une conclusion qui revient d'un autre cerveau ne se corrobore pas toute seule

Déléguer, c'est **hériter d'une conclusion sans hériter de son objet**. Si un contrôle d'ici tombe
d'accord avec ce que l'autre modèle a rendu, l'accord ne vaut que si les deux jugeaient **la même
chose, au même critère** — sinon c'est une **coïncidence**, et elle rend la conclusion fausse
*inattaquable* au lieu de la corriger.

**Le contrôle, avant de traiter un accord comme une preuve** : *les deux verdicts portent-ils sur le
MÊME objet ?* Non → chacun se prouve seul.

## §0 Le principe — arbitrage de capacité, de contexte et de coût

Router chaque tâche vers **le cerveau adapté qui sait la faire sans perte de qualité** : une IA native
de plateforme, ChatGPT ou Gemini, Codex, ou une autre session déjà contextualisée. Le coût réel et le
quota disponible se vérifient dans le plan et le modèle actifs ; ils ne se déduisent jamais de mots
comme « natif », « inclus » ou « déjà payé ».

But = **efficience de contexte + coût** : confier le volume au bon agent, garder ici le jugement fin et
ne transmettre que ce qui est nécessaire à l'exécution et à la preuve.

### Le modèle à quatre couches

Une délégation n'est complète que si elle nomme séparément :

1. **Routeur** : agent choisi et raison du choix.
2. **Mandat** : canal de commande qui atteint la bonne session avec un brief borné.
3. **Livrable** : canal d'artefact durable, souvent distinct du mandat. Un fichier, un transcript ciblé
   ou un dépôt Git transporte un résultat ; il ne crée pas une conversation magique entre agents.
4. **Cadre** : preuve attendue, cadence, limite de tours, repli et condition d'arrêt.

Une **boucle** réveille une même session. Elle ne transporte pas le travail entre deux agents.

## §0bis Le réflexe se DÉCLENCHE, il ne s'invoque pas

Le défaut structurel d'un chapeau de routage : personne ne l'invoque au bon moment, et le bon moment
est précisément celui où l'on est déjà lancé dans le travail.

**Le déclencheur — une question, posée AVANT d'attaquer un gros bout** : *ce que je m'apprête à faire,
est-ce que ça a besoin de MOI ?* Trois signaux, **un seul suffit** :

1. **Je m'apprête à produire du VOLUME** — recherche large, brouillon long, consolidation, veille,
   comparatif. Le volume est ce qui coûte, et c'est ce qui se délègue le mieux.
2. **Un cerveau déjà accessible couvre le sujet** — un projet ChatGPT existe avec sa méthode dans ses
   instructions · une plateforme a son IA native · une session sœur a déjà le contexte et la
   connexion · Codex a le disque. Son quota et son coût restent à vérifier dans le plan actif.
3. **Mon jugement fin n'est pas requis** — aucune décision produit à prendre, rien à arbitrer.

**Ce qui NE déclenche JAMAIS — et c'est la moitié de la règle** : le jugement fin · une décision · une
donnée sensible · et **tout ce qui prend moins de temps à faire qu'à déléguer**. Un aller-retour vers
un autre cerveau coûte deux tours au minimum ; sous ce seuil, déléguer est une **perte**.

> 🔄 **Le travail local n'est pas exclu pour autant.** Tant que la seule cible était un chat web sans
> disque, « il faut ouvrir des fichiers » voulait dire « aucune cible ne sait faire ». Un agent local
> comme Codex sait ouvrir les fichiers, lancer les commandes, lire les logs et écrire dans plusieurs
> dépôts. La ligne de partage n'est plus *où vit la donnée*, c'est *qui doit trancher* : le
> **jugement** et la **décision** ne se délèguent pas, le reste peut.

**Le mode s'ANNONCE en une ligne, jamais en silence** — « je passe ça à \<cerveau\>, je récupère et je
relis ». Une délégation invisible fait croire que le travail a été fait ici, et plus personne ne sait
d'où vient le résultat.

## §1 Le décideur — choisir le cerveau adapté (coût inclus)

> 🎯 **Un mode passé en argument force la branche** (bloc 🎛️ en tête, ex. **`/claude-ia-delegation
> codex`**). **Totalement optionnel.** L'utilisateur le précise, OU le contexte de la conversation le
> rend clair → on prend cette cible. Sinon → ce tableau décide, et **on ne demande RIEN** (règle plus bas).

Avant de se lancer dans une recherche ou une analyse, une question : **quel cerveau a la capacité, le
contexte et le coût adaptés à cette tâche ?** Écarter d'abord les options qui dégradent la qualité ou la
preuve, puis retenir la moins coûteuse parmi celles qui suffisent :

| La tâche… | Cerveau | Comment |
|---|---|---|
| porte sur une plateforme avec une IA native + crédits (HubSpot…) | **l'IA de la plateforme** | via sa compétence plateforme (§2) |
| = recherche / rédaction / brainstorm offloadable vers un projet ChatGPT déjà configuré (méthode dans ses instructions de projet) | **ChatGPT dans Chrome** | `ia-delegation-local-to-gpt-chrome` |
| a besoin de **toucher des fichiers réels** — plusieurs dépôts, du code, un build, des tests, une revue de diff, un balayage | **Codex** (agent OpenAI local) | `claude-codex` |
| part de **Codex local** et doit poursuivre une session Claude Desktop existante | **Claude Desktop** | `ia-delegation-codex-to-claude-desktop` |
| = recherche / raisonnement général offloadable, pas de plateforme câblée (Gemini…) | **Gemini** (ou une autre IA pas encore câblée) | accès, quota et coût à vérifier (§2) |
| a besoin d'une connexion ou d'un contexte qu'une **autre session** a déjà (ex. le contrôle du navigateur mort ici, vivant ailleurs) | **une autre session** | `ia-delegation-local-to-local` : les commandes natives de tâches et de sessions de ton application |
| = jugement fin, décision produit, donnée sensible | **Claude directement** | c'est la valeur ajoutée, on la garde |

**Si le bon cerveau n'est pas clair → trancher SEUL selon la situation. JAMAIS demander à l'utilisateur
« sur quelle plateforme veux-tu que je travaille ? »** — on ouvre la session, on pose la question à
l'agent de la plateforme, on récupère sa réponse, et on la ferme. On ne délègue pas ce qui perdrait à
l'être (données sensibles, jugement fin, décision produit) ; on ne fait pas soi-même ce qu'un cerveau
accessible et moins coûteux dans le plan actif fait aussi bien.

## §1bis L'ORDRE DES GESTES — collaborer d'abord, capturer en clôture

Le cerveau est choisi (§1). Reste **par où on commence** — c'est écrit une fois ici pour toutes les
branches :

1. **SE CONNECTER à la conversation EN COURS** — le bon projet, le bon fil, chez l'agent visé.
2. **LIRE LA FIN** — l'agent a déjà du contexte et **annonce très souvent la prochaine étape lui-même**.
   C'est ce qu'on vient chercher.
3. **TRAVAILLER AVEC LUI** — exécuter cette étape en collaborant, en plus de ce que l'utilisateur demande
   par ailleurs. C'est le métier de ces compétences.
4. **PUIS SEULEMENT** capturer / archiver / loger au registre.

**Pourquoi ça mérite une section** : la capture est le geste le plus *facile à écrire* dans une
compétence (un chemin, un bouton, un registre), donc il finit toujours par occuper le plus de place — et
la place devient de l'autorité. Cas rencontré : face à un écran montrant une conversation d'agent en
cours qui disait *littéralement* quoi faire ensuite, la réponse a été de demander à l'utilisateur de
copier-coller le contenu pour l'archiver. Le contenu affiché était une **instruction**, pas un artefact à
ranger.

⚠️ **Le test, dès qu'un écran / export / document d'agent arrive** : *résultat à ranger, ou consigne à
exécuter ?* Une consigne s'exécute — on l'archive après l'avoir faite, jamais à la place. Et un contenu
qu'on peut aller chercher soi-même en poursuivant l'échange ne se fait jamais copier-coller par
l'utilisateur.

**La seule exception, et elle est explicite** : l'utilisateur demande un **paquet précis** (« va chercher
le zip », « installe le paquet ») — là, la récupération *est* la tâche
(`ia-delegation-local-to-gpt-chrome`, `references/retour-paquet.md`). L'exception se reconnaît à ce
qu'elle est **réclamée**, pas déduite.

## §1ter CE QUE LA SUPERVISION APPORTE — mesurer, jamais relire

Une fois le travail délégué, la tentation est de **relire le raisonnement** de la cible pour vérifier
qu'il tient. C'est le réflexe qui rend la délégation inutile : on paie deux fois le même raisonnement,
une fois chez elle, une fois chez soi — exactement ce que §0 cherche à éviter. Et ça n'apporte presque
rien, parce que ces cerveaux raisonnent bien.

**La supervision qui paie tient en deux gestes, et ils ont un point commun : ce ne sont pas des
raisonnements, ce sont des mesures.**

| Geste | Pourquoi c'est nous et pas elle |
|---|---|
| **Fournir les faits qu'elle ne peut pas atteindre** | elle n'a pas toujours le disque, le dépôt, les logs. Tout ce qu'elle sait du terrain, elle le tient de ce qu'on lui a donné. |
| **Vérifier les chiffres qu'elle avance** | un chiffre qu'elle cite vient d'une source datée. Sur un dépôt vivant, il périme en heures. Le recompter coûte une commande ; le croire coûte une décision fausse. |

**Ce qu'on ne fait pas** : relire sa logique, reformuler ses conclusions, refaire son analyse « pour être
sûr ». Si sa conclusion dépend d'un fait, on mesure **le fait** — pas le chemin qui y mène.

Observé sur une session de pilotage réelle : les deux corrections qui ont servi étaient des mesures,
jamais des jugements — un garde de sécurité déclaré durci alors qu'il s'ouvre sur exception (visible en
trois lignes de code que la cible ne pouvait pas ouvrir), et un dimensionnement périmé par un commit du
jour même. Et l'inverse dans la même session : la seule « vérification » faite de mémoire au lieu d'être
mesurée était **fausse**, et la cible l'avait déjà intégrée avant qu'on la rattrape. *Une supervision qui
ne mesure pas ajoute du bruit, et parfois du faux.*

### Le corollaire qui économise le plus : lire et écrire par des canaux différents

Dans cette famille, le gaspillage le plus courant n'est pas le raisonnement — c'est **la lecture**.
Récupérer la réponse d'un agent en aspirant sa page entière ramène tout ce qui l'entoure : les consignes
épinglées, l'historique, l'interface. On paie des milliers de mots pour en extraire trois phrases, **à
chaque tour**.

- **Lire par le canal le plus étroit disponible** — celui qui rend *la dernière réponse*, pas la page.
  Quand la machine expose un tel canal (une application locale qui suit les sessions, une API, un
  export, un transcript), il gagne toujours.
- **Écrire par le canal le plus fiable** — celui qui produit de vraies frappes. L'écriture est rare,
  visible, et une écriture ratée coûte bien plus cher qu'une lecture chère.

La mécanique concrète de ce partage appartient à chaque branche (§2), pas ici.

## §1quater 🖱️ UN FLUX DÉCROCHÉ SE REPREND AU PREMIER PLAN + CLIC DANS LA PAGE — recharger ne suffit pas

La règle vit ICI plutôt que dans une branche parce qu'elle ne décrit pas une plateforme : elle décrit
**un client web qui a perdu son flux**. Toute branche pilotée par le navigateur la subit.

**Le symptôme, sous ses masques** : la réponse s'arrête en plein milieu · le fil paraît figé · un message
envoyé ne produit aucun tour · « elle n'a pas terminé » depuis dix minutes. **Le symptôme seul ne permet
pas d'attribuer la cause au serveur, au modèle ou à une protection volontaire de la plateforme.**

**Le geste** : ① onglet **au premier plan** · ② **vrai clic DANS la page**, dans la conversation. Le flux
reprend.

⛔ **Ce qui ne marche pas** : recharger l'adresse · recycler l'onglet · attendre · renvoyer un message.
Recharger **relit** ce que le serveur a déjà — ça ne fait jamais **reprendre** un flux décroché.

⚠️ **Pourquoi ça se re-découvre à chaque fois** : rien n'échoue. Pas d'erreur, pas de bandeau, un DOM
cohérent. Une réponse tronquée ressemble à une réponse finie, un fil figé ressemble à une génération
lente — donc on attend, puis on accuse le canal, puis on refait faire le travail. **Le tell décidable** :
ça dure et **rien n'avance d'un caractère** entre deux lectures espacées.

**La mécanique reste chez la branche** (comment fronter, comment cliquer, quel témoin) — par exemple
`ia-delegation-local-to-gpt-chrome`, `references/pannes-et-reprise.md`. Le chapeau porte la **règle**, pas
le geste technique (§3).

## §2 Les branches (routage — le chapeau POINTE, il ne fait pas)

- **HubSpot** → **`claude-breeze`**, **absente de ce pack** : pour ce mode, installe le pack HubSpot
  public <https://github.com/Gremelinn0/sales-hubspot-toolkit>. Mode Breeze (IA native ; quota et coût à
  vérifier dans le portail) pour un diagnostic, mode navigation écran-par-écran pour une fonctionnalité
  sans API. La méthode HubSpot vit là-bas, pas ici.
- **Application locale → ChatGPT dans Chrome** → **`ia-delegation-local-to-gpt-chrome`** : pilote un
  projet ChatGPT EXISTANT par le contrôle navigateur. La cible GPT et le transport Chrome sont tous deux
  dans le nom.
- **Codex** → **`claude-codex`** : l'agent OpenAI **local**, celui qui a le disque. Là où un chat web
  raisonne sans clone local complet, Codex monte plusieurs dépôts ensemble, écrit du code, lance des
  builds et des tests, et rend une revue de diff. Le geste (`codex exec`, sa racine de travail, ses
  dossiers supplémentaires, son bac à sable), le cadrage par `AGENTS.md` et la relecture de ce qu'il a
  commité vivent là-bas, pas ici.
- **Codex local → Claude Desktop** → **`ia-delegation-codex-to-claude-desktop`** : résout une session
  Claude existante par son transcript, puis réserve Computer Use à l'envoi unique. Une capture d'écran
  n'est qu'un repli visuel ; le suivi revient aussitôt au transcript.
- **Recherche générale (Gemini…)** → principe posé, mécanique à câbler quand on l'utilise vraiment (leur
  interface via le contrôle du navigateur, ou un MCP dédié si connecté). Ne pas gonfler ce chapeau avec
  cette mécanique avant de l'avoir.
- **Application locale → application locale** → **`ia-delegation-local-to-local`** : utiliser les
  commandes natives de tâches et de sessions (`read`, `wait`, `send`) ou le transcript local, jamais
  l'interface pour lire un statut.
- **Navigateur → navigateur** → **`ia-delegation-browser-to-browser`** : voie MVP par extension,
  seulement si lecture et écriture sont réellement exposées et testées dans la session courante.
- **Local / valeur ajoutée** → Claude exécute directement.

> 📡 **LE CANAL DE LECTURE SE RÉSOUT PAR TRAJET, IL N'EST PAS UN PRODUIT UNIQUE.** Priorité :
> (1) commande native de tâche ou de session ; (2) transcript ou export local ; (3) API ou connecteur
> dédié ; (4) un capteur local de conversations web en lecture seule, si ta machine en a un, uniquement
> pour une conversation web sans trace native ; (5) interface. Pour Claude Code, les fichiers de
> transcript suffisent. Pour Codex, les commandes de threads suffisent.
>
> ⚠️ **Lecture seule** : écrire reste le geste de la branche concernée, avec son consentement — un
> capteur qui écrirait changerait de nature. ⚠️ Un capteur local doit **dire** qu'il ne tourne pas au lieu
> de rendre une liste vide : c'est la différence entre « il n'y a rien » et « je ne vois rien ».

## §3 Garde-fous — rester MINCE

- **Ce chapeau ROUTE, il ne ré-implémente pas.** La méthode de chaque cible vit dans SA compétence. Si ce
  fichier se met à décrire *comment* parler à une plateforme, c'est qu'une branche déborde → la sortir
  dans une compétence de transport.
- **Une branche non câblée reste un principe d'une ligne** (Gemini aujourd'hui) — pas d'abstraction sur du
  vide, elle grossit le jour où on la branche pour de vrai.
- **La clôture appartient à la compétence de transport** : c'est elle qui loge son travail dans le
  registre de ses cibles (et dans ton suivi de projet, s'il en existe un), pas l'orchestrateur. Contrat
  minimum : un **récap d'opérations** en fin de tâche — délégué à qui · créé ou modifié où (chemins) · où
  se trouve la trace brute. La trace brute se garde au moment où on la lit : du stockage opportuniste de
  ce qu'on lit déjà, pas une corvée d'archivage.

**Contrat route par route** : chaque branche déclare avant l'envoi (1) le canal du mandat, (2) le canal
du livrable, (3) la preuve attendue, (4) un repli réellement disponible et (5) une limite de tours. Elle
s'arrête sur l'un de trois états : **livrable prouvé**, **blocage prouvé après repli** ou **limite
atteinte**. Une boucle ne prolonge jamais une délégation sans nouvel état observable.

## §4 Étendre — ajouter une plateforme

**Nommage des transports — la cible ET le trajet doivent être lisibles.** Une compétence de délégation
qui ne nomme que le cerveau masque son canal et finit par être utilisée dans le mauvais contexte. Famille
actuelle : `ia-delegation-local-to-local` · `ia-delegation-local-to-gpt-chrome` ·
`ia-delegation-codex-to-claude-desktop` · `ia-delegation-browser-to-browser`, plus `claude-codex` pour la
cible Codex. Une voie non prouvée porte son statut expérimental dans son contrat ; elle n'emprunte pas
l'autorité d'une voie voisine.

Nouvelle plateforme avec une IA et des crédits → (1) créer une **compétence de transport** qui nomme la
cible et le trajet (avec ton outil habituel de création de compétences), (2) ajouter **une branche** dans
le tableau §1 et dans le §2, (3) ajouter **son mode** au bloc 🎛️ et à l'`argument-hint` de l'en-tête.

**Ce chapeau AMÉLIORE aussi ses transports** — son but n'est pas que de router, c'est de rendre la
collaboration Claude ↔ IA cible fluide dans le temps : quand un transport gagne une capacité ou bute sur
une limite, on l'affine dans **SA** compétence, et on tient ce §4 à jour de la famille.

## Compétences liées

Dans ce pack : `ia-delegation-local-to-gpt-chrome` (projet ChatGPT dans Chrome) · `claude-codex` (agent
local avec disque) · `ia-delegation-local-to-local` (commandes natives entre tâches) ·
`ia-delegation-codex-to-claude-desktop` (transcript + envoi Computer Use vers Claude Desktop) ·
`ia-delegation-browser-to-browser` (MVP par extension).

Hors de ce pack : `claude-breeze` (mode `hubspot`), dans le pack HubSpot public
<https://github.com/Gremelinn0/sales-hubspot-toolkit>. Le quota disponible de ton abonnement et ton
registre local des projets IA, s'il existe, sont propres à ton installation : ce chapeau ne suppose
jamais leur présence.

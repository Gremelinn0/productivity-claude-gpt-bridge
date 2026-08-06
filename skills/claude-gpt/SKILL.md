---
name: claude-gpt
description: >-
  Piloter un projet ChatGPT EXISTANT (Chrome MCP) pour déléguer du raisonnement/rédaction au CERVEAU
  LE MOINS CHER — ChatGPT, crédits déjà payés — plutôt qu'en tokens Claude. S'applique quand tu as
  déjà un projet ChatGPT configuré avec sa méthodologie posée DANS SES INSTRUCTIONS DE PROJET : le
  skill rejoint le fil existant ou ouvre le bon chat et fait travailler ChatGPT — il ne réimplémente
  jamais la méthode, elle vit déjà côté ChatGPT. Couvre les DEUX SENS : ALLER — faire réfléchir
  ChatGPT ; RETOUR — installer le PAQUET de sources que ChatGPT a consolidé, avec reçu. À invoquer DÈS
  QUE l'utilisateur dit "délègue à ChatGPT", "fais bosser ChatGPT", "économise mes tokens sur ChatGPT
  / GPT", "pilote mon projet ChatGPT", "ouvre un chat ChatGPT pour X", "claude-gpt", ou décrit une
  tâche de recherche/rédaction/brainstorm qu'un projet ChatGPT déjà configuré peut faire à sa place —
  même sans le mot "délégation". Côté RETOUR, les déclencheurs ne valent QUE dans un contexte ChatGPT
  (paquet/zip annoncé, conversation collée, projet nommé) : "ChatGPT m'a généré un zip / un paquet",
  "j'ai cliqué sur télécharger", "va chercher le paquet", "fais-le en autonome", "installe le paquet
  de synchronisation", "README-SYNCHRO", "donne ça à Claude", et "mets à jour les sources" /
  "synchronise le projet" / "intègre ça dans la référence" QUAND les sources viennent de ChatGPT. PAS
  pour créer un nouveau projet ChatGPT, ni pour une tâche qui exige le filesystem ou les connexions
  locales de Claude.
---

# Skill — claude-gpt (déléguer vers ChatGPT)

## §0 Le principe — arbitrage de tokens

Un projet ChatGPT que l'utilisateur a déjà configuré porte des crédits **déjà payés** et, surtout, une **méthodologie déjà écrite dans ses instructions de projet**. Le refaire réfléchir en tokens Claude serait payer deux fois la même réflexion. Ce skill fait de Claude l'**opérateur** de ce projet — il ouvre, nomme le sujet, relit — jamais le cerveau qui rédige à la place de ChatGPT.

C'est une déclinaison du patron **« opérateur = agent »** appliqué à ChatGPT (convention `claude-<cible>`) : le même patron se décline pour n'importe quelle IA tierce pilotable par navigateur. Ce pack embarque la déclinaison ChatGPT, qui se suffit à elle-même.

**La collaboration a deux sens, et le skill couvre les deux** :

| Sens | Ce qui circule | Où c'est décrit |
|---|---|---|
| **ALLER** — Claude → ChatGPT | une question, un sujet à traiter ; ChatGPT réfléchit et rédige | §2 à §6 |
| **RETOUR** — ChatGPT → Claude | du **durable** consolidé (une référence, un chiffrage, une décision) qui doit remplacer les sources actives | **§7** |

Sans le retour, la collaboration fuit : ChatGPT produit du bon travail qui reste bloqué dans un fil de chat, pendant que le dépôt et le projet ChatGPT continuent de tourner sur des sources périmées — chacun croyant lire la bonne version.

## §1 Quand l'utiliser — le décideur

| Situation | Action |
|---|---|
| Un projet ChatGPT dédié **existe déjà**, avec la méthode dans ses instructions de projet | **Ce skill** — Claude pilote, ne réexplique jamais la méthode dans ses messages |
| Aucun projet ChatGPT n'existe encore pour ce sujet | **Hors scope** — la création d'un projet ChatGPT est une décision de l'utilisateur, jamais une initiative de Claude (cf §5) |
| La tâche a besoin du filesystem local, d'une connexion que seul Claude a, ou d'un jugement fin sur ton code/tes fichiers | **Pas ce skill** — Claude le fait directement, c'est sa valeur ajoutée |

Au lancement, l'utilisateur (ou le contexte de la conversation) désigne le projet ChatGPT visé — Claude ne devine jamais lequel parmi plusieurs projets existants sans le demander une fois.

## §2 Mécanique de pilotage — Chrome MCP

**✅ Mécanique prouvée en conditions réelles le 2026-07-23** (détail exact, pièges et parades → **§6**, ne pas le redupliquer ici). ChatGPT n'impose pas de contrainte d'iframe imbriqué same-origin (contrairement à d'autres plateformes pilotées de la même façon) — la mécanique est donc plus simple.

> 🧭 **Sujet DÉJÀ en cours ⇒ on rejoint le fil existant, on n'en ouvre pas un de plus.** Se connecter au fil, **lire ses derniers messages** — ChatGPT annonce souvent lui-même la suite — puis continuer avec lui ; la capture ou la récupération d'un document ne vient qu'**après** avoir travaillé avec l'agent. Ça ne contredit pas « 1 sujet = 1 chat » (§3) : cette discipline interdit de **mélanger deux sujets** dans un thread, elle n'oblige pas à repartir de zéro sur un sujet déjà entamé. Rouvrir un chat neuf sur un sujet en cours jette le contexte que ChatGPT a déjà construit et le fait re-raisonner pour rien — l'inverse exact du but de ce skill.

Le principe général, confirmé par §6 :
1. **Naviguer** vers l'URL racine du projet ChatGPT ciblé (Chrome MCP — `claude-in-chrome`, pas le navigateur interne sandboxé : il faut la session ChatGPT déjà connectée de l'utilisateur).
2. **Ouvrir un chat NEUF** pour le sujet à traiter — jamais continuer un thread existant pour un sujet différent (cf §3 — 1 question = 1 thread neuf). Plusieurs sujets indépendants → paralléliser via des onglets séparés plutôt qu'enchaîner en séquentiel (§6, gain de temps réel).
3. **Écrire** le message : `computer type` dans le composer puis **`Return` en action séparée**, jamais enchaînés sans vérification — un `screenshot` avant l'envoi confirme que le texte a bien atterri (§6 détaille les deux pièges rencontrés : coordonnées qui varient d'un onglet à l'autre, texte dupliqué après une reconnexion).
4. **Lire** la réponse : poller le texte de la page jusqu'à stabilité (la génération s'arrête de grossir), comme pour Breeze. Pour un document canvas (fichier généré affiché en panneau latéral), `get_page_text` ne le capture pas — demander à ChatGPT de le reposter en texte brut dans le chat (§6).
5. **Boucler** sur le sujet suivant → répéter depuis l'étape 2 avec un chat neuf.

**Ce qui rend ChatGPT particulièrement adapté** : certaines plateformes n'autorisent aucune instruction persistante au niveau du compte, et obligent à réinjecter la méthode par prompt à chaque fois. Un **projet ChatGPT**, lui, porte des **instructions durables au niveau du projet** : une fois posées par l'utilisateur, chaque nouveau chat DANS ce projet en hérite automatiquement. Claude n'a donc jamais à réinjecter/réexpliquer la méthode dans ses messages de lancement — nommer le sujet suffit. C'est ce qui rend cette déclinaison mécaniquement simple, sans la rendre moins rigoureuse.

## §2bis LIRE et ÉCRIRE par deux canaux différents

Lire et écrire ne se font pas par le même canal. Les deux gestes n'ont
ni la même fréquence, ni le même risque, ni le même coût — les faire par le même canal fait payer à
chacun le défaut de l'autre. Ça **remplace l'étape 4 du §2** (« poller le texte de la page ») quand
un pont de session est disponible.

### Lire — le canal le plus étroit gagne, et de loin

`get_page_text` rend **toute la page**. Sur un projet dont les instructions sont longues, il ramène
le bloc de consignes épinglé **à chaque lecture** : mesuré sur un vrai fil, environ 5 000 mots
rapatriés trois fois de suite pour en extraire trois phrases utiles.

**S'il existe sur ta machine un pont de session** — une application locale qui suit les onglets IA et
sait rendre une conversation — il gagne systématiquement : il rend *la dernière réponse*, entière
(vérifié sans troncature sur un message de 9 609 caractères), sans l'interface autour.

> 🖥️ **Ce pont n'est PAS fourni par ce plugin, et n'est pas requis.** Si tu disposes d'un outil local
> qui expose l'inventaire de tes onglets IA et le contenu d'une conversation, branche-le ici : c'est
> l'accélération la plus rentable de tout ce skill. **Sinon, tout fonctionne quand même** — on lit
> par `get_page_text` et on paie la page entière.

### Le capteur — savoir QUAND elle a fini, sans deviner

Une réponse structurée prend couramment 3 à 6 minutes (§6). Deviner l'instant où elle est prête,
c'est soit poller la page (cher), soit attendre trop (lent).

Un pont de session porte en général un **état par session** (terminé / en cours) : on attend l'état
terminé, on lit **une** fois. Sans pont, l'équivalent visuel est le bouton d'envoi qui redevient un
bouton d'arrêt pendant la génération.

### Ne jamais écrire pendant qu'elle génère

Un message envoyé en pleine génération se fait au mieux mettre en file, au pire avaler par le tour en
cours. **Le geste** : composer le message dans le champ, **vérifier l'état**, envoyer quand elle a
fini. Le texte attend très bien dans le composer — c'est l'envoi qui doit attendre, pas la rédaction.

### Écrire — trois pièges concrets, tous rencontrés en réel

1. **Le multi-ligne envoie le message.** Un saut de ligne dans un `type` déclenche l'envoi et le
   message part en morceaux. Utiliser **`shift+Return`** entre les segments, et enchaîner
   `type` → `shift+Return` → `type` … dans **un seul `browser_batch`** : un message de 2 500
   caractères passe ainsi en un appel, proprement paragraphé.
2. **Une déconnexion en pleine frappe laisse du texte orphelin.** Chrome MCP se reconnecte tout seul
   (deux fois dans une même session de pilotage), mais le champ garde ce qui avait commencé à
   s'écrire, et la frappe suivante **s'ajoute** au lieu de remplacer. Après toute déconnexion :
   `ctrl+a` puis `Delete` **avant** de retaper. Les refs d'éléments survivent à la reconnexion, le
   contenu du champ non.
3. **Relire avant d'envoyer.** `type` et `Return` restent deux actions séparées, avec une capture
   entre les deux (§6). L'envoi est le seul geste irréversible de la boucle.

### Deux limites d'un pont, à connaître avant de s'y fier

- **Une même conversation ouverte dans deux onglets compte pour deux sessions.** Un inventaire par
  onglet ne sait pas qu'il regarde deux fois la même chose. Dédupliquer sur l'URL de conversation,
  jamais sur l'onglet.
- **Le canvas reste invisible d'un pont aussi**, pas seulement de `get_page_text` (§6). Un document
  généré en panneau latéral n'est ni dans le flux de conversation, ni dans ce que rend le pont : le
  demander en texte brut dans le fil, comme décrit au §6.

## §2ter TOUT MESSAGE FINIT PAR UNE DEMANDE — sinon la boucle MEURT

**Le défaut** : on envoie un **compte rendu** (« voilà ce que j'ai fait, voilà les preuves »). ChatGPT répond *« bien reçu, rien à exécuter »*… et **plus rien n'avance**. L'utilisateur doit relancer à la main — exactement le travail que ce skill existe pour lui éviter.

**La règle** : le **dernier paragraphe** de tout message est une **DEMANDE actionnable, unique et précise** — quoi produire, avec quelles bornes, sous quelle forme. Un message qui ne demande rien n'est pas un message, c'est un accusé de réception : il coûte un tour pour rien.

**Les 3 formes qui tuent la boucle** :
1. **Le compte rendu sec** — il ne reste qu'à acquiescer.
2. **L'interdiction en dernier mot** (« ne refais pas X ») : consigne **négative**, aucun geste suivant. Si une interdiction est nécessaire, elle se met **au milieu**, jamais à la fin.
3. **La demande vague** (« dis-moi ce que tu en penses ») — elle rend un avis, pas un livrable.

**Le patron qui marche** :
> *« À toi : \<UN livrable nommé\>. Contraintes : \<les bornes\>. Rendu attendu : \<forme exacte\>. »*

**Avant de demander — le test d'efficience** : *ce que je m'apprête à lui demander, est-ce que je ne l'ai pas déjà ?* Si le protocole est **déjà écrit** dans ce qu'il a produit, on ne le lui redemande pas : on l'**exécute**, et on lui demande la seule chose qu'il est **seul** à pouvoir produire (un arbitrage, une décision de conception, une pièce absente).

> 🔍 **La question jumelle, celle qui coûte le plus cher : ce lot, ne l'a-t-il pas DÉJÀ LIVRÉ ?** Son dernier message dit ce qu'il **comptait** faire, pas ce qu'il a **fait depuis** — et ces deux états divergent dès qu'il travaille entre deux de tes tours, ce qui est le cas normal. **La complétion se lit là où le travail atterrit** (le dépôt, le dossier de sortie, l'historique des commits), **jamais dans le fil de conversation**. Un fil décrit une intention, pas un état. Confier un lot déjà livré coûte double : il refait le travail, et le tour est perdu.

## §2quater 🔁 RIEN NE REVIENT ⇒ ON RECYCLE L'ONGLET. PAS D'ANALYSE.

**La règle, et il n'y en a pas d'autre** : le message paraît parti (champ vidé, bulle affichée) mais **rien ne revient** ⇒ **recycler l'onglet immédiatement**, puis renvoyer. On ne diagnostique pas, on ne mesure pas, on n'attend pas.

**Pourquoi c'est une règle et pas un jugement** : l'envoi est **avalé silencieusement** — l'interface affiche le message, il n'atteint pas le serveur, et au rechargement il a disparu. Ça dépend de la machine et surtout **des réglages du navigateur** (onglets mis en veille pour économiser la mémoire), donc c'est **fréquent et normal**, pas une anomalie à instruire. Chaque analyse est un aller-retour perdu.

**LE GESTE — fermer l'ANCIEN, sinon on paie deux fois la mémoire** :
1. créer un onglet → 2. y ouvrir l'URL de la conversation → 3. **fermer l'ancien onglet** → 4. vider le champ de saisie (un brouillon fantôme y est souvent restauré) → 5. réécrire → 6. envoyer par **clic sur la flèche** → 7. capture de contrôle.
⚠️ L'identifiant d'onglet change → relire le contexte des onglets avant toute action suivante. Et **fermer l'ancien n'est pas optionnel** : un onglet d'IA laissé ouvert continue de consommer ; fermé puis rouvert, il repart bien plus léger.

**⏱️ LE SEUIL — après UNE occurrence dans la session, recyclage SYSTÉMATIQUE.** Plus de « peut-être que cette fois ça passera » : dès que ce symptôme est apparu une fois, **chaque envoi suivant se fait dans un onglet frais**. C'est moins cher qu'un seul tour perdu à comprendre.

**Rafraîchir marche parfois — on ne s'en contente pas.** Fermer/rouvrir est plus sain et règle aussi la mémoire. Au doute : recycler.

- ❌ Poller la conversation pendant plusieurs minutes pour voir si la réponse arrive.
- ❌ Lire des journaux, compter les messages, chercher la cause.
- ❌ Demander à l'utilisateur de renvoyer — c'est l'atelier de l'agent, pas le sien.

> 💡 La commande de setup du pack (`/claude-gpt-bridge-setup`) demande à l'utilisateur si sa machine et son navigateur sont dans le cas à risque, pour savoir s'il faut recycler **d'emblée** plutôt qu'après le premier incident.

## §3 Opérateur = agent

Le modèle : **Claude = les mains + le gardien de la discipline** (ouvre le bon projet, respecte 1 sujet = 1 chat, ne mélange jamais deux sujets dans le même thread, relit correctement) — **ChatGPT = le cerveau** (il réfléchit, rédige, structure). Claude ne se substitue jamais à lui : si une réponse ChatGPT est faible, la corriger passe par relancer ChatGPT avec plus de contexte, pas par écrire soi-même le contenu à sa place.

> 🎚️ **Ces formulations décrivent le régime de PRODUCTION DÉLÉGUÉE, pas toute la coopération.** « Claude = les mains », « ChatGPT = le cerveau », « Claude ne se substitue jamais » sont exactes quand ChatGPT **produit un livrable** (recherche, rédaction, consolidation) que Claude opère et installe : là, réécrire à sa place détruit exactement la valeur qu'on est allé chercher. Dès que les deux agents font avancer **un même chantier**, la répartition change — et c'est le **§3bis** qui la nomme.

## §3bis Les DEUX MODES de coopération — quand on ne délègue plus une production, mais un LOT d'un chantier commun

**Le basculement** : Claude et GPT travaillent sur le **même objectif durable** (un plan de développement, une version à finir, une architecture à trancher) plutôt que sur une commande ponctuelle. Claude cesse d'être un relai : il pilote la progression, révise le rendu et tranche. Deux contrats nomment ce qui change — **un seul est actif à la fois**.

| Mode | Ce qui est en jeu | Qui pilote quoi | Contrat |
|---|---|---|---|
| **DÉLÉGATION PILOTÉE** — *le défaut pour avancer* | un lot déjà cadré, dont la sortie attendue est identifiable | Claude pilote la **progression générale** ; GPT pilote intellectuellement **et** opérationnellement **le lot reçu**, jusqu'au prochain vrai gate | [`references/delegation-pilotee.md`](references/delegation-pilotee.md) |
| **RÉFLEXION CROISÉE** — *l'exception* | une contradiction ou un choix structurel réel, pas encore tranché | partenaires intellectuels : chacun peut contredire, aucune conclusion ne vaut par son auteur | [`references/reflexion-croisee.md`](references/reflexion-croisee.md) |

**Charger le contrat du mode ACTIF, jamais les deux.** Ils attribuent les mêmes décisions à des propriétaires différents ; les tenir ensemble produit un agent qui redemande l'avis de GPT sur un plan déjà décidé — le brainstorming perpétuel que la délégation pilotée existe précisément pour éviter.

**Le mode s'annonce, il ne se devine pas** : le message qui l'ouvre nomme le rôle et le mode (chaque contrat donne son patron d'invocation). Un bug ordinaire, un test rouge reproductible ou une correction locale **ne changent pas le mode** — seule une hypothèse structurante réellement remise en cause fait basculer en réflexion croisée, et on en ressort par une décision ou un lot exécutable.

> ⚖️ **Ce qu'un mode NE lève PAS** : toute la plomberie ci-dessus reste entière — demander la cible avant d'écrire (§2), lire par le canal le plus étroit (§2bis), finir chaque message par une demande actionnable (§2ter), recycler l'onglet quand rien ne revient (§2quater), 1 sujet = 1 chat (§5). Un mode change **qui décide** ; il ne change jamais **comment on écrit dans l'onglet**.

**Où ça sert** : c'est la couche de coopération qu'utilise une **campagne de développement multi-lots** — la compétence qui pilote la campagne choisit le mode et le propriétaire de chaque lot, puis s'appuie sur ces deux contrats sans les redupliquer. Si ton projet n'a pas de compétence de campagne, les deux contrats se tiennent très bien seuls : ils décrivent une **relation de travail**, pas un outillage.

## §4 Clôture — logue là où le contexte appelant suit déjà son travail

Ce skill n'impose **pas** de registre fixe. En clôture, logue le chat/projet ChatGPT utilisé dans **le mécanisme de suivi que le contexte appelant utilise déjà** :
- un fichier de suivi **généré par script** si le contexte en a un (ex un atelier qui génère son propre état — ne jamais l'éditer à la main, toujours via son script) ;
- sinon un registre des plateformes IA (colonnes : Plateforme · Projet/Chat · URL · Créé par · Sujet traité · Statut) si le projet courant en a un ;
- sinon, une simple mention dans le fil de la tâche suffit — ne pas inventer un nouveau registre par défaut.

**Avant** d'ouvrir un nouveau chat → vérifier si ce sujet a déjà été traité (le mécanisme de suivi du contexte appelant le dit) pour ne pas le refaire.

## §5 Garde-fous

- **Relai read-only par défaut** : Claude transmet, il ne décide pas du contenu produit par ChatGPT. *Ce défaut vaut pour la **production déléguée** (§3). Sous un mode de coopération (§3bis), Claude révise le rendu et tranche — mais son autorité reste bornée par le contrat du mode actif, jamais illimitée : réviser un delta n'est pas réécrire le lot à la place de GPT.*
- **Jamais toucher un livrable final** (site en prod, document publié) directement depuis ce skill — ça reste derrière la revue humaine de la compétence appelante (celle qui a le contexte métier pour juger).
- **Jamais créer un nouveau projet ChatGPT** sans demande explicite de l'utilisateur — ce skill pilote l'existant, il n'improvise pas une nouvelle structure.
- **1 sujet = 1 chat, toujours** — mélanger deux sujets dans un même thread casse la lisibilité pour l'utilisateur ET pour ChatGPT (contexte pollué).

## §6 Cas inaugural

Premier run réel : 2026-07-23, sur un projet ChatGPT dédié à la refonte d'un site, piloté depuis une session de travail Claude Code. Mécanique prouvée :

- **Ouvrir un chat neuf** : aller sur l'URL racine du projet (`https://chatgpt.com/g/g-p-<id>-<slug>`, sans `/c/...`) — le composer « New chat in \<Projet\> » y est directement ; taper et envoyer y crée automatiquement un nouveau chat.
- **Paralléliser plusieurs sessions** : `tabs_create_mcp` (Chrome MCP) une fois par session, puis répéter navigation+saisie dans chaque onglet. Indispensable en pratique — une réponse ChatGPT structurée sur ce projet prend couramment 3 à 6 minutes (« Worked for Xm » observé plusieurs fois) ; enchaîner N sessions en séquentiel coûte N fois cette attente, en parallèle une seule fois.
- **Piège coordonnées** : la position du composer varie selon l'état de charge de la page (sidebar ouverte/fermée, largeur de fenêtre) et diffère d'un onglet à l'autre. Toujours `screenshot` juste avant de cliquer — ne jamais réutiliser en dur les coordonnées d'un onglet précédent.
- **Piège double-frappe après déconnexion** : si Chrome MCP se déconnecte pendant un `type` puis se reconnecte (arrive en cours de session, transitoire — retenter directement), du texte partiel peut être resté dans le champ ; le `type` suivant s'AJOUTE au lieu de remplacer, produisant un message corrompu/dupliqué. Réflexe : `ctrl+a` puis `Delete` avant de retaper si la connexion a été instable entre-temps, puis **screenshot de vérification avant d'envoyer** — jamais enchaîner `type` puis `Return` sans relire, l'envoi (`Return`) est une action séparée.
- **Lire un document canvas** (fichier `.md` généré par ChatGPT, affiché en panneau latéral) : `get_page_text` standard NE capture PAS le contenu du canvas (autre région DOM que `<main>`). Scroller+zoomer dedans marche mais coûte cher en aller-retours. Méthode fiable et rapide : demander à ChatGPT, en message normal, de « reposter le contenu intégral en TEXTE BRUT dans ta réponse ici (pas en canvas) » — `get_page_text` le capture ensuite en un seul appel, intégralement.
- **Sources du projet ≠ fichiers créés dans un chat** : un document généré (canvas) dans un chat n'est PAS automatiquement visible par les autres chats du même projet tant qu'il n'est pas ajouté explicitement à l'onglet **Sources** de la racine du projet. Vérifier cet onglet avant de supposer un contexte partagé entre sessions.
- **Bouton téléchargement du canvas** (icône ⬇ en haut à droite du panneau) : un fichier croisé **au passage**, que l'utilisateur n'a pas demandé, se demande avant de cliquer — piège rencontré une fois ce run (clic spontané), à ne pas reproduire. ⚠️ **Distinct du paquet de synchronisation** : là, le téléchargement EST la tâche que l'utilisateur a demandée, et il se fait sans redemander (§7.2, voies A et B). Ce qui sépare les deux cas n'est pas la nature du fichier, c'est de savoir si l'utilisateur l'a réclamé ou si Claude l'a rencontré.
- **Panne large, distincte de la déconnexion simple** : après avoir ouvert ~8-9 onglets ChatGPT en parallèle, `screenshot`/`get_page_text` se sont mis à échouer (« Script injection timed out » / « Page still loading ») sur TOUS les onglets, y compris des onglets anciens et inactifs — alors que `tabs_context_mcp` (léger, pas d'injection de script) continuait de répondre normalement. Signal distinctif : si le listing de tabs marche mais que toute action qui lit/screenshotte la page échoue partout, ce n'est pas un onglet en particulier qui déconne — c'est l'injection de content-script elle-même qui est engorgée (peut-être par le nombre d'onglets). Plusieurs relances espacées (8s, 15s, 20s) n'ont pas suffi à la faire repartir dans la fenêtre observée. Réflexe : ne pas insister en boucle, réduire le nombre d'onglets simultanés si le pilotage doit recommencer, et si ça persiste au-delà de 2-3 tentatives espacées, le dire clairement à l'utilisateur plutôt que de forcer.

Discipline de mise à jour : quand ce skill gagne une capacité ou bute sur une limite en usage réel, on l'affine **ici**, pas ailleurs — c'est ce fichier qui porte la mécanique.

## §7 Le RETOUR — installer un paquet de sources produit par ChatGPT

ChatGPT ne met jamais rien à jour lui-même : il vit dans un onglet, sans accès au dépôt ni au dossier partagé. Il **produit un paquet** ; **Claude est le seul à pouvoir l'installer**. Tout le protocole ci-dessous existe parce qu'une désynchronisation ne se voit pas — chaque bout continue de tourner en croyant lire la bonne version, et l'erreur ne se découvre qu'au premier calcul faux.

### 7.1 Exiger un paquet COMPLET — jamais reconstruire depuis le fil

Avant de télécharger quoi que ce soit, vérifier que ChatGPT a bien produit une **archive autosuffisante** : les fichiers canoniques **entiers**, un `_MANIFESTE.json` régénéré, une note de version. S'il a seulement collé du texte dans le chat, **le lui redemander** — un fichier reconstitué depuis un extrait de conversation a l'air juste et a perdu des passages en silence, ce qui est pire qu'une absence de mise à jour. C'est exactement le rôle que Claude tient déjà côté ALLER (§3, gardien de la discipline), appliqué à la production de ChatGPT.

### 7.2 Récupérer — trois voies, de la plus simple à la plus autonome

**Le paquet s'auto-identifie.** Son `_MANIFESTE.json` porte, pour chaque fichier, son chemin de destination exact. Une fois le zip en main, il n'y a plus rien à demander : le projet, le bundle et la destination se lisent dedans. C'est ce qui permet de dérouler tout seul — demander « c'est quel projet ? » alors que la réponse est dans le fichier, c'est faire relire à l'utilisateur ce qu'on a déjà sous la main.

**Voie A — l'utilisateur clique, Claude déroule** *(le défaut, et le plus simple)*
Il clique sur le lien dans ChatGPT. Claude prend le fichier **le plus récent des Téléchargements**, lit le manifeste, reconnaît le projet, et enchaîne — **sans aucune question**. C'est la voie la plus rapide parce qu'elle sépare proprement les rôles : un clic humain, tout le reste automatique.

**Voie B — Claude va le chercher** *(quand l'utilisateur demande le mode autonome)*
Claude connaît la session ChatGPT visée : Chrome MCP, ouvrir la conversation, lire le **dernier message**.
- Lien de téléchargement présent → cliquer, puis annoncer le nom du fichier une fois arrivé.
- Aucun lien → **réclamer le paquet à ChatGPT** dans le fil (§7.1), attendre qu'il le génère, puis cliquer.

La demande de l'utilisateur (« va chercher le paquet », « fais-le en autonome ») **est** l'autorisation de ce téléchargement-là. Elle vaut pour le paquet visé, pas pour les autres fichiers croisés en chemin — un bouton de téléchargement rencontré au passage reste sous la règle du §6.

**Voie C — le contexte manque**
Rien dans les Téléchargements et aucune session identifiable → **deux questions, pas plus** : sur quel projet on travaille, et ce qu'il veut en faire. Les poser coûte dix secondes ; deviner coûte une installation au mauvais endroit, qu'on ne repère qu'au premier calcul faux.

Dans les trois cas, décompresser dans un dossier temporaire — jamais directement par-dessus les fichiers actifs.

### 7.3 Vérifier AVANT d'écraser — c'est le manifeste qui tranche

Comparer `_MANIFESTE.json` entrant et fichiers actifs sur **empreinte + taille + date**. Un paquet **plus ancien ou plus petit** que l'actif n'est pas forcément faux, mais il n'est jamais anodin : le signaler avec les deux chiffres et attendre l'arbitrage, plutôt que de rétrécir une référence en silence.

> Vu en réel le 2026-07-26 : paquet entrant `ba74a5b8…` / 12 ko contre actif `93fb0f13…` / 13 ko. Installé à l'aveugle, ça retire un kilo-octet de référence produit sans que personne ne le sache.

### 7.4 Le nom aplati est une ADRESSE, pas un nom de fichier

Un fichier de paquet nommé `claude_skills_mon-skill_MA-REFERENCE.md` n'est pas un fichier à poser tel quel : le préfixe encode son **chemin de destination**. Le champ `source` du manifeste donne le vrai chemin (`.claude/skills/mon-skill/MA-REFERENCE.md`) — **le lire, ne jamais déduire du nom**. Poser le nom aplati dans le dépôt crée un fichier fantôme : il existe, il a l'air installé, et aucun consommateur ne le lit jamais.

### 7.5 Les TROIS bouts — en sauter un suffit à tout désynchroniser

| Bout | Geste | Qui en est propriétaire |
|---|---|---|
| **Le dépôt** (source de vérité) | écrire au chemin réel du manifeste, puis commit — le commit EST la sauvegarde, rien à supprimer à la main | le dépôt concerné |
| **Le dossier partagé que ChatGPT lit** | **ne jamais l'éditer à la main** : il est *généré*. Mettre à jour le dépôt, puis relancer son script de build | **le script de build de ton dossier partagé** (ex `py tools/build_gpt_bundle.py --bundle <nom>`, alimentant un dossier local que tu téléverses dans ChatGPT) |
| **L'onglet Sources du projet ChatGPT** | Chrome MCP : retirer l'ancienne version, téléverser la nouvelle sous son nom canonique | ce skill |

Le bout du milieu est celui qu'on saute — et l'éditer à la main est pire que l'oublier, parce que le prochain `--check` du script annonce alors « à jour » sur un dossier bricolé. Rappel §6 : un document créé en canvas dans un chat n'est **pas** visible des autres chats du projet tant qu'il n'est pas dans **Sources**.

**Aucune ancienne copie ne reste active** : un `fichier (1).md` ou deux `.zip` du même jour dans les Téléchargements, c'est la désynchronisation qui se prépare — les doublons partent dans un dossier d'archive daté, jamais à la corbeille (règle globale : on archive, on ne supprime pas).

### 7.6 Le reçu clôt la synchronisation

Finir par un reçu court qui dit, pour chaque bout : ce qui a été remplacé, où, et ce qui **ne l'a pas été**. Un paquet à moitié installé sans que ça se voie est le seul vrai échec de ce protocole — d'où la ligne « éléments non appliqués », qui vaut mieux vide que absente.

```text
Paquet : <nom> · <date> · <empreinte>
Dépôt : <fichiers> → <chemins> · commit <sha>
Dossier partagé : régénéré via <script> ✅ / non fait
Projet ChatGPT : <fichiers remplacés> · doublons retirés : <…>
Non appliqué : aucun | <action manuelle précise>
```

Et avant tout calcul important qui s'appuie sur ces sources, **dire quelle version a réellement été lue** (empreinte ou date) — un raisonnement juste sur une source périmée reste faux, et c'est indétectable dans la réponse.

### 7.7 Si Chrome MCP bloque

Ne jamais s'arrêter à « je ne peux pas ». Faire **tout ce qui reste possible** (décompresser, installer le dépôt, régénérer le dossier partagé), puis donner à l'utilisateur **une seule manipulation précise** : quel fichier, où le déposer, quel ancien remplacer, comment vérifier. Une manip précise se fait en trente secondes ; un « ça n'a pas marché » coûte une session. Chrome MCP mort dans cette session mais vivant ailleurs → router le travail vers une autre session plutôt qu'insister.

## Skills liés

Ce skill **se suffit à lui-même** — aucun des skills ci-dessous n'est requis pour l'utiliser.

- **Un skill qui possède ton dossier partagé** (celui qui génère le bundle que ChatGPT lit dans ses Sources) : c'est lui qu'on appelle au bout n°2 du §7, jamais une copie à la main. Si tu n'en as pas, le §7.5 se réduit à deux bouts au lieu de trois.
- **Un orchestrateur d'arbitrage de tokens**, si tu délègues aussi à d'autres IA : il route vers ce skill quand le contexte est ChatGPT.
- **Une compétence de routage de sessions**, utile pour trouver ou ouvrir la session qui porte le sujet à déléguer.

**Sources de contenu** — §7 vient d'un protocole rédigé par ChatGPT lui-même le 2026-07-26 (`README-SYNCHRO.md`, livré dans un paquet), **absorbé et corrigé ici** sur trois points vérifiés en réel : le nom aplati est une adresse (7.4), le dossier partagé est généré et non éditable à la main (7.5), le téléchargement se demande avant de cliquer (7.2). Le protocole d'origine décrivait deux bouts ; il y en a trois.

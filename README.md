# claude-gpt-bridge — la délégation IA entière pour Claude Code

Confier le **gros du travail** (lire beaucoup, écrire long, comparer, toucher des fichiers sur plusieurs
dépôts) au **cerveau le moins cher qui sait le faire**, et **garder le jugement chez Claude**. Un chapeau
choisit le bon cerveau ; des transports l'atteignent, **prouvent que le message est parti**, relisent ce
qui revient et rendent un reçu.

> Version 0.2.0. Ce pack s'appelait « claude-gpt-bridge » quand il ne pilotait que ChatGPT : il garde son
> nom pour que ton installation ne casse pas, et couvre maintenant toute la délégation (voir
> « Si tu venais de la 0.1.0 » en bas).

## Le principe

Un projet ChatGPT que tu as configuré porte déjà une méthode et des crédits payés ; Codex a le disque et
sait lancer des builds ; une autre session a déjà le contexte et la connexion. Refaire raisonner Claude à
leur place, c'est payer deux fois la même réflexion. Ce pack fait de Claude **l'opérateur** : il choisit,
mandate, vérifie, relit — et ne réécrit jamais le travail de l'autre. Il ne délègue pas ce qui perdrait à
l'être : un jugement fin, une décision, une donnée sensible, ni une tâche plus courte que son propre
handoff.

**Une délégation, en six gestes** :

1. tu dis « délègue ça » (ou tu forces la cible : `/claude-ia-delegation gpt`) ;
2. le chapeau `claude-ia-delegation` décide **qui** doit faire, sans te poser la question ;
3. le transport rejoint la **bonne conversation existante** (jamais un fil neuf par mandat) ;
4. il envoie **un mandat borné** et prouve l'envoi : champ vide **et** nouveau tour visible après
   rechargement ;
5. il attend, puis **mesure les faits** que l'autre avance (il ne relit pas son raisonnement) ;
6. il rend un **reçu** : cible pilotée, demande envoyée, résultat, emplacement de la livraison,
   vérification faite, reste éventuel.

## Les modes — un mot après la commande choisit la cible

`/claude-ia-delegation` seul : le chapeau choisit. Avec un mot, la cible est forcée (la liste s'affiche
quand tu tapes la commande).

| Mode | Cible | Compétence | Ce qu'il faut avant |
|---|---|---|---|
| `gpt` | un projet **ChatGPT** existant, dans Chrome, en mode Chat | `ia-delegation-local-to-gpt-chrome` | l'extension **Claude in Chrome** connectée, ta session ChatGPT ouverte, un projet ChatGPT qui porte déjà sa méthode, un abonnement qui donne accès aux projets |
| `codex` | **Codex**, l'agent OpenAI local qui a le disque | `claude-codex` | **Codex CLI** installé et connecté (`codex login status`) |
| `session` | une **autre session** locale, vivante | `ia-delegation-local-to-local` | une application qui expose des commandes natives de tâches ou de sessions (liste, lecture, envoi) |
| `navigateur` | d'un onglet web à un autre | `ia-delegation-browser-to-browser` | une extension qui sait lister, lire, écrire **et** vérifier l'envoi — **expérimental** |
| `desktop` | depuis Codex, une session **Claude Desktop** | `ia-delegation-codex-to-claude-desktop` | Windows, Codex local, Claude Desktop, l'outil Computer Use |
| `hubspot` | **Breeze**, l'IA native de HubSpot | `claude-breeze` — **absente de ce pack** | le pack HubSpot public : <https://github.com/Gremelinn0/sales-hubspot-toolkit> |
| `gemini` | Gemini | aucune : branche non câblée, le principe seulement | — |
| `claude` | personne : Claude le fait lui-même | — | — |

Un mode dont le transport manque à ton installation n'échoue pas en silence : le chapeau le dit en une
ligne, puis choisit seul.

## Les compétences du pack

| Compétence | Ce qu'elle fait |
|---|---|
| **`claude-ia-delegation`** | Le chapeau. Décide qui doit faire (capacité, contexte, coût), sépare le canal du mandat de celui du livrable, impose une limite de tours et un repli. Mince : il route, il ne ré-implémente pas. |
| **`ia-delegation-local-to-gpt-chrome`** | Pilote un projet ChatGPT existant par l'extension Chrome : bon fil, modèle et effort réglés, envoi prouvé, suivi, récupération du paquet. Garde un registre des fils. Deux modes de coopération : *délégation pilotée* (le défaut pour avancer) et *réflexion croisée* (l'exception). |
| **`claude-codex`** | Lance Codex sur des **fichiers réels** (`codex exec`, plusieurs dossiers, bac à sable), cadre le projet par `AGENTS.md`, relit ce qu'il a écrit — y compris ce qu'il aurait pris à une autre session. |
| **`ia-delegation-local-to-local`** | Observe, poursuit ou challenge une autre tâche locale par les **commandes natives** de l'application (lire un checkpoint, attendre, envoyer), sans ouvrir l'interface pour lire un statut. |
| **`ia-delegation-codex-to-claude-desktop`** | Depuis Codex, poursuit une session Claude Desktop existante : lecture par transcript, Computer Use réservé à l'envoi unique. |
| **`ia-delegation-browser-to-browser`** | Un onglet qui en pilote un autre. Un prévol de quatre capacités ; sans les quatre, il refuse de promettre le trajet. |

Plus **une commande**, `/claude-gpt-bridge-setup` (voir « Installation »).

## Installation — deux commandes

```
/plugin marketplace add Gremelinn0/productivity-claude-gpt-bridge
/plugin install claude-gpt-bridge
```

Puis, une fois :

```
/claude-gpt-bridge-setup
```

Cette commande **vérifie, mode par mode, ce qui peut réellement fonctionner chez toi** (contrôle du
navigateur, Codex CLI et sa connexion, outils de tâches, compétence HubSpot), te dit ce qui manque et
explique comment la délégation se déclenche — et pourquoi il ne s'est rien passé, le cas échéant. Chaque
ligne de son verdict vient d'une sonde en lecture seule, jamais d'une récitation. Elle n'écrit rien sur ta
machine : le pack n'a ni hook, ni agent, ni serveur MCP, et `--uninstall` te dira qu'il n'y a rien à
retirer.

## Ce que ça coûte à tes sessions — mesuré

Mesuré le 15/10/2026 par `claude plugin details claude-gpt-bridge` (Claude Code 2.1.251), estimations de
l'outil :

- **~914 jetons toujours chargés** dans chaque session : la ligne de description de chaque compétence et
  de la commande de setup, qui les rend trouvables (l'outil compte la commande avec les compétences : 7
  entrées au total pour 6 compétences et 1 commande) ;
- le corps d'une compétence ne se charge **qu'à son invocation** : de **~1 k** (navigateur vers
  navigateur) à **~5 k** (le chapeau), ~4 k pour le transport ChatGPT, ~2,3 k pour Codex, ~2,1 k pour la
  commande de setup ;
- 0 hook, 0 agent, 0 serveur MCP.

L'**économie** que la délégation procure n'est pas chiffrée ici : elle dépend de ton plan, du quota que tu
as déjà payé côté ChatGPT ou Codex, et de la tâche. Le chapeau te demande de vérifier le coût réel et le
quota dans ton plan actif plutôt que de les déduire de mots comme « natif » ou « inclus ».

## ⚠️ Setup requis de ton côté — sans ça, certains modes ne font rien

Le pack **pilote** des cerveaux existants : il ne les crée pas.

1. **Mode `gpt` — un projet ChatGPT déjà créé, avec ta méthode dans ses instructions de projet.** C'est le
   cœur : le transport ne réinjecte jamais la méthode dans ses messages, il compte sur le fait qu'elle est
   portée par le projet. Un projet vide ne donne rien d'utile. Il lui faut aussi **le contrôle du
   navigateur avec ta session ChatGPT déjà connectée** (l'extension Claude in Chrome ; le navigateur
   sandboxé interne ne convient pas) et un abonnement qui donne accès aux projets.
2. **Mode `codex` — Codex CLI installé et connecté.** Les options décrites ont été relues dans l'aide de la
   version 0.144.5 le 15/10/2026.
3. **Mode `session` — une application qui expose des commandes natives de tâches ou de sessions.** Sans
   elles, le transport se replie sur l'interface pour *écrire* seulement, jamais pour lire un statut.
4. **Mode `hubspot` — le pack HubSpot public** (voir le tableau des modes).

Optionnel, pour le sens retour de ChatGPT : un dossier partagé **généré par un script**, celui que tu
téléverses dans les Sources de ton projet ChatGPT. Sans lui, le protocole de retour se réduit à deux bouts
au lieu de trois, et le transport le dit.

## Limites — dites d'avance

- **Les modes `gpt` et `navigateur` pilotent l'interface web de ChatGPT, qui change.** Les sélecteurs, les
  libellés de menu et les comportements consignés dans `references/pannes-et-reprise.md` sont des
  **relevés datés** (août à octobre 2026), pas un contrat. Quand un geste ne produit rien, la compétence
  demande de lire l'état de l'élément avant d'accuser l'environnement.
- **`navigateur` est expérimental** : il n'est pas annoncé comme une capacité générale. Le statut tombe
  après trois cycles réels consécutifs sur deux conversations distinctes ; ce pack n'en fournit pas la preuve.
- **`desktop` est décrit pour Windows** (gestes par Computer Use) ; aucune preuve de cycle complet n'est
  jointe au pack. À éprouver chez toi avant de t'y fier.
- **`gemini` n'est pas câblé** et **`hubspot` demande un autre pack**. Ce pack HubSpot embarque sa propre
  copie, plus mince, du chapeau `claude-ia-delegation` : si tu installes les deux, elles coexistent sous
  leurs noms qualifiés par plugin (`claude-gpt-bridge:claude-ia-delegation`) ; garde celle-ci pour tous les
  modes.
- **Les registres de cibles** (fils ChatGPT, sessions, travaux Codex) sont des gabarits vides **dans les
  compétences installées** : une mise à jour du plugin les remet à zéro. Copie-les dans un fichier de ton
  projet si tu veux qu'ils survivent.
- **Les compétences sont rédigées en français.**
- Le pack ne remplace pas ton jugement : il l'exige pour décider quoi déléguer, et il impose de **mesurer**
  les faits que l'autre cerveau avance plutôt que de les croire.

## 🌍 Portabilité

**Classe : ADAPTABLE.** Aucun chemin, compte ni dépôt de l'auteur n'est en dur ; voici ce qui dépend de ton
poste :

| Point | État | Ce que tu dois adapter |
|---|---|---|
| **Chemins / comptes** | Aucun en dur | Rien |
| **Système** | Le transport ChatGPT et le chapeau ne dépendent pas de l'OS ; `desktop` et le repli d'écriture de `session` décrivent des gestes **Windows** (Computer Use) | Mac/Linux : non testé pour ces deux-là |
| **Projet ChatGPT** | C'est **toi** qui le désignes | Ton projet, ta méthode |
| **Registres** | Gabarits vides, dans les compétences | Les copier dans ton projet |
| **Dossier partagé (sens retour)** | Suppose un dossier généré par un script, jamais édité à la main | Ton script ; sans lui, deux bouts au lieu de trois |
| **Compétences voisines** | Aucune requise, hors `claude-breeze` pour le seul mode `hubspot` | Le pack HubSpot, si tu veux ce mode |

## Si tu venais de la 0.1.0

La compétence unique `/claude-gpt` s'appelle désormais `/ia-delegation-local-to-gpt-chrome` : son nom dit
la cible (GPT) et le trajet (Chrome). Son protocole de retour (installer un paquet produit par ChatGPT)
est dans `skills/ia-delegation-local-to-gpt-chrome/references/retour-paquet.md`, ses pannes et leurs
parades dans `references/pannes-et-reprise.md`, et les deux modes de coopération dans
`references/delegation-pilotee.md` et `references/reflexion-croisee.md`. Le plugin garde le nom
`claude-gpt-bridge` : la mise à jour du plugin suffit (`claude plugin update claude-gpt-bridge`).

## Sa philosophie, en une ligne

L'opérateur n'est pas le cerveau : Claude tient la discipline (une cible, on relit avant d'envoyer, on
prouve que c'est parti, on mesure avant de croire), les autres IA fournissent le volume.

---
description: Vérifie quels modes de claude-gpt-bridge peuvent réellement fonctionner ici, dit ce qui manque, et explique comment la délégation se déclenche.
argument-hint: "[--uninstall]"
---

# Setup — claude-gpt-bridge

Tu exécutes le setup de ce plugin. Trois fonctions, **dans cet ordre** : VÉRIFIER, INSTALLER, EXPLIQUER.
L'utilisateur vient de l'installer, ou bien il revient parce que « ça n'a rien fait ».

> ⚠️ **Le verdict se CALCULE, il ne se récite pas.** Chaque ligne du rapport doit venir d'une sonde que tu
> viens de lancer. Une case cochée sans mesure derrière est pire qu'une case absente : elle fait croire
> que c'est vérifié. Si une sonde est impossible ici, écris **« non vérifiable »** — c'est une réponse
> honnête, contrairement à un ✅ optimiste. **Aucune sonde n'écrit, n'envoie ni ne crée quoi que ce soit.**

## Si l'argument est `--uninstall`

Ce plugin **n'écrit rien** chez l'utilisateur : ni fichier de configuration, ni clé, ni dossier, ni entrée
dans ses réglages. Il n'y a donc **rien à retirer** — le désinstaller, c'est retirer le plugin lui-même
(`/plugin uninstall claude-gpt-bridge`). Les registres de cibles (fils, sessions, travaux) que
l'utilisateur a pu copier dans ses propres fichiers lui appartiennent : ils restent.

Dis-le tel quel, et **ne cherche pas** de fichiers à nettoyer : en inventer donnerait l'impression que le
plugin en avait posé.

## 1 — VÉRIFIER (sonder, pas supposer)

**a) Les compétences sont-elles bien là ?** Liste le contenu de `skills/` dans le répertoire du plugin. Le
nombre et les noms trouvés sont le verdict — ne les recopie pas depuis ce fichier, ils mentiraient le jour
où le pack changera.

**b) Chaque mode, sondé pour de vrai.** Un mode n'est « prêt » que si sa sonde a répondu :

| Mode | Ce qu'il lui faut | Sonde (lecture seule) |
|---|---|---|
| `gpt` | le contrôle du navigateur avec la session ChatGPT **déjà connectée** (extension Claude in Chrome), un projet ChatGPT configuré avec sa méthode, un fil existant | tenter de lister les onglets ; si l'utilisateur a donné un fil, l'ouvrir et lire son titre et ses derniers tours — **sans rien écrire** |
| `codex` | Codex CLI installé et connecté | `codex --version`, puis `codex login status` |
| `session` | une application qui expose des commandes natives de tâches ou de sessions (liste, lecture, envoi) | chercher ces outils dans la session ; en appeler un en lecture seule (lister) |
| `navigateur` | une extension qui sait lister, lire, écrire **et** vérifier l'envoi (expérimental) | le prévol de `ia-delegation-browser-to-browser` jusqu'à l'étape 2 (lister les onglets, lire un élément connu) — **sans écrire** |
| `desktop` | Windows, Codex local, Claude Desktop, l'outil Computer Use | présence de Computer Use dans la session ; **ne rien ouvrir ni envoyer** — sinon « non vérifiable » |
| `hubspot` | le pack HubSpot public (compétence `claude-breeze`) | `claude-breeze` figure-t-elle dans les compétences disponibles ? |
| `gemini` | rien : branche non câblée | — |

Pour le navigateur, trois issues, et elles ne se confondent pas :

| Ce que tu observes | Verdict | Ce que ça veut dire |
|---|---|---|
| Les outils de navigateur n'existent pas dans cette session | ❌ **Bloquant** pour `gpt` et `navigateur` | Le plugin ne pourra rien piloter. Il faut l'extension Claude pour Chrome. |
| Les outils existent mais aucune connexion au navigateur | ⚠️ **À brancher** | L'extension est là mais pas connectée — l'utilisateur doit ouvrir Chrome et l'autoriser. |
| Tu obtiens la liste des onglets | ✅ **Opérationnel** | C'est la seule preuve qui vaut. |

⚠️ **Le navigateur interne sandboxé ne convient pas** : il faut la session ChatGPT **authentifiée** de
l'utilisateur. Si tu ne peux sonder que celui-là, dis-le — ne compte pas ça comme un succès.

**c) Le projet et le fil existent-ils ?** (mode `gpt`) La cible est une conversation existante dans un
projet ChatGPT, pas une page d'accueil. Demande son lien ou son nom précis. S'il n'existe encore aucun
projet configuré, c'est le point bloquant. Si le contrôle du navigateur répond, ouvre le fil et vérifie
son titre, son projet et ses derniers tours avant toute écriture.

## 1bis — CALIBRER LA FIABILITÉ D'ENVOI

Le seul test utile est un test de preuve, pas un questionnaire sur la machine :

1. ouvrir le fil exact ;
2. lire son dernier tour ;
3. composer un court message de contrôle seulement si l'utilisateur l'a demandé ;
4. après l'envoi, vérifier **deux témoins** : composer vide et nouveau tour utilisateur visible, ce
   dernier **après rechargement**.

Si un flux reste figé sans avancer d'un caractère entre deux lectures, mettre l'onglet au premier plan et
cliquer dans la conversation. Recharger ne fait que relire l'état déjà reçu. Si le composer reste plein
après l'envoi, le message n'est pas parti : ne pas attendre une réponse inexistante. Recycler l'onglet
connu n'arrive qu'après un échec d'écriture confirmé ; cela ne crée jamais un chat neuf et ne change
jamais la cible.

## 2 — INSTALLER

Il n'y a **rien à installer côté machine** — pas de dépendance, pas de script, pas de configuration. Ce que
tu peux réellement faire pour l'utilisateur, selon ce que la vérification a montré :

- **Contrôle du navigateur absent** (`gpt`, `navigateur`) → explique comment obtenir l'extension Claude
  pour Chrome, puis propose de relancer cette commande. Ne prétends pas l'installer toi-même.
- **Pas de projet ChatGPT** → propose de l'aider à en créer un et à **rédiger ses instructions de projet**,
  qui sont le vrai cœur du dispositif (voir ci-dessous). ⚠️ La **création** du projet reste son geste :
  les compétences pilotent un projet existant, elles n'en fabriquent jamais un de leur propre initiative.
- **Codex CLI absent ou non connecté** (`codex`) → dis-le, indique `codex login`, et laisse-le faire.
- **Mode `hubspot` demandé, `claude-breeze` absente** → indique le pack HubSpot public
  <https://github.com/Gremelinn0/sales-hubspot-toolkit>, ne l'installe pas à sa place.
- **Tout est vert** → rien à faire, et dis-le franchement plutôt que d'inventer une étape.

## 3 — EXPLIQUER

C'est la partie que les README ratent, et c'est celle qui fait revenir les gens. Couvre les trois
questions, brièvement :

**Comment ça marche.** Le plugin ne réfléchit pas à la place des autres IA : il en fait **l'opérateur**.
`claude-ia-delegation` choisit la cible (ou l'utilisateur la force par un mot : `/claude-ia-delegation
gpt`, `codex`, `session`…), puis la compétence de transport rejoint la conversation existante, lit ses
derniers tours, transmet un mandat borné, prouve que l'envoi est parti et relit le résultat. Pour
ChatGPT, la méthode vit dans les **instructions du projet** — c'est pour ça qu'un projet vide ne donne rien
d'utile.

**Quand ça se déclenche.** Sur des demandes du type « délègue ça », « économise mes tokens », « fais bosser
GPT / Codex », « parle à l'autre tâche », ou quand un travail de volume correspond manifestement à un
cerveau déjà configuré. Le déclenchement se fait sur l'intention, pas sur le nom du plugin.

**Pourquoi ça n'a rien fait.** Par ordre de fréquence réelle :
1. **Pas de projet ChatGPT configuré**, ou un projet sans méthode dans ses instructions (mode `gpt`).
2. **Contrôle du navigateur absent ou non connecté** — les modes `gpt` et `navigateur` n'ont alors aucune
   main.
3. **Session ChatGPT déconnectée** dans le navigateur : le pilotage s'ouvre sur un écran de login.
4. **Le transport du mode demandé n'est pas là** (`hubspot` sans le pack HubSpot, `gemini` non câblé).
5. **La tâche ne valait pas une délégation** : un jugement fin, une décision, ou une tâche plus courte que
   son propre handoff restent chez Claude, et c'est voulu.

## Le rapport final

Format court, une ligne par point vérifié, **avec ce qui l'a prouvé** :

```
Compétences        : <n> trouvée(s) — <noms>
gpt                : ✅ opérationnel (<n> onglets vus) | ⚠️ présent non connecté | ❌ absent
codex              : ✅ <version>, connecté | ⚠️ installé, non connecté | ❌ absent
session            : ✅ outils natifs trouvés | ❌ aucun outil de tâche/session
navigateur         : expérimental — prévol <n>/2 passé | ❌ extension absente
desktop            : ✅ Computer Use présent | non vérifiable
hubspot            : ✅ claude-breeze trouvée | ❌ absente → pack HubSpot public
Écrit sur la machine : rien
```

Puis **une** phrase : ce qu'il peut faire maintenant, ou le seul geste qui manque. Pas de liste de
recommandations quand tout est vert — le silence est une bonne nouvelle lisible.

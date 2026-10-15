---
name: claude-codex
description: >-
  Déléguer à Codex, l'agent OpenAI local (Codex CLI), le travail qui a besoin de FICHIERS RÉELS :
  plusieurs dépôts montés ensemble, du code à écrire, des builds, des tests, une revue de diff, pendant
  que Claude garde le jugement fin. Porte `codex exec` (racine, dossiers supplémentaires, bac à sable),
  le cadrage par AGENTS.md et la relecture de ce que Codex a écrit. À invoquer dès que le mot Codex
  apparaît, ou quand une tâche demande du volume SUR DES FICHIERS (« fais-moi ça dans les trois
  dépôts », « lance un agent là-dessus », « Codex ou GPT ou toi ? »). PAS pour piloter un projet ChatGPT
  web (`ia-delegation-local-to-gpt-chrome`) ni pour arbitrer le cerveau en général
  (`claude-ia-delegation`).
---

# Skill — claude-codex (déléguer à l'agent OpenAI qui a le disque)

> 🗂️ **La compétence se souvient des travaux confiés à Codex, pas l'utilisateur.** Quatre gestes, sur le
> registre juste en dessous :
> 1. **Avant de lancer Codex** : trouver au registre le travail déjà en cours sur ce sujet, avec son
>    dossier racine. Le reprendre plutôt que d'en lancer un second.
> 2. **Un travail nouveau** entre au registre avant le lancement : le sujet, le dossier racine, la
>    session Claude qui le confie.
> 3. **Après chaque rendu**, sa ligne se met à jour : le dernier lot, le résumé, le commit.
> 4. **Deux sessions** ne confient pas à Codex le même dossier en même temps.

## Le registre des travaux Codex

⚠️ Ce tableau vit dans la compétence installée : une mise à jour du plugin le remet à zéro. Pour qu'il
survive, copie-le dans un fichier à toi (par exemple à la racine de ton projet) et tiens-le là.

| Travail | Sujet | Dossier racine | Session Claude qui le confie | Lancé | Résumé | Dernier rendu |
|---|---|---|---|---|---|---|
| *(aucun pour l'instant : le premier travail lancé s'inscrit ici)* | | | | | | |

## §1 Le discriminant — une seule question

**Est-ce que la tâche a besoin de TOUCHER des fichiers réels ?**

- **Oui, et beaucoup** — plusieurs dépôts, du code, des builds, des tests, une migration, un balayage →
  **Codex**. C'est sa nature : un agent local avec un bac à sable, pas un chat à qui on colle du texte.
- **Non, elle raisonne ou elle rédige** — arbitrage éditorial, angle marketing, comparatif, brouillon long →
  **ChatGPT web**, via `ia-delegation-local-to-gpt-chrome`.
- **Elle demande un jugement fin, une décision produit, ou une donnée sensible** → **Claude**, ici, sans
  déléguer.

C'est la seule frontière qui tient. Le volume ne suffit pas à trancher : du volume qui ne touche aucun
fichier va très bien chez GPT web, et une correction de trois lignes dans quatre dépôts est un travail de
Codex même si elle est minuscule.

> ⚠️ **Cette branche corrige une exclusion qui rendait Codex indélégable.** Tant que la seule cible était
> un chat sans disque, « le local — fichiers, dépôt, logs, commandes » ne se déléguait jamais. Ce qui
> reste non délégable, c'est le **jugement** et la **décision**, pas le local (voir le chapeau
> `claude-ia-delegation`, section « Le réflexe se DÉCLENCHE »).

## §2 Le geste — `codex exec`, et ses quatre options qui comptent

Le mode non interactif est `codex exec`. Tout le reste de la CLI est du confort.

```bash
codex exec -C "<dossier principal>" --add-dir "<autre dossier>" -s workspace-write "<la tâche>"
```

| Option | Ce qu'elle règle | Pourquoi elle compte ici |
|---|---|---|
| `-C, --cd <DIR>` | la racine de travail | c'est **ce dossier** qui décide quel `AGENTS.md` est chargé (§3) |
| `--add-dir <DIR>` | dossiers supplémentaires **inscriptibles** | **c'est ça, le multi-dossiers** — le vrai mécanisme, répétable |
| `-s, --sandbox` | `read-only` · `workspace-write` · `danger-full-access` | une tâche d'audit part en `read-only` : elle ne peut alors rien casser, quoi qu'elle décide |
| `-o, --output-last-message <FILE>` | écrit la réponse finale dans un fichier | **le canal de lecture étroit** (chapeau, section « Ce que la supervision apporte ») : on lit un fichier, pas un flux entier |

Autres verbes utiles, présents dans la CLI : `codex review` (revue de diff non interactive) ·
`codex apply` (applique le dernier diff produit, en `git apply`) · `codex exec resume --last` (reprendre) ·
`codex mcp` (ses connecteurs) · `--json` (événements en JSONL, pour un script) · `--output-schema`
(forcer la forme de la réponse finale).

**La règle qui évite le gâchis** : une tâche donnée à Codex est **bornée et vérifiable**. Il travaille
seul, sans qu'on le regarde ; une consigne floue produit du volume qu'il faudra relire entièrement, ce
qui annule l'économie. Lui dire quoi produire, où, et à quoi on reconnaîtra que c'est fait.

## §3 Le cadrage — `AGENTS.md`, et la preuve qu'il est lu

**Codex charge automatiquement le `AGENTS.md` de sa racine de travail. Mesuré le 2026-08-10** : un jeton
unique posé dans ce fichier est ressorti d'un `codex exec` qui avait interdiction de lire quoi que ce soit
et n'a lancé aucune commande. C'est un témoin positif, pas une déduction.

> 🪞 **Et il faut le savoir, parce que Codex dit le contraire.** Interrogé une minute plus tôt sur les
> fichiers qu'il charge au démarrage, il a répondu : *« Aucun fichier d'instructions n'est chargé
> automatiquement »*. C'est faux : **la réponse d'un outil sur son propre effet n'est pas une source**. Un
> agent ne voit pas d'où vient son propre contexte. On mesure, on ne demande pas.

**Ce qu'on met dans un `AGENTS.md`** — le cadrage, jamais la doctrine :

- le **périmètre** du projet et les dossiers montés ;
- des **pointeurs** vers ce qu'il peut lire lui-même (le fichier de règles du projet, la carte de la
  documentation) ;
- les **interdits durs** de l'environnement — sur un dépôt partagé, les gestes git qui coûtent du travail ;
- ce qu'on attend en **sortie**.

**Ce qu'on n'y met jamais** : une copie d'un document qu'il peut ouvrir. Le contrôle en une question —
*ce que je m'apprête à recopier, peut-il le lire lui-même ?* Oui → on donne le chemin. C'est le même
renversement que pour ChatGPT connecté à un dépôt : **on donne des chemins, jamais des fichiers**.

**La confiance du dossier** vit dans `~/.codex/config.toml`, sous la forme `[projects.'<chemin en
minuscules>']` avec `trust_level = "trusted"` (structure revérifiée le 15/10/2026 sur Windows : les clés y
sont des chemins en minuscules). Elle se pose au premier lancement interactif dans le dossier ; un dossier
parent déclaré couvre ses enfants.

## §4 Le retour — Codex écrit dans le dépôt, donc on relit le dépôt

C'est la différence majeure avec `ia-delegation-local-to-gpt-chrome`, qui organise la récupération d'un
**paquet** produit ailleurs. Ici il n'y a pas de paquet : Codex a écrit sur le disque, parfois commité.

**Les trois gestes de relecture, dans l'ordre** :

1. **Ce qu'il a produit** — lire le fichier de `-o`, pas le flux.
2. **Ce qu'il a touché** — `git status` puis `git log --stat` sur ses commits. Un compte de fichiers qui ne
   correspond pas à la tâche est le signal.
3. **Ce qu'il a pris à quelqu'un d'autre** — sur un dépôt où plusieurs sessions travaillent en parallèle,
   un commit peut emporter le travail d'une voisine sans que rien ne crie. Certains gestes git coûtent du
   travail et restent invisibles à un simple `--stat` (un effacement « à somme nulle » : autant de lignes
   retirées qu'ajoutées). Ces gestes s'appliquent à Codex **exactement comme à nous** — sauf que lui ne les
   connaît pas, sauf si le `AGENTS.md` les lui dit.

**Ce qu'on ne fait pas** : relire son raisonnement. On mesure les **faits** qu'il avance, jamais le chemin
qui y mène (chapeau, même section). Une supervision qui ne mesure pas ajoute du bruit, et parfois du faux.

## §5 Garde-fous

- **Aucun bundle, aucune copie.** Fabriquer un dossier de fichiers recopiés pour Codex reviendrait à
  rebâtir, pour un agent qui a un accès disque natif, la béquille inventée pour un agent qui n'en avait
  pas. Si l'envie revient, c'est le signe qu'on n'a pas donné le bon chemin.
- **Le bac à sable est un choix, pas un défaut.** Une tâche qui n'a pas besoin d'écrire part en
  `read-only`. `danger-full-access` ne se donne pas « pour être tranquille ».
- **Un dossier de plus se monte avec `--add-dir`, jamais en déplaçant du contenu** pour le rapprocher.
  Déplacer du contenu pour arranger un agent, c'est fabriquer une deuxième vérité.
- **On annonce la délégation en une ligne** — « je passe ça à Codex, je récupère et je relis ». Une
  délégation invisible fait croire que le travail a été fait ici, et plus personne ne sait d'où vient le
  résultat.
- **La méthode de Codex vit ICI, pas dans le chapeau.** `claude-ia-delegation` route, il ne décrit pas
  comment on parle à une cible.

## Compétences liées

`claude-ia-delegation` (le chapeau qui route — cette compétence en est la branche Codex) ·
`ia-delegation-local-to-gpt-chrome` (la branche ChatGPT web : raisonne et rédige, sans disque) ·
`ia-delegation-local-to-local` (les commandes natives entre tâches, dont les tâches Codex desktop).
Le quota de ton compte est la contrainte amont : vérifie dans ton plan ce qu'il reste avant un gros lot.

## Mesures de référence

Ce bloc existe pour qu'on ne remesure pas ce qui est déjà écrit. Il se re-vérifie seulement si quelque
chose contredit.

- Codex CLI **v0.144.5**. Les options du §2 (`-C`, `--add-dir`, `-s`, `-o`, `--json`, `--output-schema`) et
  les verbes `exec resume`, `review`, `apply`, `mcp` ont été relus dans l'aide de cette version le
  15/10/2026.
- `AGENTS.md` de la racine de travail : **chargé automatiquement** (témoin positif du 10/08/2026, §3).
- `--add-dir` est bien le mécanisme multi-dossiers, et il est **répétable**.

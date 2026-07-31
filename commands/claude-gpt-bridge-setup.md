---
description: Vérifie que claude-gpt-bridge peut réellement fonctionner ici, installe ce qui manque, et explique comment il se déclenche.
argument-hint: "[--uninstall]"
---

# Setup — claude-gpt-bridge

Tu exécutes le setup de ce plugin. Trois fonctions, **dans cet ordre** : VÉRIFIER, INSTALLER,
EXPLIQUER. L'utilisateur vient de l'installer, ou bien il revient parce que « ça n'a rien fait ».

> ⚠️ **Le verdict se CALCULE, il ne se récite pas.** Chaque ligne du rapport doit venir d'une sonde
> que tu viens de lancer. Une case cochée sans mesure derrière est pire qu'une case absente : elle
> fait croire que c'est vérifié. Si une sonde est impossible ici, écris **« non vérifiable »** — c'est
> une réponse honnête, contrairement à un ✅ optimiste.

## Si l'argument est `--uninstall`

Ce plugin **n'écrit rien** chez l'utilisateur : ni fichier de configuration, ni clé, ni dossier, ni
entrée dans ses réglages. Il n'y a donc **rien à retirer** — le désinstaller, c'est retirer le plugin
lui-même (`/plugin uninstall claude-gpt-bridge`).

Dis-le tel quel, et **ne cherche pas** de fichiers à nettoyer : en inventer donnerait l'impression
que le plugin en avait posé.

## 1 — VÉRIFIER (sonder, pas supposer)

**a) Les compétences sont-elles bien là ?** Liste le contenu de `skills/` dans le répertoire du
plugin. Le nombre et les noms trouvés sont le verdict — ne les recopie pas depuis ce fichier, ils
mentiraient le jour où le pack changera.

**b) Le contrôle du navigateur est-il disponible ?** C'est **le** prérequis dur : le skill pilote
ChatGPT dans le vrai navigateur de l'utilisateur, avec sa session déjà connectée.
Sonde-le pour de vrai — tente de lister les onglets ouverts. Trois issues, et elles ne se
confondent pas :

| Ce que tu observes | Verdict | Ce que ça veut dire |
|---|---|---|
| Les outils de navigateur n'existent pas dans cette session | ❌ **Bloquant** | Le plugin ne pourra rien piloter. Il faut l'extension Claude pour Chrome. |
| Les outils existent mais aucune connexion au navigateur | ⚠️ **À brancher** | L'extension est là mais pas connectée — l'utilisateur doit ouvrir Chrome et l'autoriser. |
| Tu obtiens la liste des onglets | ✅ **Opérationnel** | C'est la seule preuve qui vaut. |

⚠️ **Le navigateur interne sandboxé ne convient pas** : il faut la session ChatGPT **authentifiée**
de l'utilisateur. Si tu ne peux sonder que celui-là, dis-le — ne compte pas ça comme un succès.

**c) Le projet ChatGPT existe-t-il ?** Ça, aucune sonde ne peut le deviner : **demande l'URL** du
projet visé (elle ressemble à `chatgpt.com/g/g-p-…`). S'il n'en a pas encore, c'est **le** point
bloquant — voir la section EXPLIQUER, parce que c'est presque toujours la vraie cause d'un « ça ne
marche pas ».

Si l'utilisateur donne une URL et que le contrôle du navigateur répond, **ouvre-la** : une page de
connexion ou une 404 en dit plus que dix questions.

## 1bis — CALIBRER LA FIABILITÉ D'ENVOI (des questions, pas un test)

Il existe une panne **fréquente et silencieuse** qui n'a rien à voir avec le plugin : le message
paraît envoyé — la bulle s'affiche, le champ se vide — mais **il n'atteint jamais le serveur**, et il
a disparu au rechargement suivant. Elle dépend de la machine et surtout **des réglages du
navigateur** (onglets mis en veille pour économiser la mémoire). Sur une machine modeste, ou avec
l'économiseur de mémoire réglé agressivement, elle arrive **souvent**.

Le skill sait déjà y répondre — il **recycle l'onglet** (ferme l'ancien, en rouvre un neuf) au lieu
d'analyser. Ce qu'il ne peut pas deviner, c'est s'il doit le faire **d'emblée** ou seulement après un
premier incident. **Demande-le, ne le teste pas** : un test ne reproduirait pas la panne à la demande,
et coûterait des tours pour rien.

Trois questions, une seule fois :

1. **« Une seule machine, ou plusieurs ? »** — si le réglage vaut pour un poste unique, la réponse
   peut être enregistrée une bonne fois ; sinon elle se redemandera ailleurs.
2. **« Ton navigateur met-il les onglets en veille rapidement ? »** — Edge le fait avec un délai
   réglable, parfois très court ; Chrome endort au bout d'un temps plus long. Un utilisateur qui a
   réduit ce délai, ou dont la machine est chargée, est en plein dans le cas à risque.
3. **« Tes onglets d'IA rament-ils, ou en gardes-tu beaucoup d'ouverts ? »** — c'est le meilleur
   indice pratique, et il ne demande aucune mesure.

**Ce que tu en fais** — inscris la réponse dans le compte rendu, et applique :

| Réponses | Comportement d'envoi à retenir |
|---|---|
| Veille rapide, machine chargée, ou onglets lents | **Recyclage d'emblée** : chaque envoi part dans un onglet neuf, l'ancien est fermé. |
| Rien de tout ça | Envoi normal, **et recyclage systématique dès le premier incident** — jamais de deuxième analyse. |

⚠️ **Fermer l'ancien onglet fait partie du geste.** Un onglet d'IA laissé ouvert continue de
consommer de la mémoire ; fermé puis rouvert, il repart nettement plus léger. Rouvrir sans fermer
double la charge — c'est l'inverse de l'effet recherché.

## 2 — INSTALLER

Il n'y a **rien à installer côté machine** — pas de dépendance, pas de script, pas de configuration.
Ce que tu peux réellement faire pour l'utilisateur, selon ce que la vérification a montré :

- **Contrôle du navigateur absent** → explique comment obtenir l'extension Claude pour Chrome, puis
  propose de relancer cette commande. Ne prétends pas l'installer toi-même.
- **Pas de projet ChatGPT** → propose de l'aider à en créer un et à **rédiger ses instructions de
  projet**, qui sont le vrai cœur du dispositif (voir ci-dessous). ⚠️ La **création** du projet reste
  son geste : le skill pilote un projet existant, il n'en fabrique jamais un de sa propre initiative.
- **Tout est vert** → rien à faire, et dis-le franchement plutôt que d'inventer une étape.

## 3 — EXPLIQUER

C'est la partie que les README ratent, et c'est celle qui fait revenir les gens. Couvre les trois
questions, brièvement :

**Comment ça marche.** Le plugin ne réfléchit pas à la place de ChatGPT : il en fait **l'opérateur**.
Claude ouvre un chat neuf dans le projet, nomme le sujet, lit la réponse, boucle. La méthode, elle,
vit dans les **instructions du projet ChatGPT** — c'est pour ça qu'un projet vide ne donne rien
d'utile, et c'est l'erreur numéro un.

**Quand ça se déclenche.** Sur des demandes du type « délègue ça à ChatGPT », « fais bosser mon
projet ChatGPT », « économise mes tokens sur GPT », ou quand un travail de recherche ou de rédaction
correspond manifestement à un projet déjà configuré. Le déclenchement se fait sur l'intention, pas
sur le nom du plugin.

**Pourquoi ça n'a rien fait.** Par ordre de fréquence réelle :
1. **Pas de projet ChatGPT configuré**, ou un projet sans méthode dans ses instructions.
2. **Contrôle du navigateur absent ou non connecté** — le plugin n'a alors aucune main.
3. **Session ChatGPT déconnectée** dans le navigateur : le pilotage s'ouvre sur un écran de login.
4. La demande relevait du **local** (fichiers, dépôt) — dans ce cas c'est Claude qui doit faire le
   travail, pas ChatGPT, et le skill est conçu pour ne pas se déclencher.

## Le rapport final

Format court, une ligne par point vérifié, **avec ce qui l'a prouvé** :

```
Compétences        : <n> trouvée(s) — <noms>
Navigateur         : ✅ opérationnel (<n> onglets vus) | ⚠️ présent non connecté | ❌ absent
Projet ChatGPT     : ✅ <url> répond | ⚠️ non fourni | ❌ inaccessible
Écrit sur la machine : rien
```

Puis **une** phrase : ce qu'il peut faire maintenant, ou le seul geste qui manque. Pas de liste de
recommandations quand tout est vert — le silence est une bonne nouvelle lisible.

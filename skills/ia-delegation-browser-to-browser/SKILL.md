---
name: ia-delegation-browser-to-browser
description: >-
  Quand un agent exécuté dans le navigateur dispose réellement d'une extension de contrôle et doit
  travailler avec une autre conversation web → piloter l'onglet cible, vérifier l'envoi et relire
  le résultat sans prétendre qu'un pont applicatif existe. Triggers : « Chrome vers Chrome »,
  « navigateur vers navigateur », « fais travailler cet autre onglet », « via l'extension ».
  MVP expérimental : ne s'active que si les capacités de lecture ET d'écriture sont visibles et testées.
---

# ia-delegation-browser-to-browser — un onglet pilote un autre onglet

> 🗂️ **La compétence se souvient des fils, pas l'utilisateur.** Quatre gestes, sur le registre juste en
> dessous :
> 1. **Avant d'écrire** : trouver le fil au registre, par le sujet. Ne jamais demander à l'utilisateur un
>    lien déjà inscrit.
> 2. **Un fil nouveau** entre au registre avant le premier envoi.
> 3. **Après chaque lot rendu**, sa ligne se met à jour : le dernier lot, le résumé, qui a la main.
> 4. **On n'envoie rien** tant que le lot d'une autre session tourne dans ce fil.
>
> Un fil ChatGPT piloté depuis une application locale s'inscrit dans le registre de
> `ia-delegation-local-to-gpt-chrome`, pas ici.

## Le registre des fils

⚠️ Ce tableau vit dans la compétence installée : une mise à jour du plugin le remet à zéro. Pour qu'il
survive, copie-le dans un fichier à toi (par exemple à la racine de ton projet) et tiens-le là.

| Fil | Sujet | Sessions qui y travaillent | Ouvert | Résumé | Dernier lot · qui a la main |
|---|---|---|---|---|---|
| *(aucun pour l'instant : le premier fil piloté s'inscrit ici)* | | | | | |

## Statut et objectif

Cette voie est un **MVP expérimental**, distinct des transports déjà prouvés. Elle devient utilisable
uniquement lorsqu'un agent exécuté dans le navigateur expose, via une extension, des commandes qui peuvent
lister les onglets, lire la page cible, écrire dans son composeur et vérifier l'envoi.

Une extension installée ne suffit pas. Sans témoin positif de ces quatre capacités dans la session
courante, ce skill refuse de promettre le trajet et route vers `ia-delegation-local-to-gpt-chrome` depuis
une application locale.

## Prévol décidable

1. Lister les onglets et retrouver la cible par URL ou identifiant stable.
2. Lire un élément connu de la conversation cible.
3. Focaliser le composeur sans écrire.
4. Vérifier qu'un reçu d'action est observable après une interaction de test non destructive.

Un seul cran manque → trajet **non disponible ici**. Ne pas remplacer silencieusement l'extension par
Computer Use ni par un capteur local de conversations : ce serait un autre transport.

## Cycle MVP

1. **Cibler** la conversation exacte ; jamais « le premier onglet ChatGPT ».
2. **Lire la fin utile**, pas toute la page ni les instructions épinglées.
3. **Préparer un message unique** avec objectif, contraintes et preuve attendue.
4. **Écrire puis vérifier avant envoi** : contenu intégral dans le composeur, bonne cible, aucune
   duplication après reconnexion.
5. **Envoyer**, puis confirmer que le composeur est vide et que le dernier message utilisateur correspond à
   la fin attendue.
6. **Attendre sans poller l'interface** si l'extension expose un état ; sinon rendre la main et relire à un
   cycle ultérieur.

## Garde-fous

- Aucune affirmation « navigateur à navigateur » (post, README, fiche de marketplace) avant un test réel
  aller + retour avec captures ou journal d'actions.
- Pas d'écriture pendant que la cible génère.
- Pas d'envoi de secret, de contenu privé inutile ni de fichier que la cible sait déjà lire.
- Pas de boucle autonome si la session source perd ses outils lorsque l'onglet passe en arrière-plan.

## Critère de sortie du statut expérimental

Trois cycles réels consécutifs sur deux conversations distinctes doivent prouver : bonne cible, message
complet, réponse récupérée, aucune duplication et aucun vol d'onglet utilisateur. Avant cela, le skill
reste un MVP honnête, pas une capacité générale.

## Compétences liées

`ia-delegation-local-to-local` pour les commandes natives entre tâches locales ·
`ia-delegation-local-to-gpt-chrome` quand la source est une application locale ·
`claude-ia-delegation` (le chapeau qui route).

**Test de survie** : le prévol, puis trois cycles complets ; son absence reste un état légitime et bruyant.

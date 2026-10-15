---
name: ia-delegation-codex-to-claude-desktop
description: >-
  Quand Codex local doit poursuivre une session Claude Desktop existante → résoudre la cible par
  identifiant, projet et titre, lire son transcript, puis utiliser Computer Use uniquement pour lui
  envoyer un mandat groupé. Triggers : « envoie ça à Claude Desktop », « Codex vers Claude »,
  « poursuis cette session Claude ». PAS pour créer une session, piloter Claude web ni lire Claude
  par capture d'écran.
---

# Codex → Claude Desktop

> 🗂️ **La compétence se souvient des sessions, pas l'utilisateur.** Quatre gestes, sur le registre juste en
> dessous :
> 1. **Avant d'écrire** : trouver la session Claude Desktop au registre, par le sujet. Ne jamais demander
>    à l'utilisateur une session déjà inscrite.
> 2. **Une session nouvelle** entre au registre avant le premier mandat.
> 3. **Après chaque lot rendu**, sa ligne se met à jour : le dernier lot, le résumé, qui a la main.
> 4. **On n'écrit pas** à une session pendant qu'un lot d'une autre y tourne.

## Le registre des sessions Claude Desktop

⚠️ Ce tableau vit dans la compétence installée : une mise à jour du plugin le remet à zéro. Pour qu'il
survive, copie-le dans un fichier à toi (par exemple à la racine de ton projet) et tiens-le là.

| Session | Sujet | Qui lui écrit | Ouverte | Résumé | Dernier lot · qui a la main |
|---|---|---|---|---|---|
| *(aucune pour l'instant : la première session pilotée s'inscrit ici)* | | | | | |

## Ce que cette compétence résout

Codex local doit poursuivre une session Claude Desktop qui existe déjà. Claude Desktop n'offre pas de canal
natif pour lui écrire. La compétence trouve la bonne session, lit son transcript, et n'emploie Computer Use
que pour écrire le mandat.

## Décisions et rejets

| Décision | Pourquoi | Option refusée | Se rouvre si |
|---|---|---|---|
| Un registre des sessions en tête | la compétence garde la mémoire des sessions qu'on lui confie, pas l'utilisateur | se fier à la mémoire de la session, ou à celle de l'utilisateur | un canal natif garde lui-même l'historique des sessions |
| Computer Use pour écrire, et seulement pour écrire | Claude Desktop n'expose pas d'envoi natif à une session existante | lire Claude par capture d'écran | Claude Desktop expose un envoi natif |

## Statut de la preuve

Cette branche produit un mandat envoyé et son reçu ; son contrôle est que le mandat apparaît dans le
transcript de la session cible. Aucune boucle. **Non mesuré ici** : aucune session de référence ni preuve
de cycle complet n'est fournie avec le pack ; l'identifiant réel d'une session se relève au premier
trajet. Trajet décrit pour Windows (gestes via Computer Use) : à éprouver chez toi avant de t'y fier.

## Contrat

Cette branche adapte `ia-delegation-local-to-local` au seul trajet **Codex local → Claude Desktop**. Elle
réutilise son contrat d'identité, de lecture et de suivi ; elle n'en recopie pas les règles communes.

Claude Desktop n'exposant pas ici de canal natif d'envoi à une session existante :

- **mandat** : Computer Use, uniquement pour écrire dans Claude Desktop ;
- **lecture et suivi** : transcript local prioritaire ;
- **livrable** : fichier, commit, transcript ciblé ou réponse stable ;
- **capture d'écran** : repli visuel pour identifier l'état affiché, jamais contrôle de Claude ni canal de
  lecture de ses fichiers.

Avant tout geste Windows, appliquer les règles de l'outil Computer Use de ta session, notamment ses
confirmations et la sélection explicite de la fenêtre cible.

## Entrée minimale

Disposer de :

1. l'identifiant stable de la session Claude ;
2. son projet et son titre, comme contrôles supplémentaires ;
3. un mandat unique et borné ;
4. le livrable et la preuve attendus.

Une correspondance sur le titre seul ne suffit jamais. Aucune session Claude n'est créée par cette
branche.

## Cycle

1. **Résoudre la session.** Retrouver le transcript par identifiant, puis vérifier projet, titre et
   derniers tours. Si plusieurs cibles restent plausibles, ne rien envoyer.
2. **Contrôler l'état.** Lire la fin du transcript. Si la cible répond encore, attend une permission ou
   porte déjà le même mandat, ne pas l'interrompre.
3. **Ouvrir la cible exacte.** Avec Computer Use, mettre Claude Desktop au premier plan, sélectionner la
   session résolue et vérifier à nouveau projet, titre et état visible.
4. **Envoyer une fois.** Saisir un seul mandat groupé. Respecter la confirmation requise au moment de
   l'envoi.
5. **Prouver l'envoi.** Le composer est vide et un nouveau message utilisateur est visible dans la bonne
   session. L'absence de l'un de ces deux témoins interdit d'annoncer un succès.
6. **Libérer l'écran.** Quitter le pilotage UI dès la preuve obtenue. Suivre ensuite la réponse par le
   transcript, à des checkpoints bornés, sans interroger l'interface en boucle.
7. **Récupérer et vérifier.** Lire le canal durable annoncé, contrôler la preuve demandée, puis rendre un
   reçu : session, mandat envoyé, livrable, vérification et reste éventuel.

## Arrêts sûrs

- session, projet ou fenêtre introuvable : **aucun envoi** ;
- cible encore en train de travailler : attendre par transcript, sans message de relance ;
- mauvaise session affichée : revenir à la résolution, sans saisir de texte ;
- Computer Use indisponible : blocage explicite, jamais faux succès ;
- envoi sans double témoin : état inconnu, relire avant toute reprise ;
- deux échecs identiques : arrêter et rendre le blocage, sans troisième tentative.

## Compétences liées

`claude-ia-delegation` choisit cette route · `ia-delegation-local-to-local` porte le contrat commun ·
`ia-delegation-local-to-gpt-chrome` porte le cycle d'envoi dont cette branche reprend les témoins · l'outil
Computer Use de ta session exécute le seul geste Windows autorisé.

**Ensemble** : transport transverse sans feature produit. Sa preuve est un cycle réel sur une session
Claude existante : cible stable, envoi unique visible, suivi par transcript et livrable vérifié.

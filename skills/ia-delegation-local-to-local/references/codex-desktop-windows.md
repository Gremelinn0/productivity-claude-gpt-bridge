# Écrire dans une tâche Codex desktop (Windows) par l'interface

Mesuré le 2026-09-29, en livrant une mission à une tâche Codex existante. Ce sont des faits d'hôte,
relevés sur une installation Windows à cette date : la méthode reste celle de `SKILL.md`, et ces gestes se
re-vérifient si l'application a été mise à jour.

## Trouver l'application

- **Dans le menu Démarrer, Codex s'appelle « ChatGPT »** : paquet `OpenAI.Codex`, exécutable
  `ChatGPT.exe`, titre de fenêtre « ChatGPT ». `request_access` se fait avec « ChatGPT » ;
  « ChatGPT Classic » est une autre application.
- La réponse de `request_access` donne l'écran de la fenêtre (`windowLocations`). Si ce n'est pas l'écran
  de la capture, `switch_display` d'abord, sinon la capture montre le bureau.

## Atteindre la tâche

- Les tâches sont dans la barre latérale, sous leur groupe.
- Un clic sur le nom d'une tâche a ouvert « Renommer le chat ». « Annuler » ne change rien. Contrôler
  ensuite le titre en haut du fil avant de taper.

## Écrire et envoyer

- `type` passe par le presse-papiers, même sans le droit `clipboardWrite` : le contenu précédent du
  presse-papiers de l'utilisateur est écrasé.
- Entrée envoie. Le message tient donc en un seul paragraphe, sans saut de ligne.
- Envoi réussi : le message apparaît dans le fil, puis « Réflexion en cours », et la tâche montre un
  indicateur d'activité dans la barre latérale.

## Prouver la réception

- Le fil vit dans `~/.codex/sessions/AAAA/MM/JJ/rollout-*-<id de la tâche>.jsonl`. Noter sa taille
  **avant** l'envoi, puis lire à partir de cet octet (`tail -c +<taille+1>`). Ce fichier a grossi de 9 Mo en
  11 minutes pendant le travail : une lecture de fin de fichier de taille fixe manque le message.
- Le nouveau `turn_context` donne le bac à sable, le modèle et l'effort du tour.
- La preuve d'intégration est le premier changement demandé, lu sur le disque. Un accusé de réception n'en
  est pas une.

## Canal

`codex exec resume <id>` n'a pas été essayé pour cette livraison. Le choix du 2026-09-29 a été
l'interface : l'agent piloté vit dans l'application, avec sa planification périodique, son navigateur et
ses connecteurs.

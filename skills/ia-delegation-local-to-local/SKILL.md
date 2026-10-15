---
name: ia-delegation-local-to-local
description: >-
  Quand une application IA locale doit observer, poursuivre ou challenger une autre tâche locale
  → utiliser d'abord les commandes natives de tâches/sessions, sans ouvrir l'interface ni passer
  par une application tierce. Triggers : « parle à l'autre tâche », « lis son checkpoint »,
  « supervise mes sessions locales », « envoie-lui ce feedback », « local vers local ».
  PAS pour piloter ChatGPT dans Chrome ni pour coordonner deux agents déjà dans le navigateur.
---

# ia-delegation-local-to-local — commandes natives entre tâches locales

> 🗂️ **La compétence se souvient des sessions, pas l'utilisateur.** La mémoire, c'est le registre juste en
> dessous. Quatre gestes :
> 1. **Avant d'écrire** : trouver la session au registre, par le sujet. Ne jamais demander à
>    l'utilisateur une session déjà inscrite.
> 2. **Une session nouvelle** (donnée par l'utilisateur, ou ouverte sur son ordre) entre au registre
>    avant le premier message.
> 3. **Après chaque lot rendu**, sa ligne se met à jour : le dernier lot, le résumé, qui a la main.
> 4. **On n'écrit pas** à une session pendant qu'un lot d'une autre session y tourne.

## Le registre des sessions

« non relevé » : l'information manque ; elle s'ajoute au prochain échange.

⚠️ Ce tableau vit dans la compétence installée : une mise à jour du plugin le remet à zéro. Pour qu'il
survive, copie-le dans un fichier à toi (par exemple à la racine de ton projet) et tiens-le là.

| Session | Sujet | Sessions qui lui écrivent | Ouverte | Résumé | Dernier lot · qui a la main |
|---|---|---|---|---|---|
| *(aucune pour l'instant : la première session pilotée s'inscrit ici)* | | | | | |

## Objectif

Faire collaborer deux tâches hébergées par une application IA locale sans transformer l'interface
graphique en protocole. Le canal natif de l'hôte gagne toujours : il connaît l'identité de la tâche, son
état et le format d'un message.

Ce skill décrit un **transport**, pas un orchestrateur. Le choix de la tâche et du cerveau reste chez le
routeur de délégation (`claude-ia-delegation`) ; ici, on observe et on transmet proprement.

## Contrat de transport

| Besoin | Canal prioritaire | Repli honnête |
|---|---|---|
| Lister les tâches | commande native de liste | métadonnées locales de sessions |
| Lire l'état | lecture/checkpoint natif ou transcript local | fin du fichier de transcript |
| Attendre | commande native d'attente avec curseur | nouveau réveil borné, jamais du polling UI |
| Écrire | commande native d'envoi à la tâche exacte | interface seulement si aucun canal natif n'existe |

Exemples d'API : une application peut exposer `list_threads`, `read_thread`, `wait_threads` et
`send_message_to_thread` ; une autre `list_sessions`, `get_session` et `send_message`. Les noms varient,
la méthode non.

## Cycle

1. **Identifier** la cible par son identifiant stable, son hôte et son projet. Le titre aide à la
   retrouver ; il ne prouve ni son identité ni son état.
2. **Lire sans réveiller** : checkpoint natif ou fin du transcript. Ne jamais envoyer « où en es-tu ? »
   pour obtenir une information déjà exposée localement.
3. **Décider** si une intervention apporte un fait, une contrainte ou un résultat attendu nouveau.
4. **Envoyer une fois**, groupé : constat, conséquence, résultat attendu, éléments à préserver, preuve
   demandée.
5. **Vérifier au tour suivant** par le canal de lecture. Une réponse d'accusé de réception n'est pas une
   preuve d'intégration.

### Repli d'écriture entre applications

**Règle : ne jamais faire porter le transport à l'utilisateur quand la cible exacte peut être résolue.**
Si l'application cible n'expose réellement aucun canal natif d'envoi :

1. résoudre d'abord la tâche par son identifiant stable, son projet et son transcript ; le titre visible
   ne sert que de contrôle supplémentaire ;
2. si plusieurs cibles restent plausibles après cette découverte, demander à l'utilisateur quelle
   conversation choisir ; sinon ne pas lui redemander une information déjà trouvée ;
3. utiliser Computer Use pour écrire un seul message groupé dans la session exacte, sans demander à
   l'utilisateur de copier-coller le handoff ;
4. vérifier visuellement la session avant la saisie, respecter toute confirmation imposée par l'outil au
   moment de l'envoi, puis relire le prochain checkpoint par le transcript ou le canal natif de lecture.

Codex desktop sous Windows : l'application s'appelle « ChatGPT » dans le menu Démarrer. Gestes et pièges
mesurés : [`references/codex-desktop-windows.md`](references/codex-desktop-windows.md).

**Contrôle décidable :** avant de demander à l'utilisateur de relayer un message, l'absence de canal natif
d'écriture est-elle prouvée et la recherche locale laisse-t-elle plusieurs cibles plausibles ? Non →
poursuivre la découverte ou livrer directement par le repli UI ; oui → poser une seule question sur la
conversation exacte.

## Frontières

- **Aucun capteur intermédiaire requis.** Un transcript ou une API native existe déjà : ajouter un capteur
  entre les deux augmenterait le nombre de pannes possibles sans créer d'information.
- **Aucun Computer Use pour lire un statut.** L'interface est réservée au dernier repli d'écriture ou à une
  inspection visuelle qui ne peut pas être dérivée des données de tâche.
- **Pas de message décoratif.** Si la cible travaille sur la même pièce et n'a pas livré de checkpoint
  stable, la laisser travailler est une décision valide.
- **Pas de création de tâche.** La création, le fork, l'archivage et le rangement appartiennent aux
  commandes de gestion de tâches de l'hôte.

## Preuve de fin

Le cycle est réussi quand la bonne tâche a reçu un message utile, que son prochain checkpoint montre la
mutation demandée et qu'aucune interface n'a été mobilisée pour une information déjà disponible par
commande ou transcript.

## Compétences liées

`claude-ia-delegation` (le chapeau qui route) · `ia-delegation-codex-to-claude-desktop` adapte ce contrat
au trajet Codex → Claude Desktop · `ia-delegation-local-to-gpt-chrome` pilote ChatGPT dans Chrome ·
`ia-delegation-browser-to-browser` couvre le trajet expérimental entre deux onglets · `claude-codex`
délègue du travail sur fichiers à Codex en ligne de commande.

**Ensemble** : méthode transverse de transport, sans feature produit ni boucle QA dédiée. Sa preuve est un
cycle réel avec identité de cible, lecture étroite, envoi unique et checkpoint relu.

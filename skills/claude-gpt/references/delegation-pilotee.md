# Mode — Délégation pilotée

> 📎 Contrat de mode de **`/claude-gpt`** (cf son §3bis). Se charge **seul**, quand ce mode est actif — jamais en même temps que [`reflexion-croisee.md`](reflexion-croisee.md), les deux décrivent des autorités différentes.
> Il dit **qui décide quoi** pendant le mode. Il ne remplace rien de la plomberie du skill (atteindre l'onglet, écrire, lire, récupérer un paquet) : celle-là reste entière.

## Finalité

Faire avancer un plan existant en confiant à GPT un lot suffisamment fermé.

Claude pilote la progression générale. GPT pilote intellectuellement et opérationnellement le lot reçu.

## Invocation

```text
Appelle /claude-gpt.

Rôle : pilote intellectuel et opérationnel du lot confié.
Mode : DÉLÉGATION PILOTÉE.

Tâche : <objectif et pointeur vers le lot actif>.
```

## Conditions d’entrée

Utiliser ce mode lorsqu’un plan, une étape ou un lot existe déjà, que la sortie attendue est identifiable et qu’aucune décision structurelle indispensable ne reste ouverte.

Passer en `RÉFLEXION CROISÉE` lorsque le problème principal consiste à comprendre, challenger ou arbitrer plusieurs directions structurelles.

## Autorités

Claude possède :

- le plan global ;
- le séquençage ;
- la sélection du lot ;
- le choix et le changement de mode ;
- la cohérence entre les lots ;
- la décision de poursuivre, corriger, différer ou arrêter.

GPT possède :

- la compréhension détaillée du lot ;
- son organisation interne ;
- le raisonnement ;
- l’exécution dans les fichiers autorisés ;
- les contrôles du lot ;
- le résultat et ses preuves.

GPT peut proposer une amélioration du plan, mais ne la transforme pas seul en nouveau chantier.

## Résolution du contexte

GitHub porte le contexte durable. GPT lit lui-même le lot, le HEAD concerné, les sources propriétaires explicitement liées et le delta utile.

Règles :

- chemin exact avant recherche ;
- index avant source longue ;
- ne pas relire un SHA inchangé ;
- aucune recherche globale répétée ;
- aucun polling GitHub ;
- grouper les écritures cohérentes.

## Boucle

1. Claude désigne le rôle, le mode, le lot et le résultat attendu.
2. GPT résout les sources, organise son exécution et poursuit jusqu’au prochain gate réel.
3. GPT rend le résultat, les fichiers concernés, les contrôles, les preuves, les écarts différés et son verdict.
4. Claude révise le delta sans refaire l’analyse complète.
5. Claude choisit : `VALIDÉ → LOT SUIVANT`, `CORRECTION CIBLÉE`, `CORRECTION DIRECTE PAR CLAUDE`, `PASSAGE EN RÉFLEXION CROISÉE`, `DÉCISION UTILISATEUR` ou `ACCÈS BLOQUÉ`.

## Revue non bloquante

Un défaut bloque seulement s’il invalide le livrable, casse une dépendance suivante, contredit une décision existante, fausse les preuves ou impose une décision structurelle.

Une amélioration non nécessaire à l’étape suivante est différée sans nouvelle investigation.

## Corrections

Claude peut corriger directement une erreur courte, évidente, réversible, locale et sans arbitrage produit ou architectural.

Sinon, il renvoie une correction consolidée :

```text
Constat : <défaut précis>
Emplacement : <fichier, section ou preuve>
Attendu : <résultat exact>
Critère de réussite : <contrôle observable>

Corrige ce même lot et rends uniquement le delta corrigé.
```

## Hors-plan

Chaque découverte est classée :

- bloque le lot → traitement minimal maintenant ;
- utile plus tard → une ligne dans le support existant, sans investigation ;
- sans impact démontré → abandon pour la vague active.

## Changement de mode

Passer en `RÉFLEXION CROISÉE` uniquement si une hypothèse structurante, l’objectif, le propriétaire durable ou l’architecture retenue sont réellement remis en cause.

Un bug ordinaire, un chemin mort identifiable, un test rouge reproductible ou une correction locale ne changent pas le mode.

## Fin

Le mode se termine lorsque le lot est accepté, qu’un nouveau lot est désigné, qu’une décision structurelle devient nécessaire, que l'utilisateur doit trancher ou qu’un accès indispensable reste bloqué.

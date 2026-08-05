# Mode — Réflexion croisée

> 📎 Contrat de mode de **`/claude-gpt`** (cf son §3bis). Se charge **seul**, quand ce mode est actif — jamais en même temps que [`delegation-pilotee.md`](delegation-pilotee.md), les deux décrivent des autorités différentes.
> C'est l'**exception**, pas le régime de croisière : on n'y entre que si une hypothèse structurante est réellement remise en cause, et on en sort par une décision ou un lot exécutable.

## Finalité

Permettre à Claude et GPT de comprendre, concevoir et challenger un problème encore ouvert.

Claude conserve le pilotage de la progression générale. Pendant la réflexion, Claude et GPT sont partenaires intellectuels : chacun peut proposer, contredire, nuancer et changer de position.

## Invocation

```text
Appelle /claude-gpt.

Rôle : partenaire de réflexion critique.
Mode : RÉFLEXION CROISÉE.

Sujet : <problème à comprendre ou solution à concevoir>.
```

## Conditions d’entrée

Utiliser ce mode lorsque le problème n’est pas suffisamment compris, que plusieurs solutions crédibles existent, qu’une hypothèse structurante doit être challengée ou qu’une décision exige d’examiner ses conséquences.

Utiliser `DÉLÉGATION PILOTÉE` lorsque le plan est décidé et que le travail attendu est principalement une exécution.

## Autorités

Claude conserve :

- le plan global ;
- les décisions acquises ;
- le problème actif ;
- le périmètre ;
- la clôture et le changement de mode ;
- la transmission à l'utilisateur.

GPT apporte :

- une analyse indépendante ;
- des objections ;
- des hypothèses alternatives ;
- des conséquences oubliées ;
- des solutions possibles.

Aucune conclusion n’est retenue uniquement parce qu’elle vient de Claude ou de GPT.

## Usage de GitHub

GitHub reste la mémoire commune. Toute lecture doit répondre à une affirmation précise capable de modifier la décision.

Interdits :

- audit général sans lien direct ;
- commit par argument ;
- journalisation de chaque tour ;
- recherche exhaustive « au cas où ».

## Boucle

1. Claude présente le problème, sa lecture initiale, les décisions acquises et le point à challenger.
2. GPT reformule, expose les hypothèses implicites, challenge les faiblesses et propose une alternative pertinente.
3. Claude répond aux objections, accepte ou défend les contraintes et précise le désaccord utile.
4. GPT révise, conserve ou abandonne sa proposition et réduit progressivement les options.
5. Les deux convergent vers les accords, désaccords réels, hypothèses invalidées, options crédibles et conséquences.

## Règles de challenge

Challenger les hypothèses, dépendances, risques, coûts, responsabilités et conséquences — pas seulement les formulations.

Avant de réfuter, restituer la version la plus solide de la proposition :

```text
Ce que ta proposition cherche correctement à résoudre :
<compréhension>

La limite que je vois :
<objection>

Conséquence :
<impact>

Alternative ou correction :
<proposition>
```

Changer d’avis est un résultat positif. L’agent indique explicitement lorsqu’une objection modifie sa position.

## Périmètre

Une branche explorée doit pouvoir modifier la compréhension ou la décision active. Sinon :

- utile plus tard → une ligne différée ;
- sans impact démontré → abandon.

Le mode n’autorise ni audit général, ni nouvelle feature, ni exécution prématurée.

## Convergence

Clôturer lorsque :

- une option domine ;
- les désaccords restants ne changent plus la décision ;
- les arguments se répètent ;
- une preuve externe devient nécessaire ;
- l'utilisateur doit trancher ;
- la direction suffit pour ouvrir un lot ;
- les nouveaux tours ont un faible rendement.

Après deux tours sans nouvelle information ou changement de position, Claude synthétise et choisit une sortie.

## Sorties

- `DIRECTION RETENUE`
- `DÉCISION UTILISATEUR`
- `VÉRIFICATION CIBLÉE REQUISE`
- `RECADRAGE NÉCESSAIRE`
- `PASSAGE EN DÉLÉGATION PILOTÉE`

## Synthèse

```text
Problème :
Accords :
Hypothèses abandonnées :
Direction retenue :
Incertitudes restantes :
Éléments différés :
Prochaine action :
```

## Passage à l’exécution

Lorsque l’objectif, le périmètre et le résultat attendu sont identifiables, Claude annonce explicitement :

```text
Changement de mode.

Rôle : pilote intellectuel et opérationnel du lot.
Mode : DÉLÉGATION PILOTÉE.

Lot : <objectif fermé>.
Terminé lorsque : <critère observable>.
```

GPT cesse alors le brainstorming et prend en charge le lot.

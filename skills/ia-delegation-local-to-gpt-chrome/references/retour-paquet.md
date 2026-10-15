# Le sens retour — récupérer ce que ChatGPT a produit

> Complément de [`../SKILL.md`](../SKILL.md). La collaboration a deux sens : **aller** (Claude → ChatGPT :
> un sujet à traiter, ChatGPT réfléchit et rédige) et **retour** (ChatGPT → Claude : du durable consolidé,
> qui doit remplacer les sources actives). Sans le retour, la collaboration fuit : du bon travail reste
> bloqué dans un fil de chat pendant que le dépôt et le projet ChatGPT continuent de tourner sur des sources
> périmées — chacun croyant lire la bonne version, l'erreur ne se découvrant qu'au premier calcul faux.

Deux cas, qui ne se traitent pas pareil :

| Cas | Ce qu'on fait |
|---|---|
| **ChatGPT est connecté à ton dépôt** | il y écrit et pousse lui-même : on relit son commit, on ne réinstalle rien (§1, §2) |
| **Il produit HORS dépôt** (un paquet de sources généré en canvas, un zip, un jeu de fichiers de référence) | ChatGPT **produit**, Claude **installe** : le protocole des §3 à §8 |

## §1 Si ChatGPT a accès au dépôt — on lui donne des CHEMINS, jamais des fichiers

**La question à poser une fois, au début, et à ne plus jamais deviner** : *ce projet ChatGPT est-il connecté
au dépôt de code ?* Un projet peut l'être (connecteur de dépôt, accès à une organisation privée) — et
alors il **lit l'état réel d'une branche, les fichiers, les commits ; il vérifie les SHA et les
modifications parallèles ; il modifie, commite et pousse**.

**Ce que ça change dans le geste** : on ne lui **transporte** plus de fichiers, ni de contenu de dépôt,
ni de doc recopiée. On lui donne **le lot, les chemins concernés, les contraintes et les vérifications
attendues** — exactement comme à un agent de dev. Le pilotage navigateur reste le canal de la
**conversation**, plus du transport de code.

**La nuance à lui faire dire** : selon la session, il peut avoir l'accès au dépôt **sans clone local
complet**. Il lit et publie quand même, mais les tests qui exigent le checkout entier **reviennent à
Claude** — et il doit le **signaler clairement, sans inventer leur résultat**.

**Le contrôle, en une question** : *ce que je m'apprête à lui coller, est-ce qu'il peut le lire lui-même ?*
Oui → donner le **chemin**, pas le contenu. Coller ce qu'il peut lire brûle des crédits sur du transport
au lieu du jugement.

- ❌ Coller un fichier de règles, une doc ou un extrait de code dans le fil sans avoir vérifié qu'il ne
  peut pas le lire.
- ❌ Bâtir un plan entier sur « il faudra lui transmettre les fichiers » : une compétence bâtie sur le seul
  pilotage navigateur laisse croire que c'est le seul canal, et l'erreur ne se voit jamais — tout
  fonctionne, en payant le transport à chaque tour.

## §2 Une livraison qu'on ne voit pas se va chercher — jamais se CONSTATE absente

Le §1 dit qu'il **peut** publier dans le dépôt. Il ne dit pas qu'il publie **toujours**. **Une livraison
absente du dépôt n'est pas une livraison absente : c'est une livraison qui vit encore dans le fil**, en
canvas ou en message. On l'y **lit** (pilotage navigateur), on ne rapporte pas « il n'a pas livré ».

Les deux erreurs sont symétriques, et les deux se paient :

1. **Conclure l'absence depuis l'historique du dépôt** — il peut publier **pendant** qu'on mesure : le
   fichier change sous les pieds entre deux appels de la même passe. ⇒ `git fetch` juste avant de conclure,
   jamais un `git log` tiré dix minutes plus tôt.
2. **Croire le fil quand le dépôt est en avance** — le canvas peut être **figé à une révision antérieure**
   à ce qui est publié. Travailler dessus, c'est réfuter une version déjà corrigée, et le diff a l'air
   parfaitement normal (cas mesuré : le fichier récupéré du fil portait une valeur chiffrée que la version
   publiée avait déjà corrigée).

**Le geste, dans cet ordre** : `git fetch` → chercher le contenu dans le dépôt → introuvable, alors **aller
le lire dans le fil** → et **avant de le traiter, comparer les deux** : le dépôt dit l'**état**, le fil dit
l'**intention**. Ce qui est publié gagne.

**La question jumelle, celle qui coûte le plus cher : ce lot, ne l'a-t-il pas DÉJÀ LIVRÉ ?** Son dernier
message dit ce qu'il **comptait** faire, pas ce qu'il a **fait depuis**. La complétion se lit là où le
travail atterrit (le dépôt, le dossier de sortie, l'historique des commits), jamais dans le fil de
conversation. Confier un lot déjà livré coûte double : il refait le travail, et le tour est perdu.

## §3 Exiger un paquet COMPLET — jamais reconstruire depuis le fil

Avant de télécharger quoi que ce soit, vérifier que ChatGPT a bien produit une **archive autosuffisante** :
les fichiers canoniques **entiers**, un `_MANIFESTE.json` régénéré, une note de version. S'il a seulement
collé du texte dans le chat, **le lui redemander** — un fichier reconstitué depuis un extrait de
conversation a l'air juste et a perdu des passages en silence, ce qui est pire qu'une absence de mise à
jour.

## §4 Récupérer — trois voies, de la plus simple à la plus autonome

**Le paquet s'auto-identifie.** Son `_MANIFESTE.json` porte, pour chaque fichier, son chemin de
destination exact. Une fois le zip en main, il n'y a plus rien à demander : le projet, le bundle et la
destination se lisent dedans. Demander « c'est quel projet ? » alors que la réponse est dans le fichier,
c'est faire relire à l'utilisateur ce qu'on a déjà sous la main.

**Voie A — l'utilisateur clique, Claude déroule** *(le défaut, et le plus simple)*. Il clique sur le lien
dans ChatGPT. Claude prend le fichier **le plus récent des Téléchargements**, lit le manifeste, reconnaît
le projet, et enchaîne — **sans aucune question**. Un clic humain, tout le reste automatique.

**Voie B — Claude va le chercher** *(quand l'utilisateur demande le mode autonome)*. Claude connaît la
session ChatGPT visée : ouvrir la conversation par le contrôle navigateur, lire le **dernier message**.
- Lien de téléchargement présent → cliquer, puis annoncer le nom du fichier une fois arrivé.
- Aucun lien → **réclamer le paquet à ChatGPT** dans le fil (§3), attendre qu'il le génère, puis cliquer.

Chaîne mesurée de bout en bout, un seul geste humain (la reconnexion de l'extension) : lister les
navigateurs connectés → naviguer vers le lien de conversation → attendre ~4 s (le spinner de chargement
fait répondre `get_page_text` « No text content found » : un chargement, pas une panne) → `get_page_text`
sur le fil → clic sur la carte du document (le premier clic la transforme en bouton « Open file », le
canvas s'ouvre au suivant) → clic sur l'icône de téléchargement en haut à droite du canvas → fichier dans
les Téléchargements (75 152 octets, 1 273 lignes, intègres) : 7 appels navigateur, zéro question après le
lien. Le téléchargement bat la lecture par défilement dès que le fichier dépasse quelques centaines de
lignes : **télécharger, puis lire en local**.

La demande de l'utilisateur (« va chercher le paquet », « fais-le en autonome ») **est** l'autorisation de
ce téléchargement-là. Elle vaut pour le paquet visé, pas pour les autres fichiers croisés en chemin — un
bouton de téléchargement rencontré au passage se demande avant de cliquer.

**Voie C — le contexte manque.** Rien dans les Téléchargements et aucune session identifiable →
**deux questions, pas plus** : sur quel projet on travaille, et ce qu'il veut en faire. Les poser coûte
dix secondes ; deviner coûte une installation au mauvais endroit, qu'on ne repère qu'au premier calcul
faux.

Dans les trois cas, décompresser dans un dossier temporaire — jamais directement par-dessus les fichiers
actifs.

## §5 Vérifier AVANT d'écraser — c'est le manifeste qui tranche

Comparer `_MANIFESTE.json` entrant et fichiers actifs sur **empreinte + taille + date**. Un paquet **plus
ancien ou plus petit** que l'actif n'est pas forcément faux, mais il n'est jamais anodin : le signaler avec
les deux chiffres et attendre l'arbitrage, plutôt que de rétrécir une référence en silence (installé à
l'aveugle, un paquet d'un kilo-octet de moins retire un kilo-octet de référence sans que personne ne le
sache).

## §6 Le nom aplati est une ADRESSE, pas un nom de fichier

Un fichier de paquet nommé `claude_skills_mon-skill_MA-REFERENCE.md` n'est pas un fichier à poser tel
quel : le préfixe encode son **chemin de destination**. Le champ `source` du manifeste donne le vrai
chemin (`.claude/skills/mon-skill/MA-REFERENCE.md`) — **le lire, ne jamais déduire du nom**. Poser le nom
aplati dans le dépôt crée un fichier fantôme : il existe, il a l'air installé, et aucun consommateur ne le
lit jamais.

## §7 Les TROIS bouts — en sauter un suffit à tout désynchroniser

| Bout | Geste | Qui en est propriétaire |
|---|---|---|
| **Le dépôt** (source de vérité) | écrire au chemin réel du manifeste, puis commit — le commit EST la sauvegarde, rien à supprimer à la main | le dépôt concerné |
| **Le dossier partagé que ChatGPT lit** | **ne jamais l'éditer à la main** : il est *généré*. Mettre à jour le dépôt, puis relancer son script de génération | le script qui génère ton dossier partagé, s'il existe |
| **L'onglet Sources du projet ChatGPT** | contrôle navigateur : retirer l'ancienne version, téléverser la nouvelle sous son nom canonique | cette compétence |

Le bout du milieu est celui qu'on saute — et l'éditer à la main est pire que l'oublier, parce que le
prochain contrôle du script annonce alors « à jour » sur un dossier bricolé. Un document créé en canvas
dans un chat n'est **pas** visible des autres chats du projet tant qu'il n'est pas dans **Sources**. **Sans
dossier partagé généré, ce tableau se réduit à deux bouts** (le dépôt et les Sources).

**Aucune ancienne copie ne reste active** : un `fichier (1).md` ou deux `.zip` du même jour dans les
Téléchargements, c'est la désynchronisation qui se prépare — les doublons partent dans un dossier
d'archive daté, jamais à la corbeille.

## §8 Le reçu clôt la synchronisation

Finir par un reçu court qui dit, pour chaque bout : ce qui a été remplacé, où, et ce qui **ne l'a pas
été**. Un paquet à moitié installé sans que ça se voie est le seul vrai échec de ce protocole — d'où la
ligne « éléments non appliqués », qui vaut mieux vide que absente.

```text
Paquet : <nom> · <date> · <empreinte>
Dépôt : <fichiers> → <chemins> · commit <sha>
Dossier partagé : régénéré via <script> ✅ / non fait / sans objet
Projet ChatGPT : <fichiers remplacés> · doublons retirés : <…>
Non appliqué : aucun | <action manuelle précise>
```

Et avant tout calcul important qui s'appuie sur ces sources, **dire quelle version a réellement été lue**
(empreinte ou date) — un raisonnement juste sur une source périmée reste faux, et c'est indétectable dans
la réponse.

## §9 Si le contrôle du navigateur bloque

Ne jamais s'arrêter à « je ne peux pas ». Faire **tout ce qui reste possible** (décompresser, installer le
dépôt, régénérer le dossier partagé), puis donner à l'utilisateur **une seule manipulation précise** :
quel fichier, où le déposer, quel ancien remplacer, comment vérifier. Une manipulation précise se fait en
trente secondes ; un « ça n'a pas marché » coûte une session. Contrôle du navigateur mort dans cette
session mais vivant dans une autre : router le travail vers l'autre session plutôt qu'insister.

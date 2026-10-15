# Pannes et reprise — piloter ChatGPT dans Chrome

> Complément de [`../SKILL.md`](../SKILL.md). Relevés faits en usage réel entre août et octobre 2026,
> sur l'interface web de ChatGPT et l'extension Claude in Chrome. **L'interface change** : chaque
> sélecteur ci-dessous est un relevé daté, pas un contrat. Quand un geste ne produit rien, **lire l'état de
> l'élément avant d'accuser l'environnement** (un bouton désactivé, une référence périmée, un mauvais
> champ) — la cause réelle a plusieurs fois été là.

**Trois témoins, par ordre de fiabilité.** (1) Le **serveur** : recharger puis relire, `stream_status`,
liste des conversations, branche Git du lot. (2) Le **DOM local** : il décrit ce que le client a *décidé*
d'afficher, jamais ce que le serveur a *accepté* — il peut mentir dans les deux sens (message affiché que
le serveur n'a pas ; réponse cachée que le serveur a). (3) La **capture d'écran** : elle expire quand
l'onglet est masqué.

## §1 Atteindre et lire

- **On pilote par l'URL de la conversation** : `/c/<id>` sur le domaine de ChatGPT, ou
  `/g/g-p-<projet>/c/<id>` pour un fil rangé dans un projet. Ne pas compter sur l'URL racine d'un projet
  (`/g/g-p-<projet>`) : lors des relevés d'août 2026 elle a redirigé vers l'accueil **sans erreur**, et
  les relevés ont donné des résultats contraires d'un jour à l'autre. Une mauvaise cible (un identifiant
  périmé) et une porte murée affichent exactement la même page d'accueil : avant d'accuser la porte,
  relire l'identifiant.
- **Retrouver un fil par son nom, sans rien demander** : les conversations récentes de la barre latérale
  sont de vrais liens, lisibles en un appel :
  `[...document.querySelectorAll('a[href^="/c/"]')].map(a=>({t:a.innerText.trim(), h:a.getAttribute('href')}))`.
  Les conversations **rangées dans un projet** n'y figurent pas : déplier la ligne du projet dans la barre
  latérale (un `element.click()` ordinaire ; relire `aria-expanded` après coup, ne jamais cliquer deux fois
  « pour être sûr ») puis lire les `href` de ses conversations, qui portent les deux identifiants
  (`/g/g-p-<projet>/c/<conversation>`). Un projet se renomme : un identifiant noté devient faux en silence,
  donc on le relit. Introuvable malgré ça : demander **ce fil-là**, nommément.
- **Page en chargement** : elle rend d'abord un spinner et `get_page_text` répond « No text content found ».
  Ce n'est pas une panne, c'est un chargement : attendre quelques secondes et relire.
- **« Could not load this ChatGPT conversation »** (titre « New chat », aucun champ de saisie) : le fil
  n'est pas cassé, ChatGPT bride. Le dire par le réseau, pas par la page : `read_network_requests` filtré
  sur `backend-api` montre des 429 et des 503. Fermer l'onglet, faire autre chose dix minutes, rouvrir une
  fois ; « Try again » et les rechargements en série entretiennent la bride (relevé le 08/10/2026 : trois
  ouvertures refusées, la quatrième dix minutes plus tard a chargé le fil). Ne jamais taper dans le champ de
  la page projet en attendant : il ouvre un fil neuf.
- **Lecture par script qui rend `[BLOCKED: Cookie/query string data]`** : le filtre de l'extension retient
  un texte qui porte un lien à paramètres (ChatGPT ajoute souvent `utm_source`). Ne pas maquiller le texte
  pour passer le filtre : `get_page_text` sur l'onglet rend la réponse entière, ou une capture
  (`scale` 0,6) ; le rendu lui-même se lit dans le commit (`git fetch`).
- **Compteur de tours par sélecteur (`[data-message-author-role]`) à 0 sur un fil chargé** : le témoin
  « message présent une fois » est muet, pas négatif. La capture ou le texte de `main` tranche ; jamais un
  renvoi sur ce seul compteur.
- **Seconde copie cachée de la conversation** : la page peut porter une copie masquée (`display:none`),
  placée après la vraie, avec les mêmes clés `data-content-search-unit-key`. Le dernier élément trouvé
  tombe alors dans cette copie, qui peut retarder. Ne lire que l'affiché :
  `.filter((e) => e.getClientRects().length > 0)`.
- **Le fil ne répond plus, même par script** (un fil long : chaque `javascript_tool` expire à 45 s,
  « Runtime.evaluate timeout ») : fermer cet onglet et ouvrir la page légère
  `https://chatgpt.com/api/auth/session` (du JSON, même origine). De là,
  `JSON.parse(document.body.innerText).accessToken`, puis `fetch('/backend-api/conversation/<id>')` rend le
  fil entier : remonter `mapping[current_node]` par ses `parent` pour lire les derniers tours ;
  `…/stream_status` dit s'il travaille encore. ⚠️ API interne **non documentée**, qui peut changer sans
  préavis : lecture seule, depuis l'onglet connecté de l'utilisateur ; le jeton reste dans le script, il
  n'est ni affiché, ni écrit, ni envoyé ailleurs. Une lecture juste après l'envoi peut ne pas voir le
  message : relire une minute plus tard avant de conclure.
- **Fil très long, page figée sur un état ancien** : la page et la lecture de la conversation peuvent
  montrer un instantané vieux de plusieurs heures. Ni l'une ni l'autre ne prouve un envoi, ni son absence.
  Le témoin fiable : la liste des conversations (`/backend-api/conversations?order=updated`, même jeton) ;
  le `update_time` du fil avance à la seconde de l'envoi. Il ne bouge pas : rien n'est parti, on peut
  renvoyer sans doublon.
- **Un fil finit par peser.** Les sorties d'outils de GPT (journaux, tests) alourdissent la conversation :
  relevé les 08 et 09/10/2026, un fil de construction pesait 25,7 Mo après un seul tour et un autre ne se
  chargeait plus après un seul lot. Demander des sorties d'outils **courtes** (la fin d'un journal, le
  résumé d'un test) et un rendu final court ; ouvrir un fil suite dès que la page ne charge plus, dont le
  premier message rappelle la façon de travailler et ce qui est déjà fusionné.

## §2 Écrire

**La séquence qui marche** (relevée en octobre 2026, onglet masqué compris) :

1. vérifier que le champ est **vide** (un brouillon de l'utilisateur peut y attendre) ;
2. `focus()` sur `form .ProseMirror[contenteditable="true"]` — le premier `.ProseMirror` de la page peut
   être une réponse rédigée affichée dans un bloc modifiable ; l'id `#prompt-textarea` d'août 2026 avait
   disparu au relevé du 08/10/2026 ;
3. `document.execCommand('insertText', false, <texte>)` — il accepte les sauts de ligne et n'envoie rien ;
4. **attendre 1,5 s** : l'écran se met à jour avant l'état interne de ChatGPT, et un clic dans la foulée
   envoie l'ancien contenu ;
5. comparer le champ au message (espaces normalisés : `innerText` ajoute des sauts de ligne) ;
6. cliquer le bouton d'envoi **du formulaire du champ** : `aria-label="Send"` (« Envoyer » en interface
   française ; il n'apparaît qu'après la saisie, donc chercher les deux libellés après une courte attente).
   Le `data-testid="send-button"` a été relevé absent le 02/10 et le 09/10/2026 (le clic par ce sélecteur
   ne part pas, le texte reste dans le composer) mais présent le soir du 08/10 : l'interface varie, d'où
   la recherche par `aria-label` d'abord ;
7. exiger les témoins (§3).

**Pièges d'écriture, tous rencontrés en réel :**

- **Clic sans effet ?** Lire l'état du bouton avant d'accuser l'onglet. En août 2026, le bouton d'envoi
  restait `disabled` après `insertText` seul (l'éditeur riche ne tient pas son état dans le DOM) et trois
  évènements (`beforeinput`, `input`, `change`, de type `insertText`) le rallumaient.
- **La touche Entrée quand le clic ne part pas** : sur un fil très long, deux clics par script sont restés
  sans effet (`update_time` inchangé) ; `focus()` sur le champ, curseur à la fin, puis la touche Entrée
  par l'outil clavier de l'extension a envoyé du premier coup.
- **Sur un fil long, insérer et cliquer en deux appels séparés.** Un seul script qui insère, attend et
  clique dépasse souvent le délai de l'outil (45 s). Après ce délai, le texte est presque toujours dans le
  champ, non envoyé : relire le champ ; s'il porte le message, cliquer « Envoyer » seulement, sans recoller.
- **Un champ vide ne prouve pas que rien n'est parti.** Un envoi a été observé partir sans clic, le champ
  vidé seul entre l'insertion et la comparaison (cause non mesurée). Avant de recoller, relire le fil, pas
  seulement le champ.
- **Trois pièges du champ** : une adresse web dans le texte devient un lien, avec une espace de plus, et
  l'empreinte diffère (écrire l'exemple sans adresse) ; le champ enregistre du markdown (barres
  d'échappement, adresses en liens), donc l'empreinte se prend **dans le champ, avant l'envoi** ; une
  longueur ne prouve rien (le navigateur compte un emoji pour deux caractères) : seule l'empreinte compare.
  Dans un onglet caché, un `setTimeout` ne part pas et un appel qui attend passe les 45 s : insérer,
  comparer et cliquer par des appels courts, sans minuterie.
- **Saisie clavier simulée (`computer type`)** : sur une disposition AZERTY, un message de ~2 000
  caractères est devenu 68 caractères de charabia (seuls les accentués passaient), sans erreur. Insérer par
  le DOM.
- **Le multi-ligne envoie le message** quand il passe par une frappe : un saut de ligne dans un `type`
  déclenche l'envoi et le message part en morceaux. `insertText` règle le problème ; sinon `shift+Return`
  entre les segments.
- **Déconnexion en pleine frappe** : l'extension se reconnecte seule, mais le champ garde ce qui avait
  commencé à s'écrire et la frappe suivante **s'ajoute**. Après toute déconnexion : `ctrl+a` puis `Delete`
  avant de retaper.
- **Fenêtre du navigateur en arrière-plan** : les frappes et clics synthétiques sont **avalés sans
  erreur** (six frappes ont toutes rapporté « Typed… », le champ est resté vide). Et l'arbre d'accessibilité
  du composer ne rend que son `placeholder` : il ne peut ni confirmer ni infirmer l'écriture. Le seul
  témoin fiable est JavaScript.
- **Ne jamais écrire pendant qu'elle génère** : un message envoyé en pleine génération se fait mettre en
  file au mieux, avaler au pire. Composer dans le champ, vérifier l'état, envoyer quand elle a fini.
- **Page masquée** (`document.visibilityState` vaut `hidden`) : le groupe d'onglets de l'extension vit
  dans la fenêtre du navigateur de l'utilisateur, en arrière-plan ; l'onglet n'est visible que s'il est
  l'onglet actif de sa fenêtre, et ni la mise au premier plan de la fenêtre, ni une capture, ni un
  redimensionnement ne le rendent visible. On envoie quand même, par script, puis on juge au témoin. Ne
  jamais conclure « masquée, donc impossible » : un blocage d'envoi observé sur un fil très long a d'abord
  été attribué à la visibilité, puis à la longueur du fil, deux hypothèses réfutées ; dans le cas relevé,
  le fil portait un tour figé resté ouvert depuis deux jours, et après « Stop » par script et rechargement
  l'envoi est parti (§4). **Avant l'envoi, chercher ce tour figé.**
- **Fermer le dernier onglet du groupe ne donne pas de fenêtre à soi** : `tabs_context_mcp` avec
  `createIfEmpty` recrée le groupe dans la fenêtre de l'utilisateur, en onglet masqué.

## §3 Prouver

- **Le reçu local est nécessaire, pas suffisant.** Il peut être entièrement vert (champ vide, message dans
  le DOM en `data-message-author-role="user"`, texte vérifié au caractère près, bouton d'arrêt affiché) sur
  un message que le serveur **n'a jamais reçu** : au rechargement, le fil était identique à celui d'avant
  l'envoi (relevé le 11/08/2026, deux fois de suite sur un même fil). **Le seul contrôle qui tranche** :
  recharger la même URL, puis relire depuis le serveur —
  `[...document.querySelectorAll('[data-message-author-role="user"]')].some(m => m.innerText.includes('<un marqueur du message>'))`.
  Lire aussi la **suite des rôles** : `…, assistant, user` avec ton message en dernier prouve qu'il est
  arrivé après la dernière réponse ; `…, user, assistant` inchangé prouve qu'il a été avalé. Le même
  message, recomposé dans un onglet neuf, est passé du premier coup.
- **Livré et répondu sont deux faits distincts.** Le rechargement prouve le premier, jamais le second.
  Après un envoi, **deux** témoins : le message est là, **et** un tour a démarré (un nœud `assistant`
  ajouté, un bouton `button[aria-label="Stop"]` présent, ou `stream_status` à `IS_STREAMING`). Ni l'un ni
  l'autre au bout d'une minute : aucun tour n'a été mis en file, relancer court.
- **La preuve sûre qu'un tour est ouvert, même onglet masqué** : `GET /backend-api/conversation/<id>/stream_status`
  rend `IS_STREAMING` ou `COMPLETE` (jeton de `/api/auth/session`). La conversation lue en entier peut, elle,
  s'arrêter à un nœud vieux de quinze minutes pendant que GPT commite encore ; `stream_status` tranche.
- **Quand GPT travaille sur une branche, la branche fait foi.** `git fetch` de la branche du lot ; un commit
  neuf qui cite le message est un reçu. C'est aussi le meilleur détecteur de tour figé : aucun commit neuf
  depuis vingt minutes alors que le mandat en annonçait plusieurs.
- **Sur un fil à la dernière réponse jamais accrochée**, le nœud courant reste le message envoyé : la
  branche fait foi, pas la page.

## §4 Un tour qui ne revient pas

- **Flux décroché** : réponse coupée en plein milieu, fil figé, message envoyé sans tour. **Onglet au
  premier plan + vrai clic dans la conversation.** Recharger, recycler l'onglet, attendre et renvoyer ne
  reprennent jamais un flux décroché ; recharger **relit** ce que le serveur a déjà. Le tell : rien
  n'avance d'un caractère entre deux lectures espacées. Une réponse anormalement courte (3 caractères, 27
  caractères coupés en pleine phrase) peut n'être qu'un affichage tronqué : après un rechargement, les
  mêmes réponses faisaient 146 et 104 caractères, complètes. Ne jamais relancer sur cette seule base :
  le rechargement sert à **lire** l'état vrai du serveur, pas à réparer un flux.
- **Tour figé** (« Analysis paused », « Connection interrupted », bouton « Stop » toujours là, plus aucun
  pas) : tant qu'il reste ouvert, un message neuf s'affiche puis disparaît au rechargement. Le geste :
  constater le figement (aucun pas neuf entre deux lectures espacées, la même dernière étape qu'au réveil
  précédent), cliquer « Stop », renvoyer, vérifier après rechargement. Ce n'est pas interrompre une
  génération : elle ne travaille plus. Détails mesurés : « Connection interrupted » peut ne s'afficher
  que quelques minutes après le chargement ; le clic de l'outil sur « Stop » peut ne pas prendre (fenêtre
  cachée : le bouton revient au rechargement) alors que le clic par script (`button.click()`) prend ;
  recharger ensuite, le bouton doit avoir disparu avant le renvoi. **Le tour ouvert se cherche par
  `button[aria-label="Stop"]`** : le `data-testid="stop-button"` ne se voyait plus au relevé d'octobre, et
  un test par ce seul sélecteur dit « libre » à tort.
- **Un envoi invisible au rechargement n'est pas forcément perdu.** Sur un tour « Connection interrupted.
  Waiting for the complete answer », le message disparaît au rechargement et GPT peut l'avoir reçu : un
  commit neuf qui cite le message dit qu'il est reçu. Ni Stop, ni renvoi : un guet.
- **Génération fantôme** : bouton d'arrêt affiché en continu, plusieurs rechargements qui confirment, et
  le dernier message de l'assistant ne bouge pas d'un caractère. « Il génère » est l'explication la plus
  naturelle du silence et elle est auto-renforçante. **Contrôle** : au-delà de ~25 minutes sans un caractère
  de plus, c'est un tour mort. Cliquer le bouton d'arrêt (c'est l'onglet de l'agent, pas le poste de
  l'utilisateur), puis relancer par un message court qui renvoie à la consigne déjà dans le fil.
- **Conversation pleine** : une conversation qui a atteint sa longueur maximale **accepte encore les
  messages** et ne peut plus jamais y répondre ; rien ne l'annonce. La **relance courte** la fait avouer
  (« You've reached the maximum length for this conversation… ») :
  `document.body.innerText.includes('maximum length for this conversation')`. Continuer dans un chat neuf
  **du projet** (depuis la page du projet, ou sa ligne dans la barre latérale — voir la réserve du §1 sur
  l'URL racine) — le bouton « Start new chat » du bandeau renvoie vers la racine de ChatGPT, hors projet :
  le nouveau fil perdrait les instructions et les Sources. Et **donner la nouvelle adresse à
  l'utilisateur** : les prompts de boucle et les tâches planifiées codent l'URL en dur.
- **Un timeout de script n'est pas un onglet mort.** « Runtime.evaluate timed out after 45000ms… The renderer
  may be frozen » peut frapper un onglet neuf comme un ancien ; le message accuse le renderer, il ne
  prouve rien. Avant de recycler : re-tirer **une lecture minimale** (`document.querySelectorAll('<sélecteur>').length`,
  sans `innerText`, sans `await`, sans clic). Elle répond → l'onglet est vivant. Elle expire aussi → là,
  recycler. Les morts observées suivaient une navigation ou un clic sur « Stop », donc une page encore en
  train de se construire : c'était le moment, pas la lecture.
- **Content-script engorgé** : après huit ou neuf onglets ChatGPT ouverts en parallèle, les captures et
  `get_page_text` ont échoué sur tous les onglets (« Script injection timed out », « Page still loading »)
  alors que `tabs_context_mcp`, plus léger, répondait. Si le listing des onglets marche et que toute
  lecture échoue partout, ce n'est pas un onglet : c'est l'injection. Réduire le nombre d'onglets
  simultanés ; ne pas insister en boucle ; au-delà de deux ou trois tentatives espacées, le dire.
- **Recycler un onglet** (seulement après un échec d'écriture confirmé) : ouvrir un onglet sur l'URL de la
  conversation → **fermer l'ancien** (un onglet d'IA laissé ouvert continue de consommer) → vider le champ
  (un brouillon fantôme y est souvent restauré) → réécrire → envoyer → témoins. L'identifiant d'onglet
  change : relire le contexte des onglets avant tout geste suivant. Cela ne crée jamais un chat neuf et
  ne change jamais la cible.

## §5 Modèle, mode et effort (relevé du 09/10/2026, fil neuf, page d'accueil, mode « Chat »)

Les libellés dépendent du compte et de la date ; le geste, non.

- Le bouton du sélecteur (`aria-label="Select ChatGPT model"`) n'affiche que la réflexion (« Extra High »).
  Son menu porte un curseur (« Extra High, 4 of 5 » ; le cinquième cran : « Locked, opens access
  options ») et trois modèles à cocher : « GPT-6 » (coché), « GPT-5.6 Sol », « GPT-5.5 » (« Leaving on
  October 14 »). Le compte s'ouvrait déjà sur le plus récent et le cran le plus haut débloqué : on le lit
  quand même.
- **Sur une page cachée, le clic de l'outil n'ouvre pas ce menu** : envoyer au bouton `pointerdown`,
  `mousedown`, `pointerup`, `mouseup`, `click` ; le fermer par un `keydown` Escape envoyé à `document` (la
  touche de l'outil ne le ferme pas).
- **Preuve côté API** : `/backend-api/conversation/<id>` — le message de GPT porte `model_slug`
  (`gpt-6-thinking` au relevé) et `thinking_effort` (`max` pour un fil envoyé avec « Extra High »).
- **Mode Chat ou Work** : le sélecteur du haut de page ; la délégation se fait en Chat (règle 🚨 du
  `SKILL.md`).

## §6 Canvas, téléchargements, onglets

- **Le contenu d'un canvas** (document généré affiché en panneau latéral) n'est pas dans `<main>` :
  `get_page_text` ne le rend pas, mais il rend **le fil**, donc il sert à repérer quel document est le
  livrable. Le **premier clic sur la carte du document** la transforme en bouton « Open file » ; le canvas
  s'ouvre au clic suivant. Dès que le fichier dépasse quelques centaines de lignes, **le téléchargement
  bat la lecture par défilement** (un patch de 1 273 lignes : télécharger, puis lire en local). Plus simple
  encore : demander à GPT de reposter le contenu intégral **en texte brut dans le chat**.
- **Le bouton de téléchargement croisé au passage** se demande avant de cliquer ; le paquet que
  l'utilisateur a réclamé, lui, se télécharge sans redemander (`retour-paquet.md`).
- **Les Sources d'un projet** ne sont pas les fichiers créés dans un chat : un document généré dans un chat
  n'est pas visible des autres chats du projet tant qu'il n'est pas ajouté explicitement aux Sources.
- **Plusieurs sujets indépendants** : un onglet par sujet plutôt qu'en séquence (une réponse structurée
  prend couramment 3 à 6 minutes) — sans dépasser le seuil d'engorgement du §4.
- **Coordonnées d'écran** : la position du composer varie d'un onglet à l'autre ; ne jamais réutiliser les
  coordonnées d'une capture périmée, ni cliquer à l'aveugle. Les références `ref_N` périment dès que la page
  se re-rend : les relire juste avant chaque usage.

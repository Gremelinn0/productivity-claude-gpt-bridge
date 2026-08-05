# claude-gpt-bridge

Fait travailler un **projet ChatGPT que tu as déjà configuré**, depuis Claude Code — puis récupère
proprement ce qu'il produit, sans qu'un bout reste en arrière.

## Le problème que ça résout

Si tu as monté un projet ChatGPT avec une vraie méthodologie dans ses **instructions de projet**, cette
méthode est déjà écrite et déjà payée. La refaire raisonner en tokens Claude, c'est payer deux fois le
même travail. Ce plugin fait de Claude l'**opérateur** de ce projet : il ouvre, il nomme le sujet, il
relit, il boucle — il ne réécrit jamais le contenu à la place de ChatGPT.

Et surtout, il couvre le **sens retour**, celui qu'on oublie. ChatGPT vit dans un onglet : il n'a accès
ni à ton dépôt, ni à tes fichiers. Il **produit** un paquet ; seul Claude peut l'**installer**. Sans ce
retour, la collaboration fuit : du bon travail reste bloqué dans un fil de chat pendant que ton dépôt
et ton projet ChatGPT continuent de tourner sur des sources périmées — chacun croyant lire la bonne
version, et l'erreur ne se découvrant qu'au premier calcul faux.

| Sens | Ce qui circule |
|---|---|
| **Aller** — Claude → ChatGPT | un sujet à traiter ; ChatGPT réfléchit et rédige |
| **Retour** — ChatGPT → Claude | du durable consolidé, réinstallé aux **trois** endroits qui doivent rester d'accord |

## Le skill

| Skill | Ce qu'il fait |
|---|---|
| **`/claude-gpt`** | Pilote un projet ChatGPT existant via le navigateur : un chat neuf par sujet, lecture de la réponse, mise en parallèle sur plusieurs onglets. Puis, au retour, vérifie le paquet avant de l'installer (empreinte + taille), écrit au vrai chemin lu dans le manifeste, et finit par un reçu qui dit explicitement **ce qui n'a pas été appliqué**. |

Le skill embarque aussi les pièges rencontrés en conditions réelles : coordonnées du composer qui
bougent d'un onglet à l'autre, texte dupliqué après une reconnexion, contenu d'un document canvas
invisible à la lecture de page, et le signal qui distingue « un onglet coince » de « l'injection de
script est engorgée ».

### Les deux modes de coopération

Déléguer une **production** et faire avancer un **chantier commun** ne demandent pas la même relation.
Tant que ChatGPT produit un livrable que Claude installe, « Claude = les mains, ChatGPT = le cerveau »
est exact. Dès que les deux travaillent sur le même objectif durable, Claude cesse d'être un relai : il
pilote la progression, révise le rendu et tranche.

Deux contrats nomment ce qui change. **Un seul est actif à la fois** — ils attribuent les mêmes
décisions à des propriétaires différents, et les tenir ensemble produit un agent qui redemande l'avis
de ChatGPT sur un plan déjà décidé.

| Mode | Quand | Contrat |
|---|---|---|
| **Délégation pilotée** — *le défaut pour avancer* | un lot déjà cadré, dont la sortie attendue est identifiable | `skills/claude-gpt/references/delegation-pilotee.md` |
| **Réflexion croisée** — *l'exception* | une contradiction ou un choix structurel encore ouvert | `skills/claude-gpt/references/reflexion-croisee.md` |

Ces fichiers ne se chargent qu'au moment où le mode est actif : ils décrivent une **relation de
travail**, pas un outillage, et se tiennent seuls même si ton projet n'a pas de compétence de campagne
qui les appelle.

## Installation

```
/plugin marketplace add Gremelinn0/productivity-claude-gpt-bridge
/plugin install claude-gpt-bridge
```

Puis, une fois :

```
/claude-gpt-bridge-setup
```

Cette commande **vérifie** que le plugin peut réellement fonctionner chez toi (contrôle du navigateur,
session ChatGPT, projet cible), **t'aide à combler** ce qui manque, et **explique** comment le skill se
déclenche — et surtout pourquoi il ne s'est rien passé, le cas échéant. Son verdict est **mesuré** à
chaque ligne, jamais récité.

C'est une **commande**, pas une compétence : son contenu n'est chargé qu'au moment où tu l'invoques,
donc elle ne coûte rien à tes sessions le reste du temps. Elle n'écrit rien sur ta machine —
`--uninstall` te le confirmera plutôt que d'aller chercher des fichiers qui n'existent pas.

## ⚠️ Setup requis de ton côté — sans ça, le plugin ne fait rien

Ce plugin **pilote** un existant : il ne le crée pas. Trois prérequis, tous chez toi :

1. **Un projet ChatGPT déjà créé, avec ta méthode dans ses instructions de projet.** C'est le cœur : le
   skill ne réinjecte jamais la méthode dans ses messages, il compte sur le fait qu'elle est portée par
   le projet et héritée par chaque nouveau chat. Un projet ChatGPT vide ne donnera rien d'utile.
2. **Un contrôle du navigateur depuis Claude Code**, avec ta session ChatGPT **déjà connectée** (dans la
   pratique : l'extension Claude in Chrome). Le navigateur sandboxé interne ne convient pas — il faut la
   session authentifiée. Le skill dit quoi faire quand ce contrôle tombe en panne au lieu de boucler.
3. **Un abonnement ChatGPT** qui te donne accès aux projets. Tout l'intérêt du plugin est d'utiliser des
   crédits que tu paies déjà ailleurs.

Optionnel, pour le sens retour uniquement : **un dossier partagé généré par un script**, celui que tu
téléverses dans les Sources de ton projet ChatGPT. Si tu n'en as pas, le protocole du §7 se réduit
proprement à deux bouts (ton dépôt + les Sources ChatGPT) au lieu de trois — le skill le dit.

## 🌍 Portabilité

**Classe : ADAPTABLE.** Le skill ne contient aucun chemin, compte ou dépôt de son auteur, mais il
suppose une organisation de travail — voici laquelle, pour que tu saches quoi adapter :

| Point | État | Ce que tu dois adapter |
|---|---|---|
| **Chemins / comptes** | Aucun en dur | Rien |
| **Système** | Indépendant de l'OS (tout passe par le navigateur) | Rien |
| **Projet ChatGPT** | C'est **toi** qui le nommes ou le désignes par le contexte | Ton projet, ta méthode |
| **Registre de clôture** | Le skill n'en impose **aucun** : il logue là où ton projet suit déjà son travail | Si tu n'as pas de registre, une mention dans le fil suffit |
| **Dossier partagé (§7.5)** | Suppose un dossier **généré par un script**, jamais édité à la main | Ton script à toi ; sans lui, deux bouts au lieu de trois |
| **Skills voisins** | **Aucun requis** — le skill se suffit à lui-même | Rien |

Le skill est rédigé en français.

## Sa philosophie, en une ligne

L'opérateur n'est pas le cerveau : Claude tient la discipline (un sujet, un chat, on relit avant
d'envoyer, on vérifie avant d'écraser), ChatGPT fournit la pensée.

# Grist Fiche

> Vue « fiche » détaillée et éditable pour [Grist](https://www.getgrist.com/), en un seul fichier HTML autonome, sans étape de build.
>
> *An editable detail view for Grist, as a single self-contained HTML custom widget.*

Ce widget affiche **un enregistrement à la fois** sous forme de fiche lisible : titre, sous-titre, pastilles, propriétés en colonnes et blocs de texte long. Tout est **éditable au clic**, les références sont **ouvrables** pour naviguer d'une fiche à l'autre, et les pièces jointes s'ouvrent dans une **visionneuse intégrée**.

<img width="1037" height="895" alt="image" src="https://github.com/user-attachments/assets/ca7e31fb-c240-4ef6-a1ef-680c3f6f420e" />


## Fonctionnalités

- **Édition au clic**, champ par champ : texte, nombre, booléen, date, choix, choix multiples, références et références multiples.
- **Éditeur markdown** (WYSIWYG) pour les champs texte longs, avec enregistrement automatique.
- **Références ouvrables** : un clic sur une pastille ouvre l'enregistrement lié dans la fiche (avec bouton « Retour »). Fonctionne aussi pour les colonnes formule de type référence.
- **Références multiples en sous-tableau** ou en pastilles, colonnes configurables.
- **Pièces jointes** : miniatures pour les images, pastilles pour les autres fichiers, et **visionneuse intégrée** pour images et PDF (téléchargement possible).
- **Image d'en-tête** (logo / photo de profil) depuis une colonne pièce jointe ou une colonne URL.
- **En-tête configurable** : choix des colonnes titre, sous-titre et tags.
- **Réorganisation** des champs par glisser-déposer, et **repli** des champs secondaires dans une section « Autres champs ».
- **Liens d'ancrage partageables** : un bouton « Lien » copie l'URL qui ouvre la fiche sur la bonne ligne.
- **Copier / coller le paramétrage** d'un widget à l'autre (même table).
- Styles conditionnels de Grist (couleurs, gras…) repris automatiquement.
- Interface en français.

## Installation

Le widget est **un seul fichier** : `grist-fiche-widget.html`. Aucune dépendance à installer, aucune compilation.

### URL prête à l'emploi

Le widget est publié sur GitHub Pages depuis la branche `main` — rien à héberger :

```
https://aurelienc.github.io/grist-fiche-widget/
```

(équivalent à `https://aurelienc.github.io/grist-fiche-widget/grist-fiche-widget.html`, l'URL courte redirige.)

Coller cette URL à l'étape 2 ci-dessous, puis passer directement à *Access level : Full*.

### Ou héberger soi-même

1. **Héberger le fichier** sur n'importe quel hébergement de fichiers statiques accessible en HTTPS (GitHub Pages, Netlify, un dossier statique sur un serveur, etc.). Noter son URL publique, par exemple `https://exemple.org/grist-fiche-widget.html`.
2. Dans le document Grist, ajouter un widget **Custom** sur la page :
   - *Add Widget to Page* → **Custom**.
   - Dans le panneau du widget : **Custom URL**, coller l'URL du fichier.
   - **Access level : Full** (le widget lit les métadonnées des tables et les valeurs pour l'édition).
3. Sélectionner une ligne (par exemple via un widget tableau/liste à gauche, lié au widget fiche) : la fiche s'affiche.

## Configuration

Ouvrir le panneau **« Champs »** (roue crantée, en haut à droite de la fiche). Les réglages sont **mémorisés par widget** (donc par page) dans les options du widget Grist, et **par table** : le panneau configure toujours la table affichée, dont le nom est rappelé en tête du panneau. Régler les champs d'un enregistrement lié ouvert dans le widget (un contact depuis une entreprise, par exemple) ne touche donc pas au paramétrage de la table principale. On y trouve :

- **Titre / Sous-titre / Tags** : les colonnes promues dans l'en-tête.
- **Image d'en-tête** : une colonne pièce jointe ou une colonne URL d'image.
- **Ordre des champs** : glisser-déposer.
- **Replier** : cocher un champ pour l'envoyer dans « Autres champs ».
- **Références multiples** : affichage en *labels* ou en *tableau*, et colonnes affichées.
- **Modèle de lien** : voir ci-dessous.
- **Copier / Coller la config** : pour reprendre le même paramétrage sur une autre page de la même base. La copie emporte **toutes** les tables paramétrées dans ce widget (table principale et sous-tables ouvertes en navigation) ; un seul collage les restaure toutes. Le modèle de lien, propre à chaque page, n'est jamais copié.

## Liens partageables (ancre vers une ligne)

Grist sait générer un **lien d'ancrage** qui ouvre une ligne précise (`…/p/PAGE#a1.sSECTION.rLIGNE`). Le widget ne peut pas fabriquer ce lien seul (il ne connaît pas la section depuis son iframe) ; il faut donc lui fournir un **modèle** :

1. Dans le tableau de gauche, sélectionner une cellule d'une ligne, puis **Ctrl+Shift+A** (⌘⇧A) — ou clic droit → *Copier le lien d'ancrage*.
2. Ouvrir **« Champs »**, coller ce lien dans **« Modèle de lien »**.
3. Le widget remplace automatiquement le numéro de ligne pour chaque fiche ; le bouton **« Lien »** copie alors le lien de la fiche courante.

À faire **une fois par page**. Le lien ne donne pas l'accès au document : le destinataire doit déjà y avoir accès.

## Aperçu local (sans Grist)

Il suffit d'ouvrir `grist-fiche-widget.html` dans un navigateur : hors de Grist, le widget affiche un **enregistrement de démonstration** qui montre la plupart des fonctionnalités.

## Mise à jour / cache

Les hébergeurs statiques mettent le fichier en cache. Après remplacement du fichier, ajouter ou incrémenter un paramètre de version dans la *Custom URL* de Grist, par exemple `…/grist-fiche-widget.html?v=2`. Le panneau « Champs » affiche en bas la **version** déployée pour vérifier.

GitHub Pages sert le fichier avec `Cache-Control: max-age=600` (non configurable) : après un push, compter quelques minutes — ou forcer avec le paramètre `?v=`.

## Sécurité

- Le markdown est rendu puis **nettoyé avec [DOMPurify](https://github.com/cure53/DOMPurify)**.
- Toutes les valeurs de table sont **échappées** avant insertion dans le HTML ; les couleurs issues des options de colonnes sont **validées** (pas d'injection CSS).
- Les URL de liens hypertextes bloquent les schémas dangereux (`javascript:`, `data:`, `vbscript:`).
- Les pièces jointes sont servies via un **jeton d'accès temporaire** fourni par l'API Grist (`getAccessToken`), en lecture seule.
- Aucune donnée n'est envoyée ailleurs que vers l'instance Grist ; pas de traceur, pas d'appel tiers hormis les bibliothèques CDN (marked, DOMPurify, Toast UI Editor).

## Dépendances (CDN)

Chargées à l'exécution depuis des CDN publics : [marked](https://github.com/markedjs/marked) (markdown), [DOMPurify](https://github.com/cure53/DOMPurify) (nettoyage HTML), [Toast UI Editor](https://github.com/nhn/tui.editor) (éditeur markdown WYSIWYG). Pour un fonctionnement hors-ligne ou souverain, il est possible de les héberger soi-même et d'adapter les balises `<script>`/`<link>`.

## Développement

Il n'y a **pas de build**. Tout est dans `grist-fiche-widget.html` (HTML + CSS + JS). Vérification rapide de la syntaxe JS :

```bash
# extrait le dernier <script> et vérifie la syntaxe
node --check <(sed -n 's/.*//; /<script>/,/<\/script>/p' grist-fiche-widget.html)
```

Contributions bienvenues via *issues* et *pull requests*.

## Licence

Copyright (C) 2026 Aurélien Cadiou

Distribué sous licence **[GNU Affero General Public License v3.0](LICENSE)** (AGPL-3.0).

Toute redistribution, modification ou mise à disposition via un réseau (usage en ligne inclus) doit rester sous AGPL-3.0 et donner accès au code source correspondant.

Code source : <https://github.com/AurelienC/grist-fiche-widget>

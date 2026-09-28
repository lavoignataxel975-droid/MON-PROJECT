# Handoff: Configurateur « Mon projet »

## Overview
Section « Mon projet » refondue en configurateur interactif en 4 étapes. Tout se passe dans **une seule grande fenêtre fixe**. À l'intérieur, une piste horizontale de 4 panneaux glisse vers la gauche d'une étape à l'autre. La page ne défile jamais verticalement entre les étapes. Le prix total se met à jour en direct.

## About the Design Files
`Mon projet.dc.html` est une **référence de design en HTML** : un prototype du rendu et du comportement attendus, pas du code de production à copier tel quel. La tâche consiste à **le recréer dans l'environnement existant du site** (React, Vue, Astro…) avec ses conventions. S'il n'y a pas d'environnement, choisir le framework le plus adapté. Pour voir le prototype, ouvrez le fichier dans un navigateur, à côté de `support.js`.

## Fidelity
**High-fidelity.** Couleurs, typographies, espacements et animations sont définitifs. Seule réserve : l'identité visuelle du site existant n'était pas disponible. Si le site a déjà des tokens (couleurs, polices), les substituer en gardant la structure, la hiérarchie et le mouvement.

## Layout global
- Section : `max-width: 1440px`, centrée, padding `clamp(40px,7vw,96px)` vertical et `clamp(12px,3vw,48px)` horizontal. Fond de page `#f2f0eb`.
- En-tête de section (flex wrap, aligné en bas, `justify-content: space-between`) :
  - Surtitre « CONFIGURATEUR » en JetBrains Mono 12px, `letter-spacing .14em`, couleur `#6f6b63`.
  - H2 « Mon projet » en Instrument Serif 400, `clamp(48px,7vw,104px)`, `line-height .92`, `letter-spacing -.02em`.
  - Texte à droite (max 380px, 16px/1.5, `#55514a`) : « Composez votre site en quatre étapes. Le prix se met à jour à chaque choix. »
- **Fenêtre (viewport)** : fond `#141311`, `border-radius clamp(20px,2.4vw,32px)`, `min-height: min(86vh, 860px)`, `overflow: hidden`, `box-shadow 0 40px 80px -40px rgba(20,19,17,.55)`. Colonne flex : barre du haut puis scène.
  - **Barre du haut** : padding `clamp(16px,2vw,24px) clamp(16px,3vw,40px)`, bordure basse `1px rgba(255,255,255,.08)`.
    - Gauche : 4 indicateurs d'étape (flex, gap 6px, largeur égale). Chacun = une barre de 3px (fond `rgba(255,255,255,.12)`, remplissage accent, largeur 0 → 100% en 0,6 s) + « 0X » en Mono 11px + un libellé de 13px (Projet / Fonctionnalités / Options / Devis). Opacité : 1 pour l'étape active, 0,7 pour les étapes passées, 0,4 pour les suivantes. Les étapes passées sont cliquables.
    - Droite : « TOTAL » (Mono 11px, `#a29d93`) + montant en Instrument Serif 36px, couleur accent.
  - **Scène** : `flex:1; overflow:hidden`. Elle contient la **piste** : `display:flex; width:100%; transform: translateX(-step*100%)`. Chaque panneau fait `flex: 0 0 100%`, avec un padding `clamp(12px,2.6vw,36px)` autour d'une **carte** : fond `#1d1c19`, bordure `1px rgba(255,255,255,.08)`, rayon `clamp(16px,2vw,24px)`, padding `clamp(24px,4vw,56px)`.

## Screens
Chaque panneau affiche un grand numéro « 0X » en Instrument Serif `clamp(72px,10vw,160px)`, `line-height .8`, couleur accent, et un titre H3 en Instrument Serif 400 `clamp(34px,4vw,56px)`, `line-height 1`.

### Étape 1 — « Présentez votre projet »
- Grille `repeat(auto-fit, minmax(min(100%,360px),1fr))`, gap `clamp(24px,4vw,64px)`.
- Gauche : numéro, titre, texte (16px/1.55, `#a29d93`, max 420px) : « Votre activité, vos objectifs, votre public. Ajoutez des photos pour que l'on cerne votre univers. » En bas : pastille « +500 € » (Mono 14px, forme pilule, fond `rgba(255,255,255,.06)`, bordure `rgba(255,255,255,.1)`) + « Site vitrine sur mesure ».
- Droite :
  - Libellé « VOTRE ENTREPRISE ».
  - Textarea : min 180px de haut, fond `#141311`, bordure `rgba(255,255,255,.12)`, rayon 16px, padding 18px, 16px/1.5. Au focus, la bordure passe à la couleur accent.
  - « PHOTOS · n/3 » : grille de 3 colonnes carrées (gap 10px, rayon 14px). Vignettes en `object-fit: cover` avec un bouton × rond de 28px (fond `rgba(20,19,17,.8)`). La case « + Ajouter » a une bordure pointillée `rgba(255,255,255,.22)` et un fond hachuré subtil ; au survol, bordure accent. Elle disparaît à 3 photos.
  - Bouton « Continuer → » aligné à droite (voir Boutons). Désactivé (opacité 0,4) tant que la description est vide, avec l'indication « Décrivez votre projet pour continuer ».

### Étape 2 — « Quelles fonctionnalités souhaitez-vous ? »
- En-tête flex wrap : numéro + titre + sous-titre « Plusieurs choix possibles. Les deux formules Click & Collect s'excluent. »
- Grille de cartes `repeat(auto-fit, minmax(min(100%,260px),1fr))`, gap 16px. Chaque carte : min 260px de haut, rayon 20px, padding `clamp(22px,2.4vw,32px)`. On y trouve un tag Mono 11px en majuscules (opacité 0,7), un cercle de 26px avec ✓ quand la carte est sélectionnée, le nom en Instrument Serif `clamp(28px,2.6vw,38px)`, une description de 15px (opacité 0,75) et le prix en Mono 16px en bas. Au survol : `translateY(-4px)`.
  - CMS — « Modifiez textes, images et pages vous-même, sans développeur. » — +300 €
  - Click & Collect sans paiement — « Vos clients réservent en ligne et règlent sur place. » — +300 €
  - Click & Collect avec paiement — « Commande et paiement sécurisé en ligne, retrait en boutique. » — +600 €
- États : non sélectionné = fond `#141311`, texte `#f2f0eb`, bordure `rgba(255,255,255,.12)`. Sélectionné = fond, bordure et texte inversés sur l'accent (texte `#141311`). Transition de 0,35 s.
- Pied : « ← Retour » (bouton secondaire) à gauche, « Continuer → » à droite.

### Étape 3 — « Ajoutez des options »
- Sous-titre : « Traduction complète du site, en plus du français. +150 € par langue. »
- Grille `repeat(auto-fill, minmax(min(100%,200px),1fr))`, lignes d'au moins 120px, gap 12px. Tuiles : rayon 18px, padding 20px 22px. En haut : code langue (Mono 13px) + cercle ✓ de 22px. En bas : nom en Instrument Serif 28px + « +150 € ». Mêmes états de sélection que l'étape 2.
- Langues : Anglais (EN), Espagnol (ES), Italien (IT), Allemand (DE), Portugais (PT), Néerlandais (NL). Sélection multiple.
- Pied : Retour, puis « Voir mon devis → ».

### Étape 4 — « Votre devis »
- Grille `repeat(auto-fit, minmax(min(100%,380px),1fr))`.
- Gauche : lignes du récapitulatif, séparées par des filets `rgba(255,255,255,.1)`. Chaque ligne = libellé 16px + sous-libellé 13px `#a29d93` à gauche, prix en Mono 14px à droite.
  - « Site vitrine sur mesure » (sous-libellé : « n photo(s) jointe(s) » ou « Présentation du projet ») — 500 €
  - Une ligne par fonctionnalité — +prix
  - « Traduction » (sous-libellé : liste des langues) — +150 € × n
  - « TOTAL ESTIMÉ HT » + montant en Instrument Serif `clamp(56px,6vw,88px)`, couleur accent.
- Droite (alignée en bas) : texte « Laissez vos coordonnées : nous revenons vers vous sous 48 h avec un devis détaillé. », puis les champs Nom et prénom, Email et Téléphone (fond `#141311`, rayon 14px, padding 18px). Ensuite le grand bouton **« Envoyer ma demande → »** : pleine largeur, padding 26px 32px, rayon 20px, fond accent, 600, `clamp(18px,1.6vw,22px)`. Il est désactivé tant que l'email ne contient pas « @ ». Enfin, le lien texte « ← Modifier mes options ».
- Après l'envoi : carte de confirmation (fond `rgba(255,255,255,.04)`, rayon 20px) avec une pastille ✓ de 48px en accent, « Demande envoyée » en Serif 40px, « Merci {prénom}. Nous revenons vers vous sous 48 h. » et un bouton « Nouveau projet » qui réinitialise tout.

### Boutons
- Primaire : pilule, padding 18px 28px, fond accent, texte `#141311`, 16px, 600. Au survol : `translateX(4px)`.
- Secondaire : pilule, padding 16px 22px, bordure `rgba(255,255,255,.16)`, transparent, texte `#f2f0eb`.

## Interactions & Behavior
- **Transition entre étapes** : `transform: translateX(-step*100%)` sur la piste, **900 ms**, `cubic-bezier(.77,0,.18,1)`. Le panneau actif est à opacité 1 et `scale(1)`. Les autres sont à opacité 0,35 et `scale(.94)`, avec la même durée. Le panneau sortant part entièrement à gauche et le suivant entre par la droite, au même emplacement.
- Aucun changement de hauteur ni de défilement de page entre les étapes : la hauteur de la fenêtre est fixe (celle du panneau le plus haut sur mobile).
- Retour : étape − 1. Les indicateurs d'étapes passées dans la barre du haut permettent aussi d'y revenir. Pas de saut vers une étape future.
- Exclusivité : sélectionner un Click & Collect désélectionne l'autre.
- Photos : 3 au maximum. `input type=file accept=image/*` multiple, tronqué aux places restantes. Aperçu via `URL.createObjectURL`.
- Total = 500 + somme des fonctionnalités + 150 × nombre de langues. Format `toLocaleString('fr-FR') + ' €'`.
- Envoi : dans le prototype, bascule simplement vers la confirmation. **À brancher** sur l'endpoint réel (payload ci-dessous). Prévoir un état de chargement et un état d'erreur.
- Accessibilité à ajouter en production : gestion du focus à chaque changement d'étape (focus sur le H3 du nouveau panneau), `aria-hidden` / `inert` sur les panneaux inactifs, `aria-pressed` sur les cartes sélectionnables, et respect de `prefers-reduced-motion` (fondu court au lieu du glissement).

## Responsive
- Desktop : 2 colonnes (étapes 1 et 4), 3 cartes en ligne (étape 2).
- Tablette et mobile : les grilles passent en une colonne via `auto-fit/minmax` ; les tailles de police et les paddings suivent `clamp()`. Les libellés d'étape sont tronqués avec une ellipse. Le glissement horizontal reste identique.

## State
```
step: 0..3
desc: string
photos: {id, url, file}[]  (≤3)
feats: ('cms'|'cc'|'ccp')[]  (cc et ccp exclusifs)
langs: ('en'|'es'|'it'|'de'|'pt'|'nl')[]
name, email, phone: string
sent: boolean
```
Payload suggéré : `{ description, photos[], features[], languages[], total, contact: {name, email, phone} }`.

## Design Tokens
- Fond de page `#f2f0eb` · Encre `#161513` · Texte secondaire (fond clair) `#55514a`, `#6f6b63`
- Fenêtre `#141311` · Carte `#1d1c19` · Champs / cartes non sélectionnées `#141311`
- Texte sur fond sombre `#f2f0eb` · Texte atténué `#a29d93`
- Bordures `rgba(255,255,255,.08 / .10 / .12 / .16 / .22)`
- Accent `oklch(0.82 0.13 85)` (≈ `#e3b54a`), texte sur accent `#141311` · Survol des liens `#8a6a1c`
- Rayons : 999 (pilules), 14, 16, 18, 20, 24 (carte), 32 (fenêtre)
- Polices (Google Fonts) : Instrument Serif 400 (titres, prix), DM Sans 400/500/600 (texte), JetBrains Mono 400/500 (libellés, prix)
- Easing : `cubic-bezier(.77,0,.18,1)`

## Assets
Aucune image fournie. Les photos sont téléversées par l'utilisateur.

## Files
- `Mon projet.dc.html` — le prototype complet (markup + logique dans la classe `Component`)
- `support.js` — le runtime nécessaire pour ouvrir le prototype dans un navigateur

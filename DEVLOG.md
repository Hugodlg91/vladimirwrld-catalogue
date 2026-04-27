# DEVLOG — Catalogue Véhicules PDM

## Stack
- HTML5 / CSS3 / JavaScript vanilla
- Police Inter via Google Fonts (fallback : system-ui)
- Aucun build tool, aucune dépendance npm
- Hébergement : Cloudflare Pages (déploiement auto depuis GitHub)

## Structure du fichier

```
catalogue-vehicules-pdm/index.html
├── <head>          → meta, Google Fonts, tout le CSS dans <style>
├── <header>        → logo + badge sticky
├── <main>
│   ├── #search     → barre de recherche live
│   ├── #cat-tabs   → onglets Sport Classique / Super / Sportifs
│   ├── #panel-sc   → contenu Sport Classique
│   ├── #panel-super→ contenu Super
│   └── #panel-sport→ contenu Sportifs
├── <footer>
└── <script>        → JS vanilla (switch catégorie, filtres, recherche, compteur)
```

Chaque panel contient :
- `.stats-bar` → 4 cartes (total, moins cher, plus cher, nom catégorie)
- `.tier-filters` → pills de filtre par gamme
- `.counter` → compteur dynamique de véhicules affichés
- `N × .tier-section` → une section par gamme de prix, avec header + liste de `.veh-row`

## Ajouter un véhicule

1. Trouver la `.tier-section` correspondante dans le bon panel (ex : `#sp-mid` pour Sportifs Milieu)
2. Ajouter une ligne dans `.tier-body` :

```html
<div class="veh-row" data-name="Nom du véhicule">
  <span class="veh-name">Nom du véhicule</span>
  <span class="veh-price">XXX XXX $</span>
</div>
```

3. Mettre à jour le compteur dans `.tier-count` du header de la section (+1)
4. Mettre à jour `.stat-val` (total véhicules) dans la `.stats-bar` du panel (+1)
5. Mettre à jour le nombre entre parenthèses dans l'onglet `.cat-tab` correspondant

## Ajouter une catégorie

1. Ajouter un onglet dans `#cat-tabs` :
```html
<button class="cat-tab" data-cat="muscle">Muscle <small style="opacity:.6;font-weight:400">(N)</small></button>
```

2. Créer un panel `#panel-muscle` en copiant la structure d'un panel existant

3. Enregistrer la liste des tiers dans `tierMap` dans le `<script>` :
```js
const tierMap = {
  // ... existants ...
  muscle: ['mu-entry','mu-mid','mu-high','mu-prestige']
};
```

4. Ajouter un `<div class="counter" id="counter-muscle"></div>` dans le panel

5. Ajouter `'muscle'` dans le `forEach` d'init des compteurs en bas du script

## Déploiement

Push sur `main` → Cloudflare Pages redéploie automatiquement en ~30 secondes.
URL production : vladimirwrld.com/catalogue-vehicules-pdm

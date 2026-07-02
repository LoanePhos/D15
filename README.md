# README — Site DCN (Mémoires de diplôme)

Coucou les futur.es petit.es DCN :) Voici un petit document qui explique comment mettre à jour le site à chaque nouvelle promotion : ajout d'une nouvelle année de mémoires, ajout de l'édition digitale, mise à jour de la liste des élèves, et ce qu'il ne faut surtout jamais toucher.

---

## 1. Structure générale du site

```
DCN/
├── index.html                  → page d'accueil
├── digital.html                → page listant les éditions "digital-N"
├── digital-1/ digital-2/ ...   → une édition digitale = un dossier
│   └── digitalN.html + pages/  → images des pages + preview.jpg
├── memoires.html                → page listant les années
├── memoires_2024.html
├── memoires_2025.html
├── memoires_2026.html          → une page de listing par année
├── memoires/
│   ├── 2024/ 2025/ 2026/ ...   → un dossier par année
│   │   └── NomDuCollectif/     → un dossier par collectif d'élèves
│   │       └── nom-du-memoire/ → un dossier par mémoire
│   │           ├── xxx.html    → la liseuse (flipbook)
│   │           └── pages/      → 1.jpg, 2.jpg, ... + preview.jpg
│   └── shared/                 → ⚠️ bibliothèque commune, voir §5
├── fonts/, images/, liste_eleves.xlsx, d15_style.css, index_style.css, memoires_style.css
```

Chaque mémoire est identifié par le chemin :
`memoires/ANNEE/COLLECTIF/nom-du-memoire/`

---

## 2. Ajouter une nouvelle année de mémoires

### Étape 1 — Créer la page de listing de l'année
- Dupliquer la dernière page `memoires_2026.html` → la renommer `memoires_2027.html`.
- Mettre à jour les titres, les vignettes et les liens vers chaque mémoire.
- Ajouter un lien vers cette nouvelle page depuis `memoires.html`.

### Étape 2 — Créer l'arborescence de l'année
Dans `memoires/`, créer un nouveau dossier `2027/`.

Dedans, créer **un dossier par collectif**, par exemple :
```
memoires/2027/moncollectif/
```

Dans chaque dossier de collectif, créer **un dossier par mémoire** (un par élève) :
```
memoires/2027/moncollectif/titre-du-memoire/
```

### Étape 3 — Exporter le mémoire en images (⚠️ étape obligatoire, anti-vol)
On ne dépose **jamais le PDF du mémoire tel quel** sur le site : il serait trop facile à télécharger et à copier intégralement.

À la place :
1. Exporter chaque page du PDF en **une image JPG par page**. Vous pouvez le faire depuis **Adobe Acrobat**. 
2. Nommer les fichiers `1.jpg`, `2.jpg`, `3.jpg`, … dans l'ordre des pages, et les placer dans un sous-dossier `pages/`. Pour vous aider à renommer tout ça plus vite, vous pouvez utiliser l'application **Bulk Rename Utility**.
3. Générer la **vignette de preview** `preview.jpg` à l'aide du **fichier InDesign prévu à cet effet** (le gabarit de preview), et la placer aussi dans `pages/`.

Résultat attendu :
```
memoires/2027/moncollectif/titre-du-memoire/
├── titre-du-memoire.html
└── pages/
    ├── 1.jpg
    ├── 2.jpg
    ├── ...
    ├── N.jpg
    └── preview.jpg
```

### Étape 4 — Créer la page HTML de la liseuse
Ne vous embêtez pas à récrire la page, copiez juste celle d'une autre année en faisant bien attention à vos chemins et vos noms de fichiers. 

1. Copier par exemple `memoires/2026/ludd/3_out-design/outdesign.html` vers `memoires/2027/moncollectif/titre-du-memoire/titre-du-memoire.html`.
2. Mettre à jour le `<title>`.
3. Vérifier les chemins relatifs vers `shared/` (`../../../shared/...`) — ils ne changent pas tant que la profondeur du dossier reste la même (année/collectif/mémoire/fichier.html = 3 niveaux avant `memoires/shared`).
4. Mettre à jour le lien de la croix de fermeture, qui doit pointer vers la page de listing de l'année :
   ```html
   <a href="../../../../memoires_2027.html" class="close-cross">
   ```
   (4 niveaux : mémoire → collectif → année → DCN).
5. **Le plus important : mettre à jour le nombre de pages** dans le script, pour qu'il corresponde exactement au nombre d'images dans `pages/` :
   ```js
   pages: 128,   // → remplacer par le nombre réel d'images
   ```

### Étape 5 — Lier le mémoire depuis la page de l'année
Dans `memoires_2027.html`, ajouter une vignette (utilisant `pages/1.jpg`) qui pointe vers `titre-du-memoire.html`.

---

## 3. Ajouter une nouvelle édition digitale

Le fonctionnement est **exactement le même** que pour les mémoires :

1. Créer un nouveau dossier à la racine, par exemple `digital-3/`.
2. Exporter le document en images page par page dans `digital-3/pages/` (`1.jpg`, `2.jpg`, …), toujours pour éviter que le document original soit récupérable.
3. Générer `preview.jpg` via le fichier InDesign de gabarit, dans le même dossier `pages/`.
4. Copier le HTML d'une édition digitale existante (ex. `digital-1/digital1.html`) vers `digital-3/digital3.html`, et l'adapter comme à l'étape 4 ci-dessus (nombre de pages, chemins, lien de fermeture vers `digital.html`).

---

## 4. Mettre à jour la liste des anciens élèves

Le fichier `liste_eleves.xlsx` à la racine du repo recense les élèves. À chaque nouvelle promotion :
- Ajouter les nouveaux noms/année/contact dans ce fichier Excel.
- Descendez l'ensemble de la liste pour rajouter les nouveaux élèves tout en haut.

---

## 5. ⚠️ Le dossier `memoires/shared/` — NE PAS TOUCHER

`memoires/shared/` contient toutes les librairies utilisées par **toutes** les liseuses, de toutes les années, présentes et futures :
- `lib/turn.js`, `turn.html4.js`, `zoom.js` (moteur de flipbook et de zoom)
- `extras/jquery*.js`, `modernizr*.js`, `jgestures*.js`
- `css/magazine.css`, `jquery.ui.css`
- `pics/` (flèches, icônes de zoom, croix de fermeture)

Toute modification dans ce dossier a un impact **rétroactif sur tous les mémoires déjà en ligne**, quelle que soit leur année. Sauf correction de bug avérée et testée sur plusieurs pages, ne pas modifier ces fichiers. Pour un nouveau mémoire ou une nouvelle édition, on ne fait que **copier une page HTML existante** qui pointe déjà vers `shared/` — il n'y a jamais besoin d'y toucher.
De la même façon, vous n'avez pas à modifier les css qui sont déjà connectés automatiquement à toutes les pages ! (surtout le memoires.css)

---

## 6. Checklist rapide pour une nouvelle année

- [ ] Dupliquer et adapter `memoires_ANNEE-1.html` → `memoires_ANNEE.html`
- [ ] Créer `memoires/ANNEE/`
- [ ] Créer un dossier par collectif
- [ ] Créer un dossier par mémoire
- [ ] Exporter chaque mémoire en images JPG par page (jamais le PDF brut)
- [ ] Générer `preview.jpg` via le fichier InDesign de gabarit
- [ ] Copier/adapter le HTML d'une liseuse existante (titre, nombre de pages, chemins, lien de fermeture)
- [ ] Ajouter les vignettes/liens dans `memoires_ANNEE.html`
- [ ] Mettre à jour `liste_eleves.xlsx`
- [ ] Ne rien modifier dans `memoires/shared/`
      
---

## 7. Bravo, vous avez mis à jour le site !

Merci d'avoir mis à jour le site :))) bon courage pour la suite les loulous :)))
<br> `Loane et la promo 2026`

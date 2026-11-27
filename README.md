# Orion AI — Site

Site one-page d'Orion AI (assistant IA, Control Panel, Orion Core) : HTML autonome,
sans build, sans dépendance npm. Page d'entrée : `index.html`.

## Architecture

- `index.html` — page unique (styles et scripts inline, sectionnés par `id`).
- `assets/` — médias statiques (pochette utilisée par l'ambiance lumineuse).

## Fond dynamique « Orion Atmosphere »

Fond ambiant plein écran inspiré des lecteurs musicaux plein écran : une seule
surface de couleur continue, sans formes visibles, qui évolue lentement.

- Rendu WebGL (`<canvas id="orion-atmosphere">`, script `orion-atmosphere-script`) :
  bruit FBM à distorsion de domaine, évolution non répétitive (procédé
  Ornstein–Uhlenbeck par canal : luminosité, contraste, saturation, teinte).
- Palette extraite des couleurs dominantes de `assets/album-darkside.png`
  (méta `<meta name="orion-atmosphere-source">`), avec repli sur la couleur du
  thème (`orion:brandchange`).
- Le logo hérite de la couleur ambiante via les variables CSS `--atm-rgb`
  et `--atm-hi-rgb`, mises à jour depuis le rendu.
- Qualité : `window.OrionAtmosphere.setQuality('high' | 'balanced' | 'lite' | 'off')`,
  adaptation automatique aux appareils (`html.lowfx`, `html.ultra-lite`,
  `prefers-reduced-motion`), repli dégradé CSS si WebGL indisponible.
- Autres API : `setColor(hex)`, `setImage(url)`, `getAmbientColor()`.

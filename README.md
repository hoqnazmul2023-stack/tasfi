# Orion AI — Site

Site one-page d'Orion AI (assistant IA, Control Panel, Orion Core) : HTML autonome,
sans build, sans dépendance npm. Page d'entrée : `index.html`.

## Ouvrir le site directement

Aucun serveur ni installation n'est nécessaire :

1. Téléchargez le dépôt : bouton **Code → Download ZIP** (ou récupérez `index.html` seul).
2. Décompressez le dossier où vous voulez.
3. Double-cliquez sur `index.html` — le site et le fond ambiant fonctionnent
   immédiatement dans le navigateur (la pochette est encodée dans le fichier,
   l'ambiance ne dépend donc d'aucun fichier externe).

Sans internet, seuls la police et le visualiseur 3D (CDN) sont ignorés ;
tout le reste, dont l'ambiance, fonctionne hors ligne.

## Architecture

- `index.html` — page unique autonome (styles, scripts et pochette encodée en base64 inline, sectionnés par `id`).
- `assets/` — médias statiques (source d'origine de la pochette utilisée par l'ambiance).

## Fond dynamique « Orion Atmosphere »

Fond ambiant plein écran inspiré des lecteurs musicaux plein écran : une seule
surface de couleur continue, sans formes visibles, qui évolue lentement avec de
nettes nuances de couleur entre les régions (zones plus claires, plus sombres ou
tirant vers une teinte voisine, sans aucune forme identifiable).

- Rendu WebGL (`<canvas id="orion-atmosphere">`, script `orion-atmosphere-script`) :
  bruit FBM à distorsion de domaine, dérive spatiale de teinte liée à la
  « lumière de la pièce », évolution non répétitive (procédé
  Ornstein–Uhlenbeck par canal : luminosité, contraste, saturation, teinte),
  quatre couleurs de palette interpolées en douceur (`u_colA`…`u_colD`).
- Palette extraite des couleurs dominantes de la pochette, encodée inline dans
  `index.html` (`window.ORION_ATMOSPHERE_SOURCE`) ou référencée via la méta
  `<meta name="orion-atmosphere-source">`, avec repli sur la couleur du
  thème (`orion:brandchange`).
- Le logo hérite de la couleur ambiante via les variables CSS `--atm-rgb`
  et `--atm-hi-rgb`, mises à jour depuis le rendu.
- Qualité : `window.OrionAtmosphere.setQuality('high' | 'balanced' | 'lite' | 'off')`,
  adaptation automatique aux appareils (`html.lowfx`, `html.ultra-lite`,
  `prefers-reduced-motion`), repli dégradé CSS si WebGL indisponible.
- Autres API : `setColor(hex)`, `setImage(url)`, `getAmbientColor()`.

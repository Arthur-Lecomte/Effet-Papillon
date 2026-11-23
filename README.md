# Effet Papillon

Simulation 2D d'un double pendule chaotique, destinée à être intégrée
comme démo sur [arthurlecomte.fr](https://arthurlecomte.fr) sous
`/demos/effet-papillon/`. Plusieurs pendules partent de conditions
initiales quasi identiques et leurs trajectoires divergent rapidement :
c'est l'effet papillon, rendu visible par les traînées colorées et par un
graphe de l'écart entre pendules au cours du temps (échelle log).

## Utiliser la démo

- **Glisser une masse** (souris ou doigt) pour choisir la position de
  départ ; au relâchement, tous les clones repartent de là.
- **Au clavier** (canvas focalisé) : flèches gauche/droite pour le premier
  angle, haut/bas pour le second, Maj pour affiner, espace pour
  lecture/pause.
- Curseurs : nombre de pendules (2 à 30), écart initial entre clones
  (10⁻⁸ à 10⁻² rad, appliqué au premier angle), vitesse (×0,1 à ×2).
- Les réglages sont mémorisés dans `localStorage` (clés préfixées
  `effet-papillon:`).

## Développer

```sh
npm ci
npm run dev       # serveur de développement
npm run build     # fichiers finaux dans dist/
```

## Choix techniques

- Équations classiques du double pendule (masses et longueurs unitaires,
  g = 9,81), intégrées en RK4 à pas fixe de 1/240 s — assez précis pour
  que la divergence affichée vienne du chaos, pas de l'intégrateur.
- L'écart initial ne porte que sur l'angle du bras supérieur, réparti
  symétriquement autour de la position choisie : c'est le choix le plus
  simple et le plus courant pour illustrer la sensibilité.
- La divergence tracée est la distance maximale entre les bouts des
  pendules, en mètres ; l'historique complet est conservé par décimation
  progressive de l'échantillonnage.
- Rendu à la demande : la boucle `requestAnimationFrame` ne tourne que
  pendant la lecture ou un glisser ; en pause ou au repos, zéro redessin
  (vérifiable via le compteur `window.__epDrawCalls` en console).
- `prefers-reduced-motion` : pas de lecture automatique au chargement ;
  la simulation ne démarre que sur action explicite (bouton, espace).

## Licences des assets

Aucun asset tiers : tout le visuel est dessiné par le code, la police est
celle du système.

## Les règles à ne pas casser (vérifiées au build du portfolio)

- **Aucune requête vers un autre domaine** : pas de CDN, de police
  distante, d'API, de stats. Tout est embarqué dans le projet.
- Pas de cookies, pas de service worker, pas de formulaire qui envoie
  des données.
- `localStorage` uniquement pour une sauvegarde ou des préférences,
  clés préfixées par le slug (voir `src/main.js`).
- Chemins relatifs (`base: './'` dans `vite.config.js` — ne pas retirer).
- Garder le lien « Retour à la fiche du projet » (le slug est ajusté à
  l'intégration).

## Intégrer au portfolio

Projet terminé → ouvrir Claude Code dans le repo du portfolio et lancer
`/nouveau-projet <chemin de ce projet>`. La fiche est créée en
brouillon ; la publication reste une décision manuelle.

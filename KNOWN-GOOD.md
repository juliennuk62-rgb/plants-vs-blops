# 🛡 KNOWN-GOOD INVARIANTS — experience.html

Comportements qui DOIVENT rester intacts à chaque itération du polish.
Si une amélioration risque de casser un de ces invariants, elle est **refusée**.

L'agent peut **ajouter** des invariants ici quand il introduit un nouveau comportement critique.
L'agent ne peut **jamais retirer** un invariant existant sans validation explicite de l'utilisateur.

**Note** : `experience.html` a été réinitialisé à partir de la prod `index.html` (iter #3, 2026-04-25).
Les itérations #1 et #2 ne s'appliquent plus.

---

## Boot & init

- `experience.html` se charge sans erreur console
- Three.js r128 chargé via CDN
- Scene + Camera + Renderer instanciés (jeu visible)
- Banner "🧬 MODE EXPÉRIENCE" visible 8s au boot puis fade à 25% (hover pour réafficher)

## Persistance

- `localStorage` préfixé automatiquement par `exp_` (monkey-patch tout en haut du `<head>`)
- Saves isolés de la version prod ET de test.html (jamais de fuite)
- `window.__PVB_TEST_MODE__` et `window.__PVB_EXP_MODE__` tous deux à `true`

## HUD principal du jardin endless

- `#cash` affiche un nombre, mis à jour à chaque transaction
- `#kills` affiche un compteur de kills, incrémenté à chaque ennemi tué
- `#level` affiche le level joueur
- `#plant-count` / `#plant-cap` affichent N/M correctement
- `#xp-text` ou équivalent affiche l'XP
- `#cash-rate` affiche le revenu passif quand applicable

## Gameplay core (jardin endless)

- Le joueur peut placer une plante (clic sur cellule libre + plante sélectionnée)
- Les plantes attaquent automatiquement les ennemis dans leur lane
- Les ennemis spawnent et marchent vers le bord gauche (HQ)
- Tuer un ennemi → drop de cash visible (incrément `#cash`)
- Plant cap respecté (impossible de placer au-delà du cap)
- Le mode endless est jouable de bout en bout

## Boutons HUD top-right (état initial = prod, à respecter sauf cleanup explicite)

L'état des boutons reflète celui de la prod. Le user pourra les retirer
au cas par cas via demande explicite (ne pas anticiper de cleanup).

## Audio (interdit de toucher)

- Le code audio existant est immuable
- Les fonctions d'audio (oscillators, AudioContext) doivent rester intactes
- Aucune nouvelle fonction audio ne sera ajoutée par les itérations

## Tests cross-cutting

- Pas d'erreur console au load
- Cible 60 FPS sur PC modeste
- Aucun nom IP-protégé ajouté
- Aucun import de fichier externe (sauf CDN existants déjà présents en prod)

# 🛡 KNOWN-GOOD INVARIANTS — experience.html

Comportements qui DOIVENT rester intacts à chaque itération du polish.
Si une amélioration risque de casser un de ces invariants, elle est **refusée**.

L'agent peut **ajouter** des invariants ici quand il introduit un nouveau comportement critique.
L'agent ne peut **jamais retirer** un invariant existant sans validation explicite de l'utilisateur.

**État baseline confirmé fonctionnel** : iter #7 (commit `b55d2e2`, 20,915 lignes).

---

## 🚨 RÈGLE D'OR (apprise des iter #2/#5/#6)

**JAMAIS retirer du HTML un élément référencé par le JS via `getElementById`.**

Le code de test.html a beaucoup de `document.getElementById('xxx-btn').appendChild/style/...` SANS guard `if (el)`. Si l'élément manque, ça throw, et l'init Three.js peut casser silencieusement (canvas noir).

**Pattern correct pour cacher un bouton** :
- ✅ Ajouter `#xxx-btn { display: none !important; }` dans `<style id="exp-hud-hide">`
- ❌ Retirer le `<div id="xxx-btn">` du HTML

## Boot & init

- `experience.html` se charge sans erreur console
- Three.js r128 chargé via CDN
- Scene + Camera + Renderer instanciés (jeu visible)
- Banner "🧬 MODE EXPÉRIENCE" visible en haut au boot

## Persistance

- `localStorage` préfixé automatiquement par `exp_` (monkey-patch tout en haut du `<head>`)
- Saves isolés de la version prod ET de test.html (jamais de fuite)
- `window.__PVB_TEST_MODE__` et `window.__PVB_EXP_MODE__` tous deux à `true`

## HUD top-right — état confirmé fonctionnel

**Visibles** :
- 🛒 Boutique
- ⚗ Mixer
- 📚 Collection
- 🥚 Autel d'Ascension
- 🎖 Carrière
- ⚙ Settings (group 1)
- ⭐ Maîtrise
- 👤 Profil
- 🎯 Missions
- 🏆 Classement
- ✨ Prestige (visible quand débloqué)
- ☁ Compte
- 🛠 Editor
- ⚙ Settings (group 4)
- 🔊 Audio
- ? Help (group 4)
- ↺ Reset

**Cachés via CSS `display:none !important`** (HTML présent, JS happy) :
- 🧪 `#labo-btn`
- 🎯 `#daily-btn`
- 🌰 `#spore-btn`
- 📅 `#meta-btn`
- 🆔 `#profile-card-btn`
- 🏅 `#achievements-v2-btn`
- 🔔 `#notifs-btn`
- 🎨 `#skins-btn`
- 📽 `#timeline-btn`
- 🕊 `#memorial-btn`
- 🎬 `#rec-btn`
- ? `#top-right .hud-group:first-child #help-btn` (le help du group 4 reste visible)

## HUD principal du jardin endless

- `#cash` affiche un nombre, mis à jour à chaque transaction
- `#kills` affiche un compteur de kills
- `#level` affiche le level joueur
- `#plant-count` / `#plant-cap` affichent N/M
- `#xp-text` affiche l'XP
- `#essence-hud` affiche l'essence d'âme (🔮)
- Hero panel (Gardien 🧙) visible en haut-gauche

## Gameplay core (jardin endless)

- Le joueur peut placer une plante (clic sur cellule libre + plante sélectionnée)
- Les plantes attaquent automatiquement les ennemis dans leur lane
- Les ennemis spawnent et marchent vers le bord gauche (HQ)
- Tuer un ennemi → drop de cash visible
- Plant cap respecté
- Mode endless jouable
- Drop d'essence d'âme (🔮) sur kills VR+ (12% chance, scale par rareté)

## Features actives

- **Autel/Ascension** (🥚 #ascension-btn) : cocons + essence d'âme + codex
- **Maîtrise** (⭐ #mastery-btn) : niveaux par plante
- **Carrière** (🎖 #career-btn) : dashboard stats agrégées

## Système d'événements (PVB_EVENTS)

- Dropdown event dans panel admin
- Apply event → décor visible (Spring = pétales)
- localStorage `exp_pvb_active_event` persiste

## Labo 2 (préservé)

- LABO2_A, B, C openModal fonctionnels
- META layer (graines, atelier, achievements, daily, prestige, hub)
- Bouton 🧬 Labo 2 auto-injecté en mode test (en bas-droite avec ⚡ Admin et 🧪 Panneau Test)

## Panneau test + Admin (dev tools, à conserver)

- Bouton 🧪 Panneau Test visible en bas-droite
- Bouton ⚡ ADMIN à gauche du Panneau Test
- Modal admin contient le dropdown événements

## Audio (interdit de toucher)

- Le code audio existant est immuable
- Aucune nouvelle fonction audio ne sera ajoutée par les itérations

## Iter #8 — Lane Highlight (depuis 2026-04-24)

- Au hover d'une plante existante : un rectangle translucide au sol surligne sa lane (couleur = rareté de la plante)
- En mode placement (seed sélectionnée) : le hover sur une cellule libre surligne la lane cible (couleur = rareté de la seed)
- Mesh unique réutilisé : `_laneHoverMesh` (PlaneGeometry, MeshBasicMaterial transparent, renderOrder 2)
- Tous les hooks sont try/catch : aucun risque de bloquer le boot

## Tests cross-cutting

- Pas d'erreur console au load
- Cible 60 FPS sur PC modeste
- Aucun nom IP-protégé ajouté
- Aucun import de fichier externe (sauf CDN existants déjà présents en prod)

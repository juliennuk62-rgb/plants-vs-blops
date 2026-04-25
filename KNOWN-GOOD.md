# 🛡 KNOWN-GOOD INVARIANTS — experience.html

Comportements qui DOIVENT rester intacts à chaque itération du polish.
Si une amélioration risque de casser un de ces invariants, elle est **refusée**.

L'agent peut **ajouter** des invariants ici quand il introduit un nouveau comportement critique.
L'agent ne peut **jamais retirer** un invariant existant sans validation explicite de l'utilisateur.

**Note** : `experience.html` reconstruite depuis `test.html` (iter #4, 2026-04-25) pour
récupérer Autel/Maîtrise/Carrière fonctionnels. Lignes : 20,902.

---

## Boot & init

- `experience.html` se charge sans erreur console
- Three.js r128 chargé via CDN
- Scene + Camera + Renderer instanciés (jeu visible)
- Banner "🧬 MODE EXPÉRIENCE" visible en haut au boot

## Persistance

- `localStorage` préfixé automatiquement par `exp_` (monkey-patch tout en haut du `<head>`)
- `window.__PVB_TEST_MODE__` et `window.__PVB_EXP_MODE__` tous deux à `true`

## HUD principal du jardin endless

- `#cash` affiche un nombre, mis à jour à chaque transaction
- `#kills` affiche un compteur de kills
- `#level` affiche le level joueur
- `#plant-count` / `#plant-cap` affichent N/M
- `#xp-text` affiche l'XP
- `#essence-hud` affiche l'essence d'âme (🔮)
- Hero panel (Gardien 🧙) visible en haut-gauche

## Boutons HUD top-right (état confirmé par utilisateur — itération #4)

**Conservés** :
- 🛒 `#shop-btn` (Boutique)
- ⚗ `#mixer-btn` (Mixer)
- 📚 `#collection-btn` (Collection)
- 🥚 `#ascension-btn` (Autel)
- 🎖 `#career-btn` (Carrière)
- ⚙ `#settings-btn` (Paramètres)
- 🧪 `#labo-btn` (Labo)
- 🎯 `#daily-btn` (Défi du jour)
- 🌰 `#spore-btn` (Spore Bank)
- 📅 `#meta-btn` (Méta-quêtes)
- 🆔 `#profile-card-btn` (Carte profil)
- 🏅 `#achievements-v2-btn` (Badges)
- 🔔 `#notifs-btn` (Notifications)
- 🎨 `#skins-btn` (Skins)
- 📽 `#timeline-btn` (Timeline)
- ? `#help-btn` (Aide)
- ⭐ `#mastery-btn` (Maîtrise)
- 👤 `#profile-btn` (Profil)
- 🎯 `#missions-btn` (Missions)
- 🏆 `#leaderboard-btn` (Classement)
- 📷 `#camera-mode-btn` (Mode caméra)
- ✨ `#prestige-btn` (Prestige)
- ☁ `#account-btn` (Compte)
- 🛠 `#editor-btn` (Editor)
- 🔊 `#audio-btn` (Audio)
- ↺ `#reset-btn` (Reset)

**Supprimés (ne pas restaurer)** :
- 🕊 `#memorial-btn` (Mémorial) — retiré iter #4
- 🎬 `#rec-btn` (Clip) — retiré iter #4

## Gameplay core (jardin endless)

- Le joueur peut placer une plante (clic sur cellule libre + plante sélectionnée)
- Les plantes attaquent automatiquement les ennemis dans leur lane
- Les ennemis spawnent et marchent vers le bord gauche (HQ)
- Tuer un ennemi → drop de cash visible
- Plant cap respecté
- Mode endless jouable
- Drop d'essence d'âme (🔮) sur kills VR+ (12% chance, scale par rareté)

## Features ajoutées

- **Autel/Ascension** : `#ascension-btn` ouvre `#ascension-modal`, achat de cocons via essence d'âme, ouvert/pressé pour révéler une rareté supérieure. Codex via `#asc-codex-modal`.
- **Maîtrise** : `#mastery-btn` ouvre `#mastery-modal`, niveaux par plante, perks débloqués via kills, hook `_masteryKillHook(plantId, blopValue)` dans la kill chain.
- **Carrière** : `#career-btn` ouvre `#career-modal`, dashboard de stats agrégées et historique.

## Système d'événements (PVB_EVENTS)

- Bouton admin avec dropdown event fonctionnel
- Apply event → décor visible (Spring = pétales)
- localStorage `exp_pvb_active_event` persiste

## Labo 2 (préservé tel quel pour l'instant)

- LABO2_A (Boucherie 60s), LABO2_B (Roulette), LABO2_C (Chain Reaction)
- META layer (graines, atelier, achievements, daily seed, prestige, hub)
- Bouton 🧬 Labo 2 dans HUD (auto-injecté en mode test)

## Panneau test + Admin (dev tools, à conserver)

- Bouton 🧪 Panneau Test visible en bas-droite
- Bouton ⚡ ADMIN à gauche du Panneau Test
- Modal admin contient le dropdown événements

## Audio (interdit de toucher)

- Le code audio existant est immuable
- Aucune nouvelle fonction audio ne sera ajoutée par les itérations

## Tests cross-cutting

- Pas d'erreur console au load
- Cible 60 FPS sur PC modeste
- Aucun nom IP-protégé ajouté
- Aucun import de fichier externe (sauf CDN existants)

# 🛡 KNOWN-GOOD INVARIANTS — experience.html

Comportements qui DOIVENT rester intacts à chaque itération du polish.
Si une amélioration risque de casser un de ces invariants, elle est **refusée**.

L'agent peut **ajouter** des invariants ici quand il introduit un nouveau comportement critique.
L'agent ne peut **jamais retirer** un invariant existant sans validation explicite de l'utilisateur.

---

## Boot & init

- `experience.html` se charge sans erreur console
- Three.js r128 chargé via CDN
- Scene + Camera + Renderer instanciés (jeu visible)
- Banner "MODE EXPÉRIENCE" visible en haut au boot

## Persistance

- `localStorage` préfixé automatiquement par `exp_` (monkey-patch)
- Saves isolés de la version prod (jamais de fuite)

## HUD principal du jardin endless

- `#cash` affiche un nombre, mis à jour à chaque transaction
- `#kills` affiche un compteur de kills, incrémenté à chaque ennemi tué
- `#level` affiche le level joueur
- `#plant-count` / `#plant-cap` affichent N/M correctement
- `#xp-text` ou équivalent affiche l'XP

## Boutons HUD top-right (cleanup itération #2 — ne PAS rajouter d'autres boutons sans validation user)

- 🛒 `#shop-btn` (Boutique) — ouvre la boutique
- 📚 `#collection-btn` (Collection) — ouvre la collection
- 🎖 `#career-btn` (Carrière) — ouvre la carrière
- ⭐ `#mastery-btn` (Maîtrise) — ouvre la maîtrise
- ✨ `#prestige-btn` (Prestige) — visible quand prestige débloqué
- ⚙ `#settings-btn` (Paramètres) — ouvre les settings

**SUPPRIMÉS définitivement (ne pas restaurer)** : Mixer, Autel/Ascension, Labo, Défi du jour, Spore Bank, Méta-quêtes, Carte profil, Badges, Notifs, Skins, Timeline, Help, Profil, Missions, Classement, Mode caméra, Mémorial, Compte, Clip, Editor, Audio, Reset, Hero Panel (Gardien), Labo 2.

## Gameplay core (jardin endless)

- Le joueur peut placer une plante (clic sur cellule libre + plante sélectionnée)
- Les plantes attaquent automatiquement les ennemis dans leur lane
- Les ennemis spawnent et marchent vers le bord gauche (HQ)
- Tuer un ennemi → drop de cash visible
- Plant cap respecté (impossible de placer au-delà du cap)

## Polish iter #1 — Coin Fly-to-HUD

- Quand un Blop meurt avec `ud.value > 0`, un coin animé vole de sa position 3D vers `#cash`
- Sprite progressif : ✨ < 25, 🪙 < 100, 💰 ≥ 100
- Cap 14 coins simultanés (perf)
- `flashCash()` se déclenche à l'arrivée du coin

## Système d'événements (PVB_EVENTS)

- Bouton admin avec dropdown event fonctionnel
- Apply event → décor visible (Spring = pétales)
- localStorage `exp_pvb_active_event` persiste la sélection
- L'annonce Fortnite-style se déclenche au trigger admin

## Panneau test + Admin (dev tools, à conserver)

- Bouton 🧪 Panneau Test visible en bas-droite
- Bouton ⚡ ADMIN à gauche du Panneau Test
- Modal admin contient le dropdown événements

## Audio (interdit de toucher)

- Le code audio existant est immuable
- Les fonctions `playSound`, oscillators, AudioContext doivent rester intactes
- Aucune nouvelle fonction audio ne sera ajoutée par les itérations

## Tests cross-cutting

- Pas d'erreur console au load
- Cible 60 FPS sur PC modeste
- Aucun nom IP-protégé ajouté
- Aucun import de fichier externe (sauf CDN existants)

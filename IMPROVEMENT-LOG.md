# 📈 IMPROVEMENT LOG — experience.html

Chaque exécution du prompt d'itération append une entrée ici.
Format : itération #N · timestamp · persona · choix · niveau de risque · résumé.

L'agent doit **lire ce fichier au début** de chaque cycle pour :
- Ne pas refaire deux fois la même amélioration
- Voir la progression
- Adapter ses choix selon ce qui a déjà été fait

---

## Itération 0 — Setup (2026-04-25)

- **Persona** : N/A (setup initial, pas une itération de polish)
- **Action** : Création de `experience.html` (copie de `test.html`)
  - Préfixe localStorage `dev_` → `exp_`
  - Banner "MODE TEST DEV" → "MODE EXPÉRIENCE"
  - Flag `__PVB_EXP_MODE__` ajouté en plus de `__PVB_TEST_MODE__`
  - Title `EXPÉRIENCE (polish itératif)`
- **Niveau** : N/A
- **Lignes** : 20,903 (identique à test.html à 4 lignes près)
- **Deploy** : commit setup initial sur `main`
- **Live URL** : https://juliennuk62-rgb.github.io/plants-vs-blops/experience.html

---

## Itération #1 — 2026-04-25

- **Persona** : Léa, 16 ans, créatrice TikTok débutante (3K followers). Veut screenshote/screen-record tout ce qui claque. Joue 5 min, doit en sortir un clip exploitable.
- **Critiques** :
  1. Aucune connexion visuelle entre la position du Blop tué et le compteur cash en HUD — le pulse de l'addition est plat
  2. Damage numbers existent mais petits et discrets
  3. Pas de feedback de streak / combo quand on enchaîne les kills
  4. Level-ups arrivent sans événement visuel marquant
  5. Kill GOAT fait un slow-mo cool mais pas de récap visuel pour screenshot
- **Choix** : **Coin Fly-to-HUD** — quand un Blop meurt et drop du cash, animation d'un coin (✨ / 🪙 / 💰 selon montant) qui vole depuis la position 3D de la mort jusqu'au compteur cash dans le HUD. Connecte spatialement le kill au reward.
- **Niveau** : 🟢 Safe (purement cosmétique, ne touche aucune mécanique core)
- **Diff** :
  - +60 lignes : fonction `spawnCoinFly(worldPos, amount)` + helper `_projectToScreen` + cap interne `_coinFlyerActive` (max 14 simultanés pour préserver FPS)
  - +1 ligne : déclenchement dans `killBlop` juste après `addCash(ud.value)` (try/catch défensif)
  - Animation : pop-in scale 0.6→1.15 → vol cubic-bezier 750ms vers HUD → arrivée scale 0.5 + flashCash
  - Sprite progressif selon montant : ✨ < 25, 🪙 < 100, 💰 ≥ 100
- **Lignes** : 20,903 → 20,967 (+64)
- **Commit** : `14240ec`

---

## Itération #2 — 2026-04-25 (cleanup)

- **Persona** : N/A (cleanup demandé par l'utilisateur, pas une itération de polish)
- **Action** : Réduction du scope visuel à **6 boutons HUD** + admin/panneau test
- **Niveau** : 🔴 Bold (suppression massive)
- **Diff** :
  - HTML : retrait de 22 boutons HUD non listés (Mixer, Autel, Labo, Défi du jour, Spore Bank, Méta-quêtes, Carte profil, Badges, Notifs, Skins, Timeline, Help, Profil, Missions, Classement, Mode caméra, Mémorial, Compte, Clip, Editor, Audio button, Reset)
  - HTML : retrait du `#hero-panel` (Gardien)
  - JS : retrait des 5108 lignes des modules LABO2_A, LABO2_B, LABO2_C, LABO2_META (lignes 15601-20708)
  - JS : retrait du setTimeout d'install Labo 2 (HUD button + retrofit + meta hooks)
- **Conservés** : 🛒 Boutique, 📚 Collection, 🎖 Carrière, ⭐ Maîtrise, ✨ Prestige, ⚙ Paramètres + ⚡ ADMIN + 🧪 Panneau Test + Polish iter #1 coin fly + PVB_EVENTS + Polish iter #1 coin fly-to-HUD
- **Lignes** : 20,967 → 15,791 (−5,176 / −24.7%)
- **Commit** : `f7de3bc`

---

## Itération #3 — 2026-04-25 (RESET depuis prod)

- **Persona** : N/A (reset demandé par l'utilisateur, jeu non-fonctionnel après iter #2)
- **Action** : Recréation de `experience.html` à partir de la **prod** (`index.html`, 8,782 lignes au lieu du test.html dérivé qui était à 15,791 et cassé).
- **Niveau** : 🔴 Bold (rebuild complet from scratch)
- **Diff** :
  - `cp index.html → experience.html`
  - **+18 lignes** : monkey-patch localStorage prefix `exp_` au top du `<head>`
  - **+13 lignes** : banner "🧬 MODE EXPÉRIENCE" auto-injecté au boot du body, fade à 25% après 8s, hover pour réafficher
  - Title : "Plants vs Blops - v6 DUAL GARDEN (local test)" → "Plants vs Blops — EXPÉRIENCE (polish itératif)"
- **Itérations précédentes (#1, #2) ABANDONNÉES** car appliquées sur la mauvaise base. Le polish reprendra fresh à partir de cette base prod.
- **Lignes** : 8,782 → 8,817 (+35)
- **Commit** : `9ed3a69`

---

## Itération #4 — 2026-04-25 (rebuild depuis test.html + features Autel/Maîtrise/Carrière)

- **Persona** : N/A (demande utilisateur : ajouter 3 features + retirer 2 boutons)
- **Action** : Rebuild de `experience.html` depuis `test.html` (1.17 MB, full features) au lieu de `index.html`. Test.html contient déjà Autel, Maîtrise, Carrière fonctionnels — extraction surgicale aurait demandé ~2000 lignes de copy-paste. Approche pragmatique : prendre le superset et retirer les 2 boutons indésirables.
- **Niveau** : 🔴 Bold (rebuild complet from scratch)
- **Diff** :
  - `cp test.html → experience.html` (20,903 lignes)
  - **Préfixe localStorage** : `dev_` → `exp_` (au top du `<head>`)
  - **Flag** : `__PVB_EXP_MODE__ = true` ajouté
  - **Title** : "TEST DEV (feedback dashboard)" → "EXPÉRIENCE (polish itératif)"
  - **Banner** : "🧪 MODE TEST DEV" → "🧬 MODE EXPÉRIENCE"
  - **Retiré** : `#memorial-btn` (🕊 Mémorial) + `#rec-btn` (🎬 Clip)
- **Features incluses** : Boutique, Mixer, Collection, **Autel/Ascension** (🥚), Carrière, Settings, Labo, Défi du jour, Spore Bank, Méta-quêtes, Carte profil, Badges, Notifs, Skins, Timeline, Help, **Maîtrise** (⭐), Profil, Missions, Classement, Camera, Prestige, Compte, Editor, Settings, Audio, Help, Reset, Hero panel (Gardien), Labo 2 (Features A/B/C), Events system (Spring), Panneau test, Admin
- **Lignes** : 8,817 → 20,902 (+12,085 / 137% increase)
- **Commit** : `b32d47c`

---

## Itération #5 — 2026-04-25 (cleanup HUD : 9 boutons retirés)

- **Persona** : N/A (demande utilisateur via screenshot annoté avec X)
- **Action** : Retrait surgical de 9 boutons du HUD top-right (group 1) marqués X dans le screenshot
- **Niveau** : 🟢 Safe (HTML uniquement, JS code intact, modals dead mais inert)
- **Diff** :
  - Retiré : `#labo-btn` (🧪 Labo)
  - Retiré : `#daily-btn` (🎯 Défi du jour)
  - Retiré : `#spore-btn` (🌰 Spore Bank — était caché, retiré pour cohérence)
  - Retiré : `#meta-btn` (📅 Méta-quêtes)
  - Retiré : `#profile-card-btn` (🆔 Carte profil)
  - Retiré : `#achievements-v2-btn` (🏅 Badges)
  - Retiré : `#notifs-btn` (🔔 Notifs)
  - Retiré : `#skins-btn` (🎨 Skins)
  - Retiré : `#timeline-btn` (📽 Timeline)
  - Retiré : `#help-btn` (group 1) — le second `#help-btn` du group 4 reste (duplicate ID dans test.html)
- **Conservés group 1** : Boutique, Mixer, Collection, Autel, Carrière, Settings (⚙), Maîtrise
- **Lignes** : 20,902 → 20,891 (−11)
- **Commit** : `3748a4c`

---

## Itération #6 — 2026-04-25 (HOTFIX iter #5 : jeu cassé, restaure boutons en display:none)

- **Persona** : N/A (hotfix)
- **Action** : Le retrait HTML des 9 boutons en iter #5 a cassé le rendu Three.js (canvas noir). Le JS référencait certains boutons (`#notifs-btn`, `#daily-btn`, etc.) sans guard `if (el)`, ce qui throw au boot et empêche l'init du game loop.
- **Niveau** : 🟢 Safe (restaure structure DOM tout en gardant le visual cleanup)
- **Diff** :
  - Restaure les 9 boutons HTML
  - Chaque bouton reçoit `style="display:none;..."` (visuellement invisible mais présent en DOM)
  - Le JS trouve les éléments via `getElementById`, no throw, init Three.js OK
- **Visuel** : identique à iter #5 (les 9 boutons restent invisibles)
- **Lignes** : 20,891 → 20,903 (+12)
- **Commit** : à venir au push
- **Leçon** : éviter les retraits HTML purs sur test.html — préférer `display:none` quand le code JS référence l'élément.

---

## ⚠️ Note pour les futures itérations

Pour retirer un bouton du HUD :
- ✅ Préférer `style="display:none"` (DOM préservé, JS happy)
- ❌ Éviter le retrait HTML pur (risque de breakage init si le JS référence l'élément)

---
- **Note** : le code JS qui hooke ces boutons reste en place (btnClick check si élément existe, donc no-op silencieux). Les modals associés restent dans le HTML mais ne sont plus accessibles via l'UI (toujours accessibles programmatiquement via `openModal()`).

---
- **Note** : si l'utilisateur veut retirer d'autres features, on le fera incrémentalement par demande explicite (et avec test post-removal).

---

---

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
- **Commit** : à venir au push

---

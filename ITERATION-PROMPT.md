# 🧬 Prompt d'itération — Polish jardin endless (experience.html)

Ce fichier contient le prompt à coller dans la conversation à chaque fois
que tu veux lancer un nouveau cycle de polish chirurgical sur `experience.html`.

**Usage** : copie tout le bloc entre les `===` ci-dessous et colle dans le chat.
À chaque exécution, je :
1. Lis la mémoire (IMPROVEMENT-LOG.md, KNOWN-GOOD.md)
2. Génère un persona joueur fictif différent
3. Critique le jeu de son point de vue (3-5 reproches)
4. Applique UNE seule amélioration chirurgicale
5. Vérifie syntaxe, met à jour les logs, deploy auto
6. Te rapporte le résultat

---

=== DÉBUT DU PROMPT — copie tout ce qui suit jusqu'à la fin ===

```
# 🧬 ITÉRATION POLISH — JARDIN ENDLESS

Tu vas réaliser UN cycle d'amélioration chirurgicale du jeu de jardin endless dans `experience.html`.
Chaque exécution = UNE seule amélioration ciblée et soignée.

# CIBLE STRICTE

- Fichier : `C:/Fortnite test/experience.html`
- Zone autorisée : LE JARDIN ENDLESS uniquement
  - Code de la state principale (window.state, plants[], enemies[], waves)
  - HUD principal (#cash, #kills, #level, #plant-count, #plant-cap, #xp-text, etc.)
  - Three.js scene du jardin (lights, materials, particles, camera)
  - DOM overlays liés au jeu principal
- Tu peux ENRICHIR l'intégration de PVB_EVENTS dans le jardin (ex: créatures Spring-themed, boost cash pendant Printemps, particules thématiques, etc.)
- Zones INTERDITES :
  - LABO2_A / LABO2_B / LABO2_C / LABO2_META (touche pas, c'est un autre univers)
  - Mode Campaign / Challenge si présents
  - Code AUDIO (existant inchangé, zéro nouvelle fonction audio)

# DIMENSIONS AUTORISÉES (pioche celle qui sert ton persona)

- 🎨 **Graphisme** : Three.js scene, materials, lights, géométries, particles, post-process léger
- 📊 **UI / HUD** : DOM overlays, stats, modals, layout, microcopy, espacement
- ⚙️ **Gameplay** : mechanics, balance, courbes de difficulté, timing
- 👹 **Monstres** : nouveaux types, comportements, élites, mini-bosses
- 🤝 **Alliés** : nouveaux plant types, synergies, companions, buffs
- 📈 **Stats** : dashboards, graphs, breakdowns, run summary
- ✨ **Dopamine** :
  - Récompenses VISIBLES : level-ups bouncy, coin animations volantes, milestone pop-ups, unlock reveals, badge drops
  - Juice : particles plus denses, screen shake calibré, hitstop sur big kills, number popups bigger, color flashes
- 🖱️ **Contenu clic fonctionnel** :
  - Augmenter les clics IMPACTANTS, réduire les clics morts
  - Interactions secondaires (clic-droit, hold, double-clic, hover-action)
  - Affordances claires : tout ce qui est cliquable doit le montrer

# DIMENSION INTERDITE

🔇 **Audio** — pas d'ajout, pas de modification, pas de retrait.

# PROTOCOLE D'ITÉRATION (ordre strict)

## 1. Mémoire
- Lis `C:/Fortnite test/IMPROVEMENT-LOG.md` (toutes les itérations passées)
- Lis `C:/Fortnite test/KNOWN-GOOD.md` (invariants à respecter)
- Scan rapide de `experience.html` pour comprendre l'état du jardin endless

## 2. Persona du jour (DIFFÉRENT à chaque itération)

Génère UN persona joueur fictif avec :
- Âge / contexte / mood
- 1-2 phrases qui décrivent ce qu'il cherche

Exemples (ne reprends PAS le même que l'itération précédente) :
- Streameuse 28 ans, 50K viewers, joue 30s entre deux pubs
- Kid 11 ans hyperactif, veut du gros nombre tout de suite
- Père 42 ans, 5 min avant que les enfants se réveillent
- Retraitée 65 ans, première fois sur PC, cherche du calme
- Dev front-end 33 ans qui critique le UI à voix haute
- Speedrunner perfectionniste qui voit chaque % manquant
- Reddit-commenter qui cherche systématiquement des reproches
- Joueur de roguelike pro qui veut de la profondeur de build
- Amateur de PvZ original qui veut voir si "ça respire pareil"
- Ado TikTok qui screenshot pour partager
- Gamer compétitif qui veut un leaderboard et des metrics
- Joueur casual stressé qui veut du feedback rassurant
- Designer UX qui zoom sur la friction
- Ami sceptique qu'on a forcé à essayer
- ... ou ton invention (autorisé)

## 3. Critique (3-5 reproches du persona)

Du POV de ce persona, liste **3 à 5 reproches concrets** au jardin endless actuel.
Sois précis : "à la wave 3 j'ai pas compris pourquoi mes graines ont pas tué", pas "c'est confus".

## 4. Sélection de l'amélioration

Pick UNE seule amélioration parmi tes critiques — la plus impactante POUR CE PERSONA.
Justifie en 1 phrase pourquoi c'est elle.

## 5. Niveau de risque

Annonce le niveau :
- 🟢 **Safe** : juice / polish / particules / popups / espacement / microcopy / visual tweaks
- 🟡 **Medium** : nouveau type d'enemy ou plant, rebalance, nouvelle interaction clic
- 🔴 **Bold** : nouvelle mécanique, restructuration UI/UX, système entier

## 6. Application

- Édite `experience.html` via Edit tool
- Respecte les invariants `KNOWN-GOOD.md` (bloque toute modif qui les casse)
- **RÈGLE D'OR** : ne JAMAIS retirer du HTML un élément référencé par le JS via `getElementById`. Pour cacher un bouton du HUD : utiliser `display:none` dans le bloc CSS `<style id="exp-hud-hide">`. Le HTML reste intact.
- Code propre, commenté, dans le style du fichier existant

## 7. Vérification syntaxe

```
node -e "const fs=require('fs');const html=fs.readFileSync('C:/Fortnite test/experience.html','utf8');const re=/<script(?:\s[^>]*)?>([\s\S]*?)<\/script>/gi;let m,i=0,ok=true,errs=[];while((m=re.exec(html))){i++;try{new Function(m[1]);}catch(e){ok=false;errs.push('#'+i+': '+e.message.split('\n')[0]);}}console.log('Scripts:',i,'OK:',ok);if(!ok)console.log(errs.join('\n'));"
```

Si `OK: false` → revert et choisis une autre amélioration. Re-vérifie.

## 8. Update mémoire

Append une entrée dans `IMPROVEMENT-LOG.md` :

```
## Itération #N — <date ISO>

- **Persona** : <description courte>
- **Critiques** :
  1. <critique 1>
  2. <critique 2>
  3. <critique 3>
- **Choix** : <ce qu'on a fait et pourquoi>
- **Niveau** : 🟢 Safe / 🟡 Medium / 🔴 Bold
- **Diff** : <résumé en 2-3 lignes des modifications>
- **Lignes** : <ancien total → nouveau total> (+/- N)
- **Commit** : <hash>
```

Si tu as introduit un nouveau comportement critique, append-le aussi dans `KNOWN-GOOD.md`.

## 9. Auto-deploy

```
cp "C:/Fortnite test/experience.html" "/tmp/plants-vs-blops/experience.html"
cd /tmp/plants-vs-blops
git add experience.html IMPROVEMENT-LOG.md KNOWN-GOOD.md
git commit -m "polish(exp): <description courte> [iter #N]"
git push origin main
```

## 10. Rapport à l'utilisateur

Réponds avec :
- 🎭 **Persona du jour** (1-2 lignes)
- 📋 **Critiques** (3-5 bullets)
- ✨ **Amélioration appliquée** (titre + courte description + niveau de risque)
- 🔧 **Diff résumé** (quoi a changé techniquement)
- ✅ **Syntax + deploy** (statut)
- 🌐 **URL live** : https://juliennuk62-rgb.github.io/plants-vs-blops/experience.html
- 💡 **Idée pour la prochaine itération** (1 ligne, direction suggérée)

# RÈGLES DURES

- **UNE** seule amélioration par exécution (chirurgical, pas batch)
- **JAMAIS** toucher à l'audio (zéro modif sur les fonctions audio existantes)
- **JAMAIS** modifier LABO2_* ou les modes hors jardin endless
- **JAMAIS** casser un invariant `KNOWN-GOOD.md`
- **JAMAIS** refaire la même amélioration que les 3 dernières itérations (lis le log)
- **JAMAIS** retirer un élément HTML référencé par le JS via getElementById (utiliser CSS display:none à la place)
- **TOUJOURS** trouver quelque chose à améliorer (pas de "rien à faire")
- **TOUJOURS** rotation du persona (pas le même que la fois d'avant)

# C'EST PARTI

Lis les fichiers de mémoire. Génère ton persona. Critique. Choisis. Applique. Vérifie. Logue. Deploy. Rapporte.
Une seule itération. Soigne-la.
```

=== FIN DU PROMPT ===

---

## 🔧 Notes d'usage

- **Recolle le bloc en entier** (entre les `=== DÉBUT ===` et `=== FIN ===`, sans inclure ces marqueurs eux-mêmes)
- **À chaque itération** je génère un persona DIFFÉRENT pour diversifier les améliorations
- **Le compteur d'itération** s'auto-incrémente (lit le dernier #N dans IMPROVEMENT-LOG.md)
- **Auto-deploy activé** : tu peux refresh la page test après chaque itération sans rien faire d'autre
- **Si quelque chose casse** : dis-moi et je hotfix (jamais bloquant pour la suite)

## 🛡 Garde-fous actifs

Trois fichiers persistent et garantissent la cohérence :

1. **`C:/Fortnite test/IMPROVEMENT-LOG.md`** — historique complet des itérations (persona, critique, choix, lignes, commit)
2. **`C:/Fortnite test/KNOWN-GOOD.md`** — invariants à protéger (boutons visibles, gameplay core, règle d'or anti-breakage)
3. **`C:/Fortnite test/ITERATION-PROMPT.md`** — ce fichier (sauvegarde du prompt)

Tous les trois sont versionnés sur GitHub avec chaque itération.

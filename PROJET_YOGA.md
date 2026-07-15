# Yoga 2.0 — PWA de pratique du yoga

Application web installable (PWA), fichier unique HTML, 100 % hors-ligne. Ashtanga complet, styles multiples, générateur intelligent, guidage vocal. Aucune dépendance externe hors polices Google (mises en cache).

**Version actuelle : v2.7.0** · thème clair uniquement · icône line-art Bakasana.

---

## Déploiement

Drag-and-drop des 7 fichiers à plat, à la racine du repo (GitHub Pages ou Infomaniak, HTTPS requis pour le service worker) :

```
index.html
manifest.json
sw.js
icon-192.png
icon-512.png
icon-maskable-512.png
apple-touch-icon.png
```

Installation : sur Android Chrome → menu ⋮ → « Ajouter à l'écran d'accueil ».
**À chaque changement d'icône : désinstaller puis réinstaller la PWA** (Android cache l'ancienne).

Le localStorage conserve réglages, historique, séquences enregistrées et niveau. Perdu si les données du site sont effacées.

---

## Fonctionnalités

### Onglet Séries (styles)
Puces de style en haut : **Ashtanga · Hatha · Yin · Vinyasa · Sport**. Choix mémorisé.

- **Ashtanga** — 4 séries traditionnelles :
  - Série 1 (Yoga Chikitsa), Série 2 (Nadi Shodhana), Série 3 & 4 (Sthira Bhaga A/B)
  - Sections dépliables, « Séance complète » par série (inclut salutations + debout + finale)
  - Note d'honnêteté : ordre variable selon lignées ; séries 5-6 non incluses (sources divergentes)
- **Hatha** (3 séances), **Yin** (3, tenues longues 2-5 min), **Vinyasa** (3 flows)
- **Sport** (9 disciplines) : Surf, Escalade, Ski de rando, Trail, Alpinisme, Course à pied, Natation, Apnée, Vélo

**Ajustement du temps universel** : sélecteur Native · 10′ · 15′ · 20′ · 30′ · 45′ · 1 h sur tous les styles ET l'Ashtanga. Le Yin démarre à 20 min (tenues longues préservées). L'algorithme étire/comprime les tenues et ajoute/retire des postures en gardant le savasana en clôture.

**Aperçu ▾ Voir la séance** : déplie la composition complète (pictos + durées) sans démarrer, sur toutes les cartes y compris les séries Ashtanga.

### Onglet Routines
- Carte intro : philosophie d'équilibrage avant/arrière, souffle par les postures (pas de pranayama — géré dans l'appli OA)
- Générateur « Séance sur mesure » : temps × objectif (Mobilité & respiration, Souplesse jambes & hanches, Dynamique, Relaxation, Récup sport, Dos & posture) → séquence équilibrée composée automatiquement, contre-postures injectées
- **Suggestion intelligente** : analyse l'historique de 7 jours et conseille un objectif (dominante avant → mobilité, etc.)
- **Ma semaine** : bilan hebdo, barre d'équilibre avant/arrière, 3 dernières séances
- 3 routines équilibrées prédéfinies (Souffle du matin, Avant·Arrière, Grande respiration)

### Onglet Séquenceur
- Bibliothèque de **176 postures** filtrable, ajout via « + »
- Réglage des souffles par posture, réordonnancement, enregistrement de séquences nommées
- **Partage par lien** (`?s=...`) : encode la séquence dans l'URL, partage natif Android ; à la réception, carte d'import (Lancer / Charger / Ignorer)
- Indicateur de composition ⬇⬆↺🙃 en temps réel

### Lecteur (séance guidée)
- Cercle de respiration à deux vitesses (inspir/expir réglables séparément)
- Décompte des souffles, gong doux entre postures (Web Audio)
- **Guidage vocal français** (Web Speech) : nom, côté, durée, consignes d'alignement, « dernier souffle », « Namasté » final
- **Pictogrammes SVG** filaires par posture (34 archétypes, générés en JS)
- **Consignes d'alignement** affichées + annoncées (95+ postures)
- **Niveaux de pratique** : Débutant · Facile · Standard · Avancé, changeables en direct via pastille dans le lecteur (79+ variantes)
- Écran maintenu allumé (Wake Lock), pause/skip/précédent

### Réglages
Inspiration (1-6 s) / Expiration (1-10 s) séparées · gong on/off · guidage vocal on/off · niveau de pratique · effacer l'historique. Thème clair uniquement.

---

## Design system

- Polices : Bricolage Grotesque (titres) + Spline Sans + Spline Sans Mono
- Thème clair : fond crème `#f4f7f6`, encre `#0c313d`, accent teal `#0f8a7d`, doré `#b9761a`
- Tout piloté par variables CSS (`--deep`, `--aqua`, `--air`, `--ink`, `--dim`, `--line`…)
- Hero avec vague animée, cercle de respiration, cartes arrondies

---

## Workflow technique

- **Fichier unique** `index.html` : CSS inline dans `<style>`, JS inline dans `<script>`
- Patchs via scripts Python (heredoc), une modification validée à la fois
- Validation : `html5lib` (structure) + `node --check` (JS) avant chaque livraison
- Tests JS en Node avec DOM factice (scaling, catégorisation, encode/décode partage, couverture pictos/consignes/variantes)
- **Service worker** : cache versionné `yoga-vX.Y.Z`, `Promise.allSettled` au précache, cache-first + runtime cache (offline complet), `ignoreSearch` pour les liens partagés
- **Bump obligatoire** de `VERSION` dans `sw.js` à chaque modif (sinon pas de mise à jour côté client)
- Version affichée en haut à droite du hero (doit matcher le SW)

---

## Catégorisation des postures (catOf)

⬇ flexion avant · ⬆ extension/arrière · ↺ torsion · 🙃 inversion. Sert au bilan d'équilibre, au générateur et aux suggestions. Attention aux pièges déjà corrigés : Janu Sirsasana = avant (pas inversion), Eka Pada Sirsasana = avant, Mukta/Baddha Hasta Sirsasana = inversion.

---

## Historique des versions

- v1.0 — base : série 1, routines, séquenceur, lecteur
- v1.1 — routines rééquilibrées, sans pranayama
- v1.2 — générateur intelligent + objectifs
- v1.3 — historique de pratique + suggestion
- v1.4 — guidage vocal
- v1.5 — version affichée, rythme inspir/expir, consignes d'alignement
- v1.6 — pictogrammes SVG
- v1.7 — partage de séquence par lien
- v1.8 — série 2 + niveaux de pratique
- v1.9 — séries 3-4, menus Hatha/Yin/Vinyasa/Sport
- v2.0 — sports Natation/Apnée/Vélo + ajustement du temps (sport)
- v2.1 — aperçu dépliable des séances
- v2.2 → v2.3 — thèmes (puis réduits à sombre/clair)
- v2.4 → v2.6 — renommage Yoga 2.0, itérations d'icône
- **v2.7 — ajustement du temps universel (+ Ashtanga), aperçu des séries Ashtanga, mode sombre retiré, icône line-art**

---

## Règles de contenu (importantes)

- **Aucun exercice de pranayama** dans l'appli : le travail respiratoire dédié se fait à part (Oxygen Advantage). Le souffle se développe *par les postures* (torsions, extensions, inversions).
- Kapalabhati et Wim Hof explicitement exclus (paradigme incompatible).
- L'illustration d'icône est générée par IA : le texte devanagari en haut n'est pas un mot correct (décoratif, illisible en taille icône).

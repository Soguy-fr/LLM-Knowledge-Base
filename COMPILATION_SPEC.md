# COMPILATION SPEC — KB Shauri IA & Transformation du Travail

## Vue d'ensemble
Ce document définit **comment compiler la wiki**. À chaque mise à jour, le compilateur (Claude Code) doit suivre **exactement ces règles**. C'est le contrat entre toi et l'IA.

---

## 1. STRUCTURE DES RÉPERTOIRES

```
LLM_KB/
├── Clippings/                     ← Entrée Web Clipper (traité à chaque compilation)
│   ├── article-x-2026.md         ← Créé par Obsidian Web Clipper
│   ├── article-y-2026.md
│   └── document-manuel-upload.pdf ← Fichiers ajoutés manuellement
│
├── raw/                          ← Articles bruts nettoyés (après traitement)
│   ├── article-spring-ragain-2026.md
│   ├── article-leparmentier-2026.md
│   ├── article-glavas-2026.md
│   └── document-manuel-upload.md  ← PDF converti en MD ou résumé
│
├── wiki/                         ← Wiki compilée (générée par l'IA)
│   ├── index.md                  ← Accueil (concepts + articles sources)
│   ├── concepts/                 ← Pages concepts (idées synthétiques)
│   │   ├── transformation-vs-destruction.md
│   │   ├── metacompetences.md
│   │   └── inegalites-acces.md
│   │
│   ├── articles/                 ← Articles compilés (résumés + citations)
│   │   ├── article-spring-ragain-2026.md
│   │   ├── article-leparmentier-2026.md
│   │   └── article-glavas-2026.md
│   │
│   └── concepts-registry.md      ← NOUVEAU : inventaire des concepts existants
│
├── COMPILATION_SPEC.md           ← Ce fichier (tu le lis en premier)
└── .gitignore
   Clippings/                       ← Fichiers temporaires (après traitement → raw/)
```

---

## 1b. WORKFLOW D'INGESTION DES FICHIERS

### Web Clipper (Obsidian)
```
1. Tu lis article → clique "Save to Obsidian" (Web Clipper)
2. Fichier créé dans Clippings/article-title-2026.md
3. À chaque compilation, Claude Code :
   a. Lit Clippings/
   b. Nettoie + formate les fichiers (images locales, etc.)
   c. Déplace en raw/
   d. Vide Clippings/ (prêt pour la prochaine)
```

### Upload manuel (PDF, doc, etc.)
```
1. Tu télécharges un fichier → mets-le dans Clippings/
   Exemple : Clippings/rapport-unicef-2026.pdf
   
2. À la compilation, Claude Code :
   a. Détecte fichiers non-MD (PDF, DOCX, etc.)
   b. Convertit/extrait le texte (si PDF → txt/md)
   c. Crée raw/rapport-unicef-2026.md (ou garde PDF si pertinent)
   d. Signale à l'utilisateur : "PDF de 45 pages. Dois-je extraire le texte complètement ou créer un résumé ?"
```

### Règles
- **Clippings/** = zone temporaire (entrée)
- **raw/** = source archivée (définitive, jamais modifiée après)
- **Conversion** : PDF → extrait texte en MD si trop gros (>20 pages). Sinon garde PDF + crée résumé MD
- **Nettoyage** : Web Clipper souvent ajoute du bruit (pubs, commentaires). Claude Code nettoie avant raw/

---

```
FLUX HUMAIN (navigation) :
Concept → Article compilé → Source brute
   ↓             ↓              ↓
(idée)     (détails)      (original)

FLUX IA (interrogation) :
Lis SEULEMENT les concepts/
(pas articles ni raw/)
```

**Règle d'or** : Un lien = un niveau de profondeur. Pas de sauts.

---

## 2b. GESTION DES CONCEPTS : Éviter les doublons

### Le problème
Chaque nouvel article peut sembler créer de nouveaux concepts. 
Résultat : 100 articles = 150+ concepts redondants = wiki unusable.

### La solution : Concepts Registry

Claude Code maintient **wiki/concepts-registry.md** :

```markdown
# Concepts Registry

## Format
Chaque concept existant est listé avec :
- Nom (clé unique)
- Définition courte (1 ligne)
- Slug (chemin du fichier)
- Concepts liés (si any)
- Mots-clés d'équivalence

## Registry

| Concept | Slug | Clés équivalentes | Type |
|---------|------|------------------|------|
| Transformation vs Destruction | transformation-vs-destruction | emploi, automation, jobs, tâches | parent |
| Métacompétences | metacompetences | supervision IA, culture générale | parent |
| Inégalités et Token Economy | inegalites-acces | token economy, divide, classe bipolaire | enfant |
| Impact recherche | impact-recherche | science, hallucinations, opacité | standalone |

```

### Processus de compilation (CRITIQUE)

**AVANT de créer un nouveau concept** :
```
1. Claude Code lit concepts-registry.md
2. Analyse nouvel article pour concepts clés
3. Pour CHAQUE concept potentiel :
   a. Cherche dans registry avec fuzzy matching
   b. Si concept existant → RÉUTILISE [[concept]]
   c. Si concept "proche" (>80% overlap) → enrichit existant
   d. Si concept VRAIMENT nouveau → crée + ajoute au registry
4. Documente sa décision : "Réutilisé 3 concepts, créé 1 nouveau"
```

### Exemple concret

Tu ajoutes article sur "Compétences au 21e siècle"

```
Claude Code analyse :
"Article parle de : superviser l'IA, culture générale, formation..."

Cherche dans registry :
- "superviser l'IA" → match [[Métacompétences]] (90%)
- "formation" → match existant
- "transformations emploi" → match [[Transformation vs Destruction]] (85%)

Décision : Enrichir [[Métacompétences]], ajouter lien vers 
[[Transformation vs Destruction]]. 

Pas de nouveaux concepts = wiki reste lean.
```

---

## 2c. CONCEPTS LIÉS ET HIÉRARCHIE

### Types de structure

**Type 1 : Standalone**
```
[[Concept A]]
(idée indépendante)
```

**Type 2 : Horizontally linked** (même niveau)
```
[[Concept A]] ↔ [[Concept B]]

Exemple :
[[Transformation vs Destruction]] ↔ [[Métacompétences]]
```

**Type 3 : Hiérarchie parent/enfants**
```
[[Inégalités technologiques]]  ← PARENT
├─ [[Inégalités et Token Economy]]  ← ENFANT
├─ [[Accès IA par région]]  ← ENFANT
└─ [[Formation et divide]]  ← ENFANT
```

### Quand créer une hiérarchie ?

✅ **Si** :
- >3 concepts étroitement liés au même sujet
- Enfants ne survivent pas seuls (trop spécifiques)
- Parent = idée générale, enfants = manifestations

❌ **Si non** :
- Juste 2 concepts liés → liens horizontaux suffisent
- Enfants sont autonomes → concepts indépendants

### Format Markdown

**Parent concept**
```markdown
# Inégalités technologiques

Définition large des disparités d'accès IA...

## Manifestations
* [[Inégalités et Token Economy]] — l'angle économique
* [[Accès IA par région]] — l'angle géographique  
* [[Formation et divide]] — l'angle éducatif

## Concepts connexes (horizontal)
* [[Transformation vs Destruction]] — cause commune

#inegalites #technologie
```

**Enfant concept**
```markdown
# Inégalités et Token Economy

Les tokens deviennent métrique de valeur...

## Parent
[[Inégalités technologiques]]

## Siblings (frères/sœurs)
* [[Accès IA par région]]
* [[Formation et divide]]

## Concepts liés horizontalement
* [[Métacompétences]] — formation pour tokens
* [[Transformation vs Destruction]] — risque déclassement

#tokens #inegalites
```

### Registry : Type field
```
| Concept | Type | Parent |
|---------|------|--------|
| Inégalités technologiques | parent | - |
| Inégalités et Token Economy | child | [[Inégalités technologiques]] |
| Transformation vs Destruction | standalone | - |
```

### Règle d'or : Max 2 niveaux
```
Parent
├─ Enfant ← OK
│  └─ Petit-enfant ← NON (trop profond)
```

---

## 3. TEMPLATE : PAGE CONCEPT

### Structure
```markdown
# [Titre du concept]

[Définition claire : 2-3 phrases max]

## Idées clés
* Idée 1 (1 ligne)
* Idée 2 (1 ligne)
* Idée 3 (1 ligne)
[max 5 idées]

## Questions ouvertes
* Question 1 ?
* Question 2 ?
[max 3 questions]

## Sources
* [[Nom article compilé]]
[Lien SEULEMENT vers article compilé, pas vers raw/]

#tag1 #tag2 #tag3
```

### Règles spécifiques

✅ **À faire**
- Synthétiser l'essence de l'idée (pas tout l'article)
- Une phrase = une idée (lisibilité)
- Linker vers 1-2 articles compilés maximum
- Ajouter 3-5 tags pertinents

❌ **À NE PAS FAIRE**
- Lister tous les détails (c'est pour l'article compilé)
- Inclure des citations longues (max 1 phrase)
- Linker directement vers raw/ (passe par article compilé)
- Dupliquer le contenu de l'article compilé
- Créer des "sous-concepts" (une idée = un concept)

### Exemple
```markdown
# Transformation vs Destruction d'Emploi

L'IA ne détruit pas les emplois, elle redistribue les tâches. 
Pour la première fois, les tâches cognitives qualifiées sont exposées 
à l'automatisation — créant un déséquilibre entre déplacement et réintégration.

## Idées clés
* Trois niveaux : Emploi ≠ Compétence ≠ Tâche
* Rupture historique : tâches cognitives exposées (never before)
* Déplacement > Réintégration : risque de chômage structurel
* Exemple : traduction = IA fait jets, humans font validation
* Geographic inversion : expose métropoles + travailleurs qualifiés

## Questions ouvertes
* Comment accélérer création de tâches de réintégration ?
* Quelles politiques d'accompagnement ?

## Sources
* [[L'IA générative ne détruira pas votre emploi]]

#transformation #emploi #competences #taches
```

---

## 4. TEMPLATE : ARTICLE COMPILÉ

### Structure
```markdown
# [Titre original de l'article]

**Auteur** : [Nom]
**Source** : [Journal/Site](URL)
**Date** : YYYY-MM-DD
**Fichier brut** : [raw/filename.md](../../raw/filename.md)

## Résumé
[1-2 paragraphes synthétisant l'article]

## Concepts clés
* [[Concept 1]]
* [[Concept 2]]
* [[Concept 3]]
[Linker vers concepts wiki]

## Citations pertinentes
> Citation 1 (2-3 lignes max)

> Citation 2

## Structure de l'article original
[Optionnel : bullets avec structure article si complexe]

## Pensées / Connexions
[Optionnel : ta réflexion personnelle ou connexions non-évidentes]

#article #source #year2026
```

### Règles spécifiques

✅ **À faire**
- Résumer l'article en 2-3 paragraphes max
- Extraire 3-5 citations pertinentes (2-3 lignes chacune)
- Linker vers concepts wiki existants
- Garder métadonnées claires (auteur, date, source)
- **Ajouter toujours** un lien direct vers raw/filename.md

❌ **À NE PAS FAIRE**
- Copier l'article entier (c'est pour raw/)
- Linker vers d'autres articles compilés (c'est "horizontal", pas "vertical")
- Oublier le lien vers raw/
- Ajouter du contenu qui n'est pas dans l'article

### Exemple
```markdown
# L'IA générative ne détruira pas votre emploi

**Auteur** : Hugo Spring-Ragain
**Source** : [The Conversation](https://theconversation.com/...)
**Date** : 2026-04-20
**Fichier brut** : [raw/article-spring-ragain-2026.md](../../raw/article-spring-ragain-2026.md)

## Résumé
Cet article démêle trois niveaux confondus dans le débat public : emploi, 
compétence, tâche. L'IA ne détruit pas les emplois mais redistribue les tâches. 
Pour la première fois historiquement, ce sont les tâches cognitives qualifiées 
qui sont exposées à l'automatisation — inverse du pattern des 200 ans passés. 
La formation doit donc prioritariser "superviser l'IA" plutôt que "l'utiliser".

## Concepts clés
* [[Transformation vs Destruction d'Emploi]]
* [[Métacompétences et Formation]]

## Citations pertinentes
> "Ce ne sont pas les emplois qui disparaissent. Ce sont des bouquets 
> de tâches qui se redistribuent entre humains et machines."

> "Superviser efficacement un output d'IA requiert exactement ce que 
> les formations courtes et techniques peinent à développer : une culture générale solide."

#article #source #2026
```

---

## 5. TEMPLATE : INDEX (wiki/index.md)

### Structure
```markdown
# KB Index : IA Générative et Transformation du Travail

## Vue d'ensemble
[Description du KB : 2-3 phrases]
Dernière mise à jour : [DATE]
Nombre de concepts : [X]
Nombre d'articles : [Y]

## Concepts principaux
1. [[Concept 1]]
   * Idée 1
   * Idée 2

2. [[Concept 2]]
   * Idée 1
   * Idée 2

## Articles source compilés
* [[Article 1]] (Auteur, Date)
* [[Article 2]] (Auteur, Date)
* [[Article 3]] (Auteur, Date)

## Métadonnées
Dernière compilation : YYYY-MM-DD HH:MM
Status : [Work in progress / Stable]
Raw files in `/raw/` : [X articles]
```

### Règles
- Liste tous les concepts (avec 1-2 idées clés chacun)
- Liste tous les articles compilés
- Met à jour la date de compilation à chaque exécution
- C'est le point d'entrée

---

## 6. PROCESSUS DE COMPILATION

### Input : Tu fournis
```
Situation : "J'ai ajouté 2 nouveaux articles en raw/"
Fichiers : raw/article-new-1.md, raw/article-new-2.md
Action : "Update wiki"
```

### Process : Claude Code fait
```
1. Lis CONCEPTS-REGISTRY.md d'abord (inventory des concepts existants)
2. Lis TOUS les fichiers en raw/
3. Pour chaque NOUVEL article :
   a. Analyse le contenu → extrait concepts clés
   b. Cherche chaque concept dans registry (fuzzy match)
      - Si existe → RÉUTILISE [[concept]]
      - Si "proche" → enrichis existant
      - Si VRAIMENT nouveau → crée + ajoute registry
   c. Crée 1 article compilé (résumé + citations + lien vers raw/)
   d. Ajoute liens concept → article compilé
4. Récompile index.md (concepts + articles)
5. Vérifie liens (pas de 404 internes)
6. Met à jour concepts-registry.md si changements
7. Output : "Compilation done. [X concepts réutilisés, Y nouveaux, Z articles, W liens]"
```

**Clé** : Registry check = zéro duplication de concepts

### Output : Tu obtiens
- wiki/concepts/ : à jour
- wiki/articles/ : à jour
- wiki/index.md : à jour
- Confirmé : liens corrects

---

## 7. CHECKLIST DE COMPILATION

À chaque compilation, Claude Code doit vérifier :

- [ ] **Concepts Registry**
  - [ ] Registry.md lue AVANT compilation
  - [ ] Chaque concept check dans registry (fuzzy match)
  - [ ] Concepts existants réutilisés prioritairement
  - [ ] Uniquement nouveaux concepts créés si vraiment nouveau (non-redundant)
  - [ ] Registry mis à jour avec nouveaux concepts + types + parents

- [ ] **Concepts**
  - [ ] Chaque concept a titre + définition + idées clés + questions
  - [ ] Chaque concept linke vers 1-2 articles compilés SEULEMENT
  - [ ] Pas de lien direct vers raw/
  - [ ] Max 5 idées par concept
  - [ ] Pas de duplication entre concepts
  - [ ] Parent/enfant et liens horizontaux bien formattés

- [ ] **Articles compilés**
  - [ ] Métadonnées complètes (auteur, date, source, raw link)
  - [ ] Résumé 2-3 paragraphes max
  - [ ] Lien direct vers raw/filename.md présent
  - [ ] Citations pertinentes (2-3 max, 2-3 lignes chacune)
  - [ ] Concepts liés présents et corrects

- [ ] **Index**
  - [ ] Liste tous les concepts avec 1-2 idées clés
  - [ ] Liste tous les articles compilés
  - [ ] Date de compilation à jour
  - [ ] Compte : [X concepts, Y articles]

- [ ] **Liens internes**
  - [ ] Concept → Article compilé (bidirectionnel)
  - [ ] Article compilé → raw/ (avec chemin relatif correct)
  - [ ] Pas de lien "mort" ou 404
  - [ ] Pas de lien concept → raw/

- [ ] **Format**
  - [ ] Markdown valide (pas d'erreurs de syntaxe)
  - [ ] Tags pertinents sur chaque page
  - [ ] Pas de redondance de contenu

---

## 8. INSTRUCTIONS POUR CLAUDE CODE

### À copier/coller dans chaque prompt de compilation

```
COMPILATION TASK

Tu vas compiler la KB Shauri selon COMPILATION_SPEC.md (lis-le d'abord).

Règles à appliquer :

1. STRUCTURE
   - Concepts dans wiki/concepts/
   - Articles compilés dans wiki/articles/
   - raw/ jamais modifié
   - Liens : Concept → Article compilé → raw/

2. CONCEPTS
   - Définition claire (2-3 phrases)
   - 3-5 idées clés (une phrase chacune)
   - 2-3 questions ouvertes
   - Linke vers 1-2 articles compilés SEULEMENT

3. ARTICLES COMPILÉS
   - Métadonnées : auteur, date, source, raw link
   - Résumé : 2-3 paragraphes max
   - Lien brut toujours présent : [raw/filename.md](../../raw/filename.md)
   - Citations pertinentes (2-3 max)
   - Concepts liés

4. INDEX
   - Liste tous les concepts (+ 1-2 idées)
   - Liste tous les articles (+ auteur date)
   - Date de compilation
   - Status

5. VÉRIFICATION
   - Checklist complète (voir COMPILATION_SPEC.md section 7)
   - Pas de lien direct concept → raw/
   - Pas de redondance

Output :
- Confirme : [X concepts créés/mis à jour, Y articles, Z liens vérifiés]
- Liste les nouveaux concepts/articles (si any)
```

---

## 9. EXEMPLE : TOI + CLAUDE CODE (dialogue type)

### Toi (samedi 10h)
```
J'ai clippé 3 nouveaux articles en raw/:
- raw/article-smith-2026.md
- raw/article-chen-2026.md  
- raw/article-patel-2026.md

Compile la wiki avec COMPILATION_SPEC.md
```

### Claude Code (réponse)
```
Lisant COMPILATION_SPEC.md...

Compilation en cours:
1. Analysant raw/article-smith-2026.md → crée concept "X", article compilé
2. Analysant raw/article-chen-2026.md → enrichit concept "Y", article compilé
3. Analysant raw/article-patel-2026.md → crée concept "Z", article compilé

Mise à jour index.md... Vérification liens... ✓

COMPILATION DONE
- 3 articles compilés
- 2 nouveaux concepts créés
- 1 concept existant enrichi
- 12 liens internes vérifiés
- Checklist : ✓ Tous critères OK

Wiki prête. Dernière update : 2026-05-18 10:15 UTC
```

### Toi (vendredi 14h, question)
```
Lis ma wiki et réponds : 
"Comment ça affecte la formation en Afrique francophone ?"
```

### Claude Code
```
Lisant wiki/concepts/ uniquement...
Synthèse depuis :
- [[Métacompétences et Formation]]
- [[Transformation vs Destruction d'Emploi]]
- [[Inégalités et Token Economy]]

Réponse synthétisée...
```

---

## 10. NOTES IMPORTANTES

1. **CHAQUE COMPILATION** : Claude Code lit ce spec en entier avant de compiler
2. **PAS DE DÉVIATION** : Le spec est le contrat. Pas d'exceptions.
3. **CROISSANCE LINÉAIRE** : À chaque ajout, la wiki grandit mais reste structurée
4. **HUMAIN LISIBLE** : Obsidian va rendre ça très joli. Le markdown doit être lisible même en plain text.
5. **IA EFFICACE** : Quand tu interroges, l'IA lit juste concepts/ (pas articles ni raw/)
6. **TRAÇABILITÉ** : raw/ = archive complète, articles/ = résumés, concepts/ = idées synthétiques

---

## 11. MISE À JOUR DU SPEC

Si tu veux changer la structure/règles :
1. Modifie ce fichier
2. Explique à Claude Code : "Nouvelle version de COMPILATION_SPEC.md — compile avec ça"
3. Nouvelle compilation applique les nouvelles règles

Historique des versions :
- v1.0 : 2026-05-13 — initial (3 articles, structure basique)


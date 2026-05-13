# Concepts Registry — KB Shauri

Inventaire centralisé de tous les concepts existants. 
**À consulter AVANT chaque compilation pour éviter les doublons.**

## Format

| Concept | Slug | Clés équivalentes | Type | Parent | Enfants | Liens horizontaux | # Articles | Notes |
|---------|------|------------------|------|--------|---------|------------------|------------|-------|
| [Titre exact du concept] | `slug-to-file` | mot1, mot2, synonymes | standalone/parent/child | [[parent]] | [[enfant1]], [[enfant2]] | [[concept1]], [[concept2]] | 3 | Commentaire |

## Registry actuelle

| Concept | Slug | Clés équivalentes | Type | Parent | Enfants | Liens horizontaux | # Articles | Notes |
|---------|------|------------------|------|--------|---------|------------------|------------|-------|
| **Transformation vs Destruction d'Emploi** | `transformation-vs-destruction` | emploi, automation, jobs, tâches, salaires, redistribution | parent | - | [[Métacompétences]] | [[Inégalités et Token Economy]] | 2 | Distinction clé : emploi ≠ compétence ≠ tâche. Nouvelle rupture : tâches cognitives exposées. |
| **Métacompétences et Formation** | `metacompetences` | supervision IA, culture générale, critical thinking, jugement, formation, apprentissage | parent | - | - | [[Transformation vs Destruction]], [[Inégalités et Token Economy]] | 2 | Superviser > utiliser. Culture générale revient centrale. |
| **Inégalités et Token Economy** | `inegalites-acces` | token economy, divide, classe bipolaire, silicon valley, salaires, productivité, augmented employee | child | [[Inégalités technologiques]] | - | [[Transformation vs Destruction]], [[Métacompétences]] | 1 | Tokens = nouvelle mesure de pouvoir. Risque classe bipolaire. |
| **Impact sur la Recherche Scientifique** | `impact-recherche` | hallucinations, opacité, sciences sociales, transparence, algorithmes, responsabilité, secret | standalone | - | - | - | 1 | Hallucinations et opacité = risques majeurs pour recherche. Culture du secret chez chercheurs. |

## Synonymes et variations à matcher

**Pour "Transformation vs Destruction"**, Claude Code doit matcher :
- "emploi" / "emplois"
- "destruction d'emploi" / "job destruction"
- "automatisation" / "automation"
- "redistribution tâches" / "task redistribution"
- "salaire" / "wages"
- "restructuration" / "restructuring"

**Pour "Métacompétences"**, Claude Code doit matcher :
- "superviser IA" / "supervise AI"
- "culture générale" / "general knowledge"
- "critical thinking" / "esprit critique"
- "formation" / "training"
- "learning" / "apprentissage"
- "skills" / "compétences"

**Pour "Inégalités et Token Economy"**, Claude Code doit matcher :
- "tokens" (unité IA)
- "divide" / "divides"
- "classe bipolaire" / "bipolar class"
- "productivité" / "productivity"
- "salaires" / "salaries"
- "augmented employee"
- "sans tokens" / "without access"

**Pour "Impact recherche"**, Claude Code doit matcher :
- "hallucination" / "hallucinations"
- "opacité" / "opacity" / "black box"
- "transparence" / "transparency"
- "sciences sociales" / "social sciences"
- "algorithme" / "algorithms"
- "responsabilité" / "accountability"
- "secret" / "secrecy"

## Règles pour fuzzy matching

Quand Claude Code analyse un nouvel article :

1. **Extraction mots-clés** : tire 10-15 mots-clés importants
2. **Matching** : pour chaque concept dans registry, compte combien de mots-clés match
3. **Score** :
   - `>80% match` = Réutilise + enrichit existant
   - `50-80% match` = "Closely related". Enrichit existant ET note la connexion
   - `<50% match` = Considère nouveau concept
4. **Double-check** : si doute, préfère réutiliser = "quand en doute, réutilise"

## Évolution du registry

À chaque compilation où nouveaux concepts créés :

```
2026-05-13 (v1) : 4 concepts, 3 articles
2026-05-20 (v2) : +1 nouveau concept "X", registry updatée
2026-05-27 (v3) : "Y" fusionné dans "X", registry updatée
```

---

## Comment utiliser ce registry en practice

### Tu ajoutes article sur "Compétences au 21e siècle"

Claude Code :
```
1. Lit concepts-registry
2. Extrait mots-clés : ["competences", "superviser", "IA", 
   "culture générale", "formation", "automation", "emplois", ...]
3. Matcher :
   - "superviser" → 95% match [[Métacompétences]]
   - "automation" → 80% match [[Transformation vs Destruction]]
   - "formation" → 85% match [[Métacompétences]]
   - "emplois" → 75% match [[Transformation vs Destruction]]
4. Décision :
   - Réutilise [[Métacompétences]] (enrichit)
   - Ajoute lien horizontal vers [[Transformation vs Destruction]]
   - Pas de nouveau concept
5. Output : "Registry check OK. Réutilisé 2 concepts. Nouveaux : 0"
```

### Tu ajoutes article sur "IA et biotechnologie"

Claude Code :
```
1. Lis registry
2. Extrait mots-clés : ["biotechnologie", "molecules", "R&D", 
   "découverte", "science", "opacité", "hallucinations", ...]
3. Matcher :
   - "opacité", "hallucinations" → 70% match [[Impact recherche]]
   - Autres mots = pas de match
4. Décision :
   - Enrichit [[Impact recherche]]
   - Mais aussi : émerge potentiellement "Applications IA secteurs"
   - Question : est-ce un nouveau concept standalone ?
   - Réponse : "Applications IA" serait trop vague. Reste focus sur [[Impact recherche]].
   - Output : "Registry check OK. Enrichi 1 concept. Nouveaux : 0"
```

### Tu ajoutes article sur "Régulation IA en Afrique"

Claude Code :
```
1. Lis registry
2. Extrait mots-clés : ["régulation", "Afrique", "policy", "accès", 
   "gouvernement", "inégalités", "divide", "formations", ...]
3. Matcher :
   - "inégalités", "divide", "accès" → 75% match [[Inégalités et Token Economy]]
   - "Afrique" + "régulation" = pas vraiment dans registry
4. Décision :
   - Enrichit [[Inégalités et Token Economy]] (manifestation géographique)
   - Considère : création parent concept "Inégalités technologiques"
     avec enfants : [[Inégalités Token Economy]], [[Accès Afrique]], [[Formation divide]]
   - Output : "Registry check. Créé parent concept [[Inégalités technologiques]], 
     réorganisé. Enfants : 3. Nouveaux concepts : 1 (parent)"
   - Registry updatée
```

---

## Exemple complet : Initial Registry creation

À ton setup initial (avant article 1) :

```
Tu as 3 articles.
Claude Code analyse :

Article 1 (Spring-Ragain) : transformation vs destruction
  → Crée concept [[Transformation vs Destruction d'Emploi]]

Article 2 (Leparmentier) : tokens, inégalités
  → Check registry : [[Transformation vs Destruction]] existe
     Aussi nouveau angle : Inégalités
  → Crée [[Inégalités et Token Economy]] (nouveau)
  → Ajoute lien horizontal vers [[Transformation vs Destruction]]

Article 3 (Glavas) : recherche, opacité, hallucinations
  → Check registry : pas de match
  → Crée [[Impact sur la Recherche Scientifique]] (nouveau)

Registry finale (v1) :
- 3 concepts
- 3 articles
- 2 liens horizontaux
```

---

## Maintenance du registry

Chaque compilation, Claude Code met à jour :
- Nombre d'articles par concept (# Articles)
- Liens horizontaux nouveaux
- Parents/enfants si hiérarchie créée
- Notes si clarifications
- Historique des versions

**Tu ne touches pas le registry toi-même.** Claude Code le gère.

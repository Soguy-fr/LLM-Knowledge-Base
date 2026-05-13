# KB Shauri : Guide d'utilisation quotidien

## En 30 secondes

Tu as 3 dossiers clés :

```
clipping/  ← Tu ajoutes articles ici (Web Clipper ou upload manuel)
raw/       ← Claude Code nettoie et archive
wiki/      ← Claude Code compile (concepts + articles)
```

Tu demandes à Claude Code : **"Update wiki"**

Il :
1. Vérife registry pour éviter doublons de concepts
2. Compile articles en résumés
3. Crée/enrichit concepts
4. Mets à jour index

Done. Wiki prête à consulter ou interroger.

---

## Workflow quotidien

### Lundi : Tu ajoutes des articles

```
1. Web Clipper : clique "Save to Obsidian" sur 3 articles
   → Créé clipping/article-1.md, clipping/article-2.md, clipping/article-3.md

2. OU upload manuel : télécharge PDF → place dans clipping/
   → clipping/rapport-2026.pdf

3. Dis à Claude Code :
   "Update wiki. Fichiers en clipping/ : 4 nouveaux"
```

### Claude Code (traitement automatique)

```
1. Lis clipping/ → nettoie + bouge en raw/
2. Lis concepts-registry.md
3. Pour chaque article :
   - Extrait concepts
   - Check registry (fuzzy match)
   - Réutilise si existe
   - Crée SEULEMENT si vraiment nouveau
4. Met à jour wiki/concepts, wiki/articles, wiki/index.md
5. Sauvegarde registry updatée

Output : "Wiki updated. Concepts réutilisés: 3, Nouveaux: 1, Articles: 4"
```

### Mercredi : Tu consultes ou interroges

**Option A : Tu naviges dans Obsidian**
```
- Ouvres wiki/
- Lis concepts (lisibles, bien structurés)
- Cliques sur liens → articles compilés
- Cliques sur lien brut → source originale
```

**Option B : Tu interroges l'IA**
```
"Lis ma wiki. Comment ça s'applique à Shauri en contexte M&E ?"

Claude lit SEULEMENT wiki/concepts/ :
- Pas d'articles ni raw/ (trop lourd)
- Synthétise + répond
```

---

## Structure récap

### clipping/ (ENTRÉE)
- Web Clipper output (auto-créé)
- PDFs uploads manuels
- **Temporaire** (vidé après chaque compilation)

### raw/ (ARCHIVE)
- Fichiers nettoyés
- Jamais modifiés après
- Source de vérité pour chaque article

### wiki/concepts/ (SYNTHÈSE)
- Idées clés seulement (2-3 phrases)
- 3-5 idées clés par concept
- 1-3 questions ouvertes
- Liens VERS articles compilés seulement
- **C'est ce que l'IA lit pour répondre**

### wiki/articles/ (RÉSUMÉS)
- Résumé 2-3 paragraphes
- 2-3 citations pertinentes
- Lien vers raw/
- Liens vers concepts

### wiki/concepts-registry.md (CONTRÔLE)
- Inventaire de tous les concepts existants
- Clés d'équivalence (fuzzy match)
- Évite doublons
- Claude Code la lit AVANT chaque compilation

---

## 3 cas d'usage

### Cas 1 : "Je veux ajouter un article"
```
1. Web Clipper → clipping/
2. Claude Code : "Update wiki"
3. Registry checkée, concepts réutilisés, zéro doublons
Done
```

### Cas 2 : "Je veux répondre à une question sur ma KB"
```
Claude Code lit wiki/concepts/ seulement
Synthétise et répond
Réponse peut être sauvée dans wiki/ pour enrichir
```

### Cas 3 : "Je veux lire un article complètement"
```
Concept → cliques lien → Article compilé
Article compilé → cliques lien → Source brute (raw/)
Profondeur 3 niveaux de détail
```

---

## Checklist : Avant chaque "Update wiki"

Tu dis à Claude Code :

```
✅ J'ai [X] nouveaux fichiers en clipping/
✅ COMPILATION_SPEC.md t'aide (tu la lis d'abord)
✅ concepts-registry.md t'aide (check AVANT créer concepts)
✅ Réutilise concepts existants prioritairement
✅ Lis la spec section "8. Instructions pour Claude Code"

Now: Update wiki
```

---

## Important

### Tu NE touches PAS
- raw/ (archive, sacré)
- wiki/concepts-registry.md (Claude Code la gère)
- wiki/articles/*.md (Claude Code les génère)
- wiki/concepts/*.md (Claude Code les génère)

### Tu PEUX
- Ajouter fichiers en clipping/
- Demander des mises à jour
- Interroger la wiki
- Lire les fichiers (consulter, pas éditer)
- Modifier COMPILATION_SPEC.md si tu veux changer les règles

---

## TL;DR : Flux complet

```
MONDAY 10am
├─ Tu clipes 3 articles → clipping/
├─ Tu dis "Update wiki"
└─ Claude Code fait le travail
   ├─ Nettoie + move raw/
   ├─ Check registry
   ├─ Compile concepts
   ├─ Compile articles
   └─ Update index

MONDAY 2pm
├─ Tu ouvres Obsidian
├─ Lis wiki/ (lisible + structuré)
└─ Tu consultes ou tu penses

TUESDAY
├─ Tu as une question : "Lis ma wiki, dis-moi..."
├─ Claude Code lit wiki/concepts/ seulement
└─ Il répond en 30 secondes

Repeat.
```

---

## Questions fréquentes

**Q: Et si j'ajoute un PDF de 100 pages ?**
A: Claude Code demande : "Extraire texte complet ou résumé ?" Toi tu dis. Lui il traite.

**Q: Peut-on créer plusieurs niveaux de concepts (parent/enfant/petit-enfant) ?**
A: NON. Max 2 niveaux. Parent → enfants. Rien au-delà. (Trop complexe)

**Q: Si je crée manuellement un fichier en wiki/concepts/, il se fait overwrite ?**
A: OUI. Claude Code regénère tous les concepts. Ne pas éditer manuellement. C'est son job.

**Q: Comment j'accède à la wiki dans Claude Chat (pas Code) ?**
A: Upload le dossier wiki/ complet (PDF ou ZIP). Claude lit tout et répond. Mais c'est ponctuel — mieux d'utiliser Code où c'est persistant.

**Q: Registry : pourquoi elle existe ?**
A: Éviter : Article 1 → crée concept "IA", Article 2 → crée concept "Intelligence Artificielle", Article 3 → crée "AI". Maintenant = 1 concept, 3 noms. Registry = "IA" est la clé, variantes listées.

**Q: Je veux fusionner 2 concepts ?**
A: Dit à Claude Code : "Fusionne [[Concept A]] et [[Concept B]]. B disparaît, articles se relient à A." Il fait. Registry updatée.

---

## Commandes type pour Claude Code

```
"Update wiki"
→ Compile avec fichiers en clipping/

"Update wiki. Fichiers : article-x.md, article-y.md"
→ Spécifie lesquels (optionnel)

"Registry status"
→ Affiche combien de concepts, articles, etc.

"Lis ma wiki et réponds : comment ça s'applique à l'Afrique francophone ?"
→ Interroge wiki/concepts/

"Fusionne [[Concept A]] et [[Concept B]]. B disparaît."
→ Réorganise

"Crée parent concept [[X]] avec enfants [[Y]], [[Z]]"
→ Hiérarchie

"Linting : trouves-moi les incohérences dans la wiki"
→ Claude Code cherche erreurs, doublons, liens cassés, etc.
```

---

## Timeline exemple

```
Semaine 1
├─ Setup : COMPILATION_SPEC.md + registry initiale
├─ Article 1-3 : Clip + "Update wiki"
└─ Wiki : 4 concepts, 3 articles

Semaine 2
├─ Article 4-6 : Clip + "Update wiki"
├─ Claude Code : réutilise 4 concepts existants, crée 1 nouveau
└─ Wiki : 5 concepts, 6 articles

Semaine 3
├─ Tu interroges : "M&E + IA, comment ?"
├─ Claude lit concepts, synthétise
└─ Réponse sauvée en wiki/ pour futur

Semaine 4
├─ Article 7-10 : géré comme avant
└─ Wiki : 8 concepts, 10 articles (grandit, reste lean)
```

---

## Support

Questions sur :
- **COMPILATION_SPEC.md** : regarde la section pertinente
- **concepts-registry-EXEMPLE.md** : vois comment ça marche
- **wiki/concepts-registry.md** : l'inventaire actuelle (générée)
- **Commandes Claude Code** : voir section "Commandes type"

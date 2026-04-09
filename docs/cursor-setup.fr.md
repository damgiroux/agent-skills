# Utilisation de agent-skills avec Cursor

Ce guide couvre l'intégration de agent-skills dans l'éditeur Cursor. ([English version](cursor-setup.md))

## Configuration

### Option 1 : Répertoire de Règles (Recommandé)

Cursor prend en charge un répertoire `.cursor/rules/` pour les règles spécifiques au projet :

```bash
# Créer le répertoire de règles
mkdir -p .cursor/rules

# Copier les skills que vous voulez comme règles
cp /path/to/agent-skills/skills/test-driven-development/SKILL.md .cursor/rules/test-driven-development.md
cp /path/to/agent-skills/skills/code-review-and-quality/SKILL.md .cursor/rules/code-review-and-quality.md
cp /path/to/agent-skills/skills/incremental-implementation/SKILL.md .cursor/rules/incremental-implementation.md
```

Les règles de ce répertoire sont automatiquement chargées dans le context de Cursor.

### Option 2 : Fichier .cursorrules

Créez un fichier `.cursorrules` à la racine de votre projet avec les skills essentiels intégrés :

```bash
# Générer un fichier de règles combiné
cat /path/to/agent-skills/skills/test-driven-development/SKILL.md > .cursorrules
echo "\n---\n" >> .cursorrules
cat /path/to/agent-skills/skills/code-review-and-quality/SKILL.md >> .cursorrules
```

### Option 3 : Notepads

La fonctionnalité Notepads de Cursor vous permet de stocker du context réutilisable. Créez un notebook pour chaque skill que vous utilisez fréquemment :

1. Ouvrez Cursor → Settings → Notepads
2. Créez un nouveau notepad nommé "swe: Test-Driven Development"
3. Collez le contenu de `skills/test-driven-development/SKILL.md`
4. Référencez-le dans le chat avec `@notepad swe: Test-Driven Development`

## Configuration Recommandée

### Skills Essentiels (À toujours charger)

Ajoutez ceux-ci à `.cursor/rules/` :

1. `test-driven-development.md` — Workflow TDD et pattern Prove-It.
2. `code-review-and-quality.md` — Revue sur cinq axes.
3. `incremental-implementation.md` — Construire en petites tranches vérifiables.

### Skills Spécifiques à une Phase (Charger comme Notepads)

Créez des notepads pour les skills que vous utilisez contextuellement :

- "swe: Spec Development" → `spec-driven-development/SKILL.md`
- "swe: Frontend UI" → `frontend-ui-engineering/SKILL.md`
- "swe: Security" → `security-and-hardening/SKILL.md`
- "swe: Performance" → `performance-optimization/SKILL.md`

Référencez-les avec `@notepad` lorsque vous travaillez sur des tâches pertinentes.

## Astuces d'Utilisation

1. **Ne pas charger tous les skills en même temps** — Cursor a des limites de context. Chargez 2-3 skills comme règles et gardez les autres comme notepads.
2. **Référencer les skills explicitement** — Dites à Cursor "Suis les règles test-driven-development pour ce changement" pour vous assurer qu'il lit les règles chargées.
3. **Utiliser les agents pour la revue** — Copiez le contenu de `agents/code-reviewer.md` et dites à Cursor de "revoir ce diff en utilisant ce framework de revue de code".
4. **Charger les références à la demande** — Lorsque vous travaillez sur la performance, référencez `@notepad performance-checklist` ou collez le contenu de la checklist.

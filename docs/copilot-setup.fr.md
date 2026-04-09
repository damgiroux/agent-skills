# Utilisation de agent-skills avec GitHub Copilot

Ce guide explique comment intégrer agent-skills dans votre workflow GitHub Copilot. ([English version](copilot-setup.md))
## Configuration

### Instructions Copilot

Copilot prend en charge la création de skills d'agent en utilisant un répertoire `.github/skills`, `.claude/skills` ou `.agents/skills` dans votre repository.

```bash
mkdir -p .github

# Créer les fichiers pour les skills essentiels
cat /path/to/agent-skills/skills/test-driven-development/SKILL.md > .github/skills/test-driven-development/SKILL.md
cat /path/to/agent-skills/skills/code-review-and-quality/SKILL.md > .github/skills/code-review-and-quality/SKILL.md
```

Pour plus de détails, reportez-vous à [Creating agent skills for GitHub Copilot](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/coding-agent/create-skills).

### Personas d'Agent (agents.md)

Copilot prend en charge les personas d'agent spécialisés. Utilisez les agents de agent-skills :

```bash
# Copier les définitions d'agent
cp /path/to/agent-skills/agents/code-reviewer.md .github/agents/code-reviewer.md
cp /path/to/agent-skills/agents/test-engineer.md .github/agents/test-engineer.md
cp /path/to/agent-skills/agents/security-auditor.md .github/agents/security-auditor.md
```

Invoquez les agents dans Copilot Chat :
- `@code-reviewer Review this PR`
- `@test-engineer Analyze test coverage for this module`
- `@security-auditor Check this endpoint for vulnerabilities`

### Instructions Personnalisées (Niveau Utilisateur)

Pour les skills que vous souhaitez avoir dans tous vos repositories :

1. Ouvrez VS Code → Settings → GitHub Copilot → Custom Instructions
2. Ajoutez les résumés de vos skills les plus utilisés.

## Configuration Recommandée

### .github/copilot-instructions.md

GitHub Copilot prend en charge les instructions au niveau du projet via `.github/copilot-instructions.md`.

```markdown
# Standards de Codage du Projet

## Testing
- Écrire les tests avant le code (TDD)
- Pour les bugs : écrire d'abord un test qui échoue, puis corriger (pattern Prove-It)
- Hiérarchie des tests : unit > integration > e2e (utiliser le niveau le plus bas qui capture le comportement)
- Exécuter `npm test` après chaque changement

## Qualité du Code
- Revue sur cinq axes : correctness, readability, architecture, security, performance
- Chaque PR doit passer : lint, type check, tests, build
- Aucun secret dans le code ou le contrôle de version

## Implémentation
- Construire par petites incréments vérifiables
- Chaque incrément : implement → test → verify → commit
- Ne jamais mélanger des changements de formatage avec des changements de comportement

## Limites (Boundaries)
- Toujours : Exécuter les tests avant les commits, valider les entrées utilisateur
- Demander d'abord : Changements de schéma de base de données, nouvelles dépendances
- Jamais : Committer des secrets, supprimer des tests qui échouent, sauter la vérification
```

### Agents Spécialisés

Utilisez les agents pour des workflows de revue ciblés dans Copilot Chat.

## Astuces d'Utilisation

1. **Garder les instructions concises** — Les instructions Copilot fonctionnent mieux lorsqu'elles sont focalisées. Résumez les règles clés plutôt que d'inclure des fichiers de skill complets.
2. **Utiliser les agents pour la revue** — Les agents code-reviewer, test-engineer et security-auditor sont conçus pour le modèle d'agent de Copilot.
3. **Référence dans le chat** — Lorsque vous travaillez sur une phase spécifique, collez le contenu du skill pertinent dans Copilot Chat pour le context.
4. **Combiner avec les revues de PR** — Configurez Copilot pour revoir les PR en utilisant le persona d'agent code-reviewer.

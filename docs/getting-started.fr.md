# Prise en main de agent-skills

agent-skills fonctionne avec n'importe quel AI coding agent qui accepte des instructions Markdown. Ce guide couvre l'approche universelle. Pour une configuration spécifique à un outil, consultez les guides dédiés.

## Fonctionnement des Skills

Chaque skill est un fichier Markdown (`SKILL.md`) qui décrit un workflow d'ingénierie spécifique. Lorsqu'il est chargé dans le context de l'agent, celui-ci suit le workflow — y compris les verification steps, les anti-patterns à éviter et les exit criteria.

**Les Skills ne sont pas des documents de référence.** Ce sont des processus étape par étape que l'agent doit suivre.

## Démarrage Rapide (Tout Agent)

### 1. Cloner le repository

```bash
git clone https://github.com/addyosmani/agent-skills.git
```

### 2. Choisir un skill

Parcourez le répertoire `skills/`. Chaque sous-répertoire contient un fichier `SKILL.md` avec :
- **When to use** — déclencheurs indiquant que ce skill s'applique.
- **Process** — workflow étape par étape.
- **Verification** — comment confirmer que le travail est terminé.
- **Common rationalizations** — excuses que l'agent pourrait utiliser pour sauter des étapes.
- **Red flags** — signes que le skill n'est pas respecté.

### 3. Charger le skill dans votre agent

Copiez le contenu du `SKILL.md` pertinent dans le system prompt de votre agent, son rules file ou la conversation. Les approches les plus courantes :

**System prompt :** Collez le contenu du skill au début de la session.

**Rules file :** Ajoutez le contenu du skill au rules file de votre projet (CLAUDE.md, .cursorrules, etc.).

**Conversation :** Référencez le skill lors de vos instructions : "Suis le processus test-driven-development pour ce changement."

### 4. Utiliser le meta-skill pour la découverte

Commencez par charger le skill `using-agent-skills`. Il contient un flowchart qui fait correspondre les types de tâches au skill approprié.

## Configuration Recommandée

### Minimale (Commencez ici)

Chargez trois skills essentiels dans votre rules file :

1. **spec-driven-development** — Pour définir ce qu'il faut construire.
2. **test-driven-development** — Pour prouver que cela fonctionne.
3. **code-review-and-quality** — Pour vérifier la qualité avant la fusion (merge).

Ces trois skills couvrent les lacunes de qualité les plus critiques dans le développement assisté par AI.

### Cycle de Vie Complet

Pour une couverture complète, chargez les skills par phase :

```
Démarrage d'un projet :  spec-driven-development → planning-and-task-breakdown
Pendant le développement : incremental-implementation + test-driven-development
Avant la fusion (merge) :  code-review-and-quality + security-and-hardening
Avant le déploiement :     shipping-and-launch
```

### Chargement Contextuel

Ne chargez pas tous les skills en même temps — cela gaspille du context. Chargez les skills pertinents pour la tâche en cours :

- Travail sur l'UI ? Chargez `frontend-ui-engineering`
- Débogage ? Chargez `debugging-and-error-recovery`
- Configuration du CI ? Chargez `ci-cd-and-automation`

## Anatomie d'un Skill

Chaque skill suit la même structure :

```
Frontmatter YAML (name, description)
├── Overview — Ce que fait ce skill
├── When to Use — Déclencheurs et conditions
├── Core Process — Workflow étape par étape
├── Examples — Échantillons de code et patterns
├── Common Rationalizations — Excuses et réfutations
├── Red Flags — Signes que le skill est violé
└── Verification — Checklist des exit criteria
```

Consultez [skill-anatomy.md](skill-anatomy.fr.md) pour la spécification complète.

## Utilisation des Agents

Le répertoire `agents/` contient des personas d'agents préconfigurés :

| Agent | Rôle |
|-------|---------|
| `code-reviewer.md` | Revue de code sur cinq axes |
| `test-engineer.md` | Stratégie et rédaction de tests |
| `security-auditor.md` | Détection de vulnérabilités |

Chargez une définition d'agent lorsque vous avez besoin d'une revue spécialisée. Par exemple, demandez à votre coding agent de "revoir ce changement en utilisant le persona d'agent code-reviewer" et fournissez la définition de l'agent.

## Utilisation des Commandes

Le répertoire `.claude/commands/` contient des slash commands pour Claude Code :

| Commande | Skill Invoqué |
|---------|---------------|
| `/spec` | spec-driven-development |
| `/plan` | planning-and-task-breakdown |
| `/build` | incremental-implementation + test-driven-development |
| `/test` | test-driven-development |
| `/review` | code-review-and-quality |
| `/ship` | shipping-and-launch |

## Utilisation des Références

Le répertoire `references/` contient des checklists supplémentaires :

| Référence | Utilisation avec |
|-----------|----------|
| `testing-patterns.md` | test-driven-development |
| `performance-checklist.md` | performance-optimization |
| `security-checklist.md` | security-and-hardening |
| `accessibility-checklist.md` | frontend-ui-engineering |

Chargez une référence lorsque vous avez besoin de patterns détaillés au-delà de ce que le skill couvre.

## Artifacts de Spec et de Plan

Les commandes `/spec` et `/plan` créent des artifacts de travail (`SPEC.md`, `tasks/plan.md`, `tasks/todo.md`). Traitez-les comme des **documents vivants** pendant que le travail est en cours :

- Conservez-les dans le contrôle de version pendant le développement afin que l'humain et l'agent aient une source de vérité partagée.
- Mettez-les à jour lorsque la portée (scope) ou les décisions changent.
- Si votre repo ne souhaite pas conserver ces fichiers à long terme, supprimez-les avant la fusion ou ajoutez le dossier au `.gitignore` — le workflow ne nécessite pas qu'ils soient permanents.

## Astuces

1. **Commencez par spec-driven-development** pour tout travail non trivial.
2. **Chargez toujours test-driven-development** lors de l'écriture de code.
3. **Ne sautez pas les étapes de vérification** — elles sont le but principal.
4. **Chargez les skills de manière sélective** — plus de context n'est pas toujours mieux.
5. **Utilisez les agents pour la revue** — des perspectives différentes capturent des problèmes différents.

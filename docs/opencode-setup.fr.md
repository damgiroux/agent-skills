# Configuration de OpenCode

Ce guide explique comment utiliser Agent Skills avec OpenCode d'une manière qui reflète étroitement l'expérience Claude Code (sélection automatique des skills, workflows pilotés par le cycle de vie et application stricte des processus).

## Overview

OpenCode prend en charge les `/commands` personnalisées, mais ne dispose pas d'un système de plugin natif ou d'un routage automatique des skills comme Claude Code.

À la place, nous atteignons la parité via :

- Un system prompt robuste (`AGENTS.md`)
- L'outil `skill` intégré
- Une découverte cohérente des skills à partir du répertoire `/skills`

Cela crée un **workflow piloté par l'agent** où les skills sont sélectionnés et exécutés automatiquement.

Bien qu'il soit possible de recréer `/spec`, `/plan` et d'autres commandes dans OpenCode, cette intégration utilise intentionnellement une approche pilotée par l'agent à la place :

- Les skills sont sélectionnés automatiquement en fonction de l'intention (intent).
- Les workflows sont appliqués via `AGENTS.md`.
- Aucune invocation manuelle de commande n'est requise.

Cela correspond plus étroitement au comportement de Claude Code en pratique, où les skills sont déclenchés automatiquement plutôt que manuellement.

---

## Installation

1. Clonez le repository :

```bash
git clone https://github.com/addyosmani/agent-skills.git
```

2. Ouvrez le projet dans OpenCode.

3. Assurez-vous que les fichiers suivants sont présents dans votre workspace :

- `AGENTS.md` (à la racine)
- répertoire `skills/`

Aucune installation supplémentaire n'est requise.

---

## Fonctionnement

### 1. Découverte des Skills

Tous les skills résident dans :

```
skills/<skill-name>/SKILL.md
```

Les agents OpenCode reçoivent des instructions (via `AGENTS.md`) pour :

- Détecter quand un skill s'applique.
- Invoquer l'outil `skill`.
- Suivre le skill à la lettre.

### 2. Invocation Automatique des Skills

L'agent évalue chaque demande et la fait correspondre au skill approprié.

Exemples :

- "build a feature" → `incremental-implementation` + `test-driven-development`
- "design a system" → `spec-driven-development`
- "fix a bug" → `debugging-and-error-recovery`
- "review this code" → `code-review-and-quality`

L'utilisateur n'a **pas** besoin de demander explicitement des skills.

### 3. Lifecycle Mapping (Commandes Implicites)

Le cycle de vie du développement est encodé implicitement :

- DEFINE → `spec-driven-development`
- PLAN → `planning-and-task-breakdown`
- BUILD → `incremental-implementation` + `test-driven-development`
- VERIFY → `debugging-and-error-recovery`
- REVIEW → `code-review-and-quality`
- SHIP → `shipping-and-launch`

Cela remplace les slash commands comme `/spec`, `/plan`, etc.

---

## Exemples d'Utilisation

### Exemple 1 : Développement de fonctionnalité

Utilisateur :
```
Ajoute l'authentification à cette application
```

Comportement de l'agent :
- Détecte un travail de fonctionnalité.
- Invoque `spec-driven-development`.
- Produit une spec avant d'écrire du code.
- Passe aux skills de planification et d'implémentation.

---

### Exemple 2 : Correction de bug

Utilisateur :
```
Ce endpoint renvoie des erreurs 500
```

Comportement de l'agent :
- Invoque `debugging-and-error-recovery`.
- Reproduit → localise → corrige → ajoute des gardes (guards).

---

### Exemple 3 : Revue de code

Utilisateur :
```
Revois cette PR
```

Comportement de l'agent :
- Invoque `code-review-and-quality`.
- Applique une revue structurée (correctness, design, readability, etc.).

---

## Attentes de l'Agent (Critique)

Pour que OpenCode fonctionne correctement, l'agent doit suivre ces règles :

- Toujours vérifier si un skill s'applique avant d'agir.
- Si un skill s'applique, il DOIT être utilisé.
- Ne jamais sauter les workflows requis (spec, plan, test, etc.).
- Ne pas passer directement à l'implémentation.

Ces règles sont appliquées via `AGENTS.md`.

---

## Limitations

- Pas de slash commands natives (gérées via l'intent mapping à la place).
- Pas de système de plugin (géré via le prompt + la structure).
- L'invocation des skills dépend de la conformité du modèle.

Malgré cela, le workflow correspond étroitement à Claude Code en pratique.

---

## Workflow Recommandé

Utilisez simplement le langage naturel :

- "Design a feature"
- "Plan this change"
- "Implement this"
- "Fix this bug"
- "Review this"

L'agent sélectionnera et exécutera automatiquement les bons skills.

---

## Résumé

L'intégration OpenCode fonctionne en combinant :

- Des skills structurés (ce repo).
- Des règles d'agent fortes (`AGENTS.md`).
- Une invocation automatique des skills via le raisonnement.

Cela se traduit par un **workflow d'ingénierie complet piloté par l'agent, de qualité production**, sans nécessiter de plugins ou de commandes manuelles.

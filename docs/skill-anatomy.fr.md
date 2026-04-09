# Anatomie d'un Skill

Ce document décrit la structure et le format des fichiers de skill de agent-skills. Utilisez-le comme guide lors de la contribution de nouveaux skills ou pour comprendre les skills existants.

## Emplacement des Fichiers

Chaque skill réside dans son propre répertoire sous `skills/` :

```
skills/
  skill-name/
    SKILL.md          # Requis : La définition du skill
    supporting-file.md # Optionnel : Matériel de référence chargé à la demande
```

## Format du SKILL.md

### Frontmatter (Requis)

```yaml
---
name: skill-name-with-hyphens
description: Brève déclaration de ce que fait le skill. À utiliser lorsque [conditions de déclenchement spécifiques].
---
```

**Règles :**
- `name` : Minuscules, séparées par des traits d'union. Doit correspondre au nom du répertoire.
- `description` : Commence par ce que fait le skill (troisième personne), suivi des conditions de déclenchement. Inclure à la fois le *quoi* et le *quand*. Maximum 1024 caractères.

**Pourquoi c'est important :** Les agents découvrent les skills en lisant les descriptions. La description est injectée dans le system prompt, elle doit donc indiquer à l'agent ce que le skill fournit et quand l'activer. Ne résumez pas le workflow — si la description contient les étapes du processus, l'agent pourrait suivre le résumé au lieu de lire le skill complet.

### Sections Standards

```markdown
# Titre du Skill

## Overview
Une à deux phrases expliquant ce que fait ce skill et pourquoi il est important.

## When to Use
- Liste à puces des conditions de déclenchement (symptômes, types de tâches)
- Quand NE PAS utiliser (exclusions)

## [Core Process / The Workflow / Steps]
Le workflow principal, divisé en étapes numérotées ou en phases.
Incluez des exemples de code lorsqu'ils sont utiles.
Utilisez des flowcharts (ASCII) lorsque des points de décision existent.

## [Specific Techniques / Patterns]
Guidage détaillé pour des scénarios spécifiques.
Exemples de code, templates, configuration.

## Common Rationalizations
| Rationalization | Reality |
|---|---|
| Excuse utilisée par les agents pour sauter des étapes | Pourquoi l'excuse est fausse |

## Red Flags
- Patterns comportementaux indiquant que le skill est violé
- Choses à surveiller lors de la revue

## Verification
Après avoir terminé le processus du skill, confirmez :
- [ ] Checklist des exit criteria
- [ ] Exigences de preuves
```

## Rôles des Sections

### Overview
Le "pitch" du skill. Doit répondre à : Que fait ce skill, et pourquoi un agent devrait-il le suivre ?

### When to Use
Aide les agents et les humains à décider si ce skill s'applique à la tâche actuelle. Inclure à la fois les déclencheurs positifs ("Utiliser quand X") et les exclusions négatives ("PAS pour Y").

### Core Process
Le cœur du skill. C'est le workflow étape par étape que l'agent suit. Doit être spécifique et exploitable — pas de conseils vagues.

**Bon :** "Exécutez `npm test` et vérifiez que tous les tests passent"
**Mauvais :** "Assurez-vous que les tests fonctionnent"

### Common Rationalizations
La caractéristique la plus distinctive des skills bien conçus. Ce sont des excuses que les agents utilisent pour sauter des étapes importantes, associées à des réfutations. Elles empêchent l'agent de rationaliser sa façon d'éviter de suivre le processus.

Pensez à chaque fois qu'un agent a dit "J'ajouterai des tests plus tard" ou "C'est assez simple pour sauter la spec" — ces éléments vont ici avec un contre-argument factuel.

### Red Flags
Signes observables que le skill est violé. Utile lors de la revue de code et de l'auto-surveillance.

### Verification
Les exit criteria. Une checklist que l'agent utilise pour confirmer que le processus du skill est terminé. Chaque case à cocher doit être vérifiable avec des preuves (résultat de test, résultat de build, screenshot, etc.).

## Fichiers de Support

Créez des fichiers de support uniquement lorsque :
- Le matériel de référence dépasse 100 lignes (gardez le SKILL.md principal focalisé)
- Des outils de code ou des scripts sont nécessaires
- Les checklists sont assez longues pour justifier des fichiers séparés

Gardez les patterns et les principes inline lorsqu'ils font moins de 50 lignes.

## Principes de Rédaction

1. **Processus avant connaissance.** Les skills sont des workflows, pas des documents de référence. Des étapes, pas des faits.
2. **Spécifique avant général.** "Exécutez `npm test`" l'emporte sur "vérifiez les tests".
3. **Preuve avant supposition.** Chaque case de vérification nécessite une preuve.
4. **Anti-rationalisation.** Chaque étape susceptible d'être sautée nécessite un contre-argument dans le tableau des rationalisations.
5. **Divulgation progressive.** Le SKILL.md principal est le point d'entrée. Les fichiers de support ne sont chargés qu'en cas de besoin.
6. **Économe en tokens.** Chaque section doit justifier son inclusion. Si la supprimer ne changerait pas le comportement de l'agent, supprimez-la.

## Conventions de Nommage

- Répertoires de skill : `minuscules-separees-par-des-traits-d-union`
- Fichiers de skill : `SKILL.md` (toujours en majuscules)
- Fichiers de support : `minuscules-separees-par-des-traits-d-union.md`
- Références : stockées dans `references/` à la racine du projet, pas à l'intérieur des répertoires de skill.

## Références entre Skills

Référencez d'autres skills par leur nom :

```markdown
Suivez le skill `test-driven-development` pour l'écriture des tests.
Si le build échoue, utilisez le skill `debugging-and-error-recovery`.
```

Ne dupliquez pas le contenu entre les skills — référencez et liez à la place.

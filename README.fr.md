# Agent Skills

**Compétences d'ingénierie de qualité production pour les AI coding agents.**

Les Agent Skills encodent les workflows, les barrières de qualité (quality gates) et les meilleures pratiques que les ingénieurs seniors utilisent lors de la construction de logiciels. Celles-ci sont packagées de manière à ce que les agents AI les suivent de manière cohérente à chaque phase du développement.

```
  DEFINE          PLAN           BUILD          VERIFY         REVIEW          SHIP
 ┌──────┐      ┌──────┐      ┌──────┐      ┌──────┐      ┌──────┐      ┌──────┐
 │ Idea │ ───▶ │ Spec │ ───▶ │ Code │ ───▶ │ Test │ ───▶ │  QA  │ ───▶ │  Go  │
 │Refine│      │  PRD │      │ Impl │      │Debug │      │ Gate │      │ Live │
 └──────┘      └──────┘      └──────┘      └──────┘      └──────┘      └──────┘
  /spec          /plan          /build        /test         /review       /ship
```

---

## Commandes

7 slash commands qui correspondent au cycle de vie du développement. Chacune active automatiquement les bons skills.

| Ce que vous faites | Commande | Principe clé |
|-------------------|---------|---------------|
| Définir ce qu'il faut construire | `/spec` | Spec avant le code |
| Planifier comment le construire | `/plan` | Tâches petites et atomiques |
| Construire incrémentalement | `/build` | Une tranche à la fois |
| Prouver que cela fonctionne | `/test` | Les tests sont la preuve |
| Revoir avant la fusion (merge) | `/review` | Améliorer la santé du code |
| Simplifier le code | `/code-simplify` | La clarté avant l'ingéniosité |
| Expédier en production | `/ship` | Plus rapide est plus sûr |

Les Skills s'activent également automatiquement en fonction de ce que vous faites — concevoir une API déclenche `api-and-interface-design`, construire une UI déclenche `frontend-ui-engineering`, et ainsi de suite.

---

## Démarrage Rapide

<details>
<summary><b>Claude Code (recommandé)</b></summary>

**Installation via le Marketplace :**

```
/plugin marketplace add addyosmani/agent-skills
/plugin install agent-skills@addy-agent-skills
```

> **Erreurs SSH ?** Le marketplace clone les repos via SSH. Si vous n'avez pas de clés SSH configurées sur GitHub, vous pouvez soit [ajouter votre clé SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account), soit passer à HTTPS uniquement pour les récupérations (fetches) :
> ```bash
> git config --global url."https://github.com/".insteadOf "git@github.com:"
> ```

**Installation Locale / Développement :**

```bash
git clone https://github.com/addyosmani/agent-skills.git
claude --plugin-dir /path/to/agent-skills
```

</details>

<details>
<summary><b>Cursor</b></summary>

Copiez n'importe quel fichier `SKILL.md` dans `.cursor/rules/`, ou référencez le répertoire complet `skills/`. Voir [docs/cursor-setup.fr.md](docs/cursor-setup.fr.md).

</details>

<details>
<summary><b>Gemini CLI</b></summary>

Installez en tant que skills natifs pour la découverte automatique, ou ajoutez à `GEMINI.md` pour un context persistant. Voir [docs/gemini-cli-setup.fr.md](docs/gemini-cli-setup.fr.md).

**Installer à partir du repo :**

```bash
gemini skills install https://github.com/addyosmani/agent-skills.git --path skills
```

**Installer à partir d'un clone local :**

```bash
gemini skills install ./agent-skills/skills/
```

</details>

<details>
<summary><b>Windsurf</b></summary>

Ajoutez le contenu des skills à votre configuration de règles Windsurf. Voir [docs/windsurf-setup.fr.md](docs/windsurf-setup.fr.md).

</details>

<details>
<summary><b>OpenCode</b></summary>

Utilise l'exécution des skills pilotée par l'agent via `AGENTS.md` et l'outil `skill`.

Voir [docs/opencode-setup.fr.md](docs/opencode-setup.fr.md).

</details>

<details>
<summary><b>GitHub Copilot</b></summary>

Utilisez les définitions d'agent du répertoire `agents/` comme personas Copilot et le contenu des skills dans `.github/copilot-instructions.md`. Voir [docs/copilot-setup.fr.md](docs/copilot-setup.fr.md).

</details>

<details>
<summary><b>Codex / Autres Agents</b></summary>

Les Skills sont du simple Markdown - ils fonctionnent avec n'importe quel agent qui accepte des system prompts ou des fichiers d'instructions. Voir [docs/getting-started.fr.md](docs/getting-started.fr.md).

</details>

---

## Les 20 Skills

Les commandes ci-dessus sont les points d'entrée. Sous le capot, elles activent ces 20 skills — chacun étant un workflow structuré avec des étapes, des points de vérification et des tableaux d'anti-rationalisation. Vous pouvez également référencer n'importe quel skill directement.

### Define - Clarifier ce qu'il faut construire

| Skill | Ce qu'il fait | Quand l'utiliser |
|-------|-------------|----------|
| [idea-refine](skills/idea-refine/SKILL.md) | Pensée divergente/convergente structurée pour transformer des idées vagues en propositions concrètes | Vous avez un concept brut qui a besoin d'être exploré |
| [spec-driven-development](skills/spec-driven-development/SKILL.md) | Rédiger un PRD couvrant les objectifs, les commandes, la structure, le style de code, les tests et les limites avant tout code | Démarrage d'un nouveau projet, d'une fonctionnalité ou d'un changement important |

### Plan - Décomposer le travail

| Skill | Ce qu'il fait | Quand l'utiliser |
|-------|-------------|----------|
| [planning-and-task-breakdown](skills/planning-and-task-breakdown/SKILL.md) | Décomposer les specs en petites tâches vérifiables avec des critères d'acceptation et un ordre de dépendance | Vous avez une spec et avez besoin d'unités implémentables |

### Build - Écrire le code

| Skill | Ce qu'il fait | Quand l'utiliser |
|-------|-------------|----------|
| [incremental-implementation](skills/incremental-implementation/SKILL.md) | Tranches verticales minces - implémenter, tester, vérifier, committer. Feature flags, valeurs par défaut sûres, changements faciles à annuler | Tout changement touchant plus d'un fichier |
| [test-driven-development](skills/test-driven-development/SKILL.md) | Red-Green-Refactor, pyramide des tests (80/15/5), tailles de tests, DAMP plutôt que DRY, Beyonce Rule, tests par navigateur | Implémenter de la logique, corriger des bugs ou modifier un comportement |
| [context-engineering](skills/context-engineering/SKILL.md) | Fournir aux agents les bonnes informations au bon moment - fichiers de règles, packing de context, intégrations MCP | Démarrage d'une session, changement de tâche, ou lorsque la qualité de sortie diminue |
| [source-driven-development](skills/source-driven-development/SKILL.md) | Fonder chaque décision de framework sur la documentation officielle - vérifier, citer les sources, signaler ce qui n'est pas vérifié | Vous voulez un code faisant autorité et sourcé pour n'importe quel framework ou bibliothèque |
| [frontend-ui-engineering](skills/frontend-ui-engineering/SKILL.md) | Architecture de composants, design systems, gestion d'état, responsive design, accessibilité WCAG 2.1 AA | Construire ou modifier des interfaces utilisateur |
| [api-and-interface-design](skills/api-and-interface-design/SKILL.md) | Conception orientée contrat (contract-first), Hyrum's Law, One-Version Rule, sémantique d'erreur, validation des limites | Concevoir des APIs, des limites de modules ou des interfaces publiques |

### Verify - Prouver que cela fonctionne

| Skill | Ce qu'il fait | Quand l'utiliser |
|-------|-------------|----------|
| [browser-testing-with-devtools](skills/browser-testing-with-devtools/SKILL.md) | Chrome DevTools MCP pour les données d'exécution en direct - inspection du DOM, logs console, traces réseau, profilage de performance | Construire ou déboguer tout ce qui s'exécute dans un navigateur |
| [debugging-and-error-recovery](skills/debugging-and-error-recovery/SKILL.md) | Triage en cinq étapes : reproduire, localiser, réduire, corriger, protéger. Règle stop-the-line, replis (fallbacks) sûrs | Les tests échouent, les builds cassent ou le comportement est inattendu |

### Review - Barrières de qualité avant la fusion (merge)

| Skill | Ce qu'il fait | Quand l'utiliser |
|-------|-------------|----------|
| [code-review-and-quality](skills/code-review-and-quality/SKILL.md) | Revue sur cinq axes, dimensionnement des changements (~100 lignes), étiquettes de sévérité (Nit/Optional/FYI), normes de vitesse de revue, stratégies de découpage | Avant de fusionner tout changement |
| [code-simplification](skills/code-simplification/SKILL.md) | Chesterton's Fence, Rule of 500, réduire la complexité tout en préservant le comportement exact | Le code fonctionne mais est plus difficile à lire ou à maintenir qu'il ne le devrait |
| [security-and-hardening](skills/security-and-hardening/SKILL.md) | Prévention du Top 10 OWASP, patterns d'authentification, gestion des secrets, audit des dépendances, système de limites à trois niveaux | Manipulation d'entrées utilisateur, authentification, stockage de données ou intégrations externes |
| [performance-optimization](skills/performance-optimization/SKILL.md) | Approche orientée mesure - objectifs Core Web Vitals, workflows de profilage, analyse de bundles, détection d'anti-patterns | Des exigences de performance existent ou vous suspectez des régressions |

### Ship - Déployer avec confiance

| Skill | Ce qu'il fait | Quand l'utiliser |
|-------|-------------|----------|
| [git-workflow-and-versioning](skills/git-workflow-and-versioning/SKILL.md) | Trunk-based development, commits atomiques, dimensionnement des changements (~100 lignes), le pattern commit-as-save-point | Lors de tout changement de code (toujours) |
| [ci-cd-and-automation](skills/ci-cd-and-automation/SKILL.md) | Shift Left, Plus rapide est plus sûr, feature flags, pipelines de barrières de qualité, boucles de feedback en cas d'échec | Configuration ou modification des pipelines de build et de déploiement |
| [deprecation-and-migration](skills/deprecation-and-migration/SKILL.md) | Mentalité "le code est une dette", dépréciation obligatoire vs facultative, patterns de migration, suppression de code zombie | Suppression d'anciens systèmes, migration d'utilisateurs ou arrêt de fonctionnalités |
| [documentation-and-adrs](skills/documentation-and-adrs/SKILL.md) | Architecture Decision Records, docs API, standards de documentation inline - documenter le *pourquoi* | Prises de décisions architecturales, changement d'APIs ou déploiement de fonctionnalités |
| [shipping-and-launch](skills/shipping-and-launch/SKILL.md) | Checklists de pré-lancement, cycle de vie des feature flags, déploiements progressifs (staged rollouts), procédures de retour arrière (rollback), configuration du monitoring | Préparation du déploiement en production |

---

## Personas d'Agent

Personas de spécialistes préconfigurés pour des revues ciblées :

| Agent | Rôle | Perspective |
|-------|------|-------------|
| [code-reviewer](agents/code-reviewer.md) | Senior Staff Engineer | Revue de code sur cinq axes avec le standard "un staff engineer approuverait-il cela ?" |
| [test-engineer](agents/test-engineer.md) | Spécialiste QA | Stratégie de test, analyse de couverture et pattern Prove-It |
| [security-auditor](agents/security-auditor.md) | Ingénieur Sécurité | Détection de vulnérabilités, modélisation des menaces, évaluation OWASP |

---

## Checklists de Référence

Matériel de référence rapide que les skills utilisent en cas de besoin :

| Référence | Couvre |
|-----------|--------|
| [testing-patterns.md](references/testing-patterns.md) | Structure de test, nommage, mocking, exemples React/API/E2E, anti-patterns |
| [security-checklist.md](references/security-checklist.md) | Vérifications pré-commit, authentification, validation des entrées, headers, CORS, Top 10 OWASP |
| [performance-checklist.md](references/performance-checklist.md) | Objectifs Core Web Vitals, checklists frontend/backend, commandes de mesure |
| [accessibility-checklist.md](references/accessibility-checklist.md) | Navigation au clavier, lecteurs d'écran, design visuel, ARIA, outils de test |

---

## Fonctionnement des Skills

Chaque skill suit une anatomie cohérente :

```
┌─────────────────────────────────────────────┐
│  SKILL.md                                   │
│                                             │
│  ┌─ Frontmatter ─────────────────────────┐  │
│  │ name: lowercase-hyphen-name           │  │
│  │ description: Use when [trigger]       │  │
│  └───────────────────────────────────────┘  │
│                                             │
│  Overview         → Ce que fait ce skill    │
│  When to Use      → Conditions de déclenchement   │
│  Process          → Workflow étape par étape   │
│  Rationalizations → Excuses + réfutations     │
│  Red Flags        → Signes que quelque chose ne va pas │
│  Verification     → Exigences de preuves   │
└─────────────────────────────────────────────┘
```

**Choix de conception clés :**

- **Processus, pas prose.** Les Skills sont des workflows que les agents suivent, pas des documents de référence qu'ils lisent. Chacun a des étapes, des points de vérification et des critères de sortie.
- **Anti-rationalisation.** Chaque skill inclut un tableau des excuses courantes que les agents utilisent pour sauter des étapes (ex : "J'ajouterai les tests plus tard") avec des contre-arguments documentés.
- **La vérification n'est pas négociable.** Chaque skill se termine par des exigences de preuves - tests réussis, sortie de build, données d'exécution. "Semble correct" n'est jamais suffisant.
- **Divulgation progressive.** Le `SKILL.md` est le point d'entrée. Les références de support ne se chargent qu'en cas de besoin, minimisant l'utilisation des tokens.

---

## Structure du Projet

```
agent-skills/
├── skills/                            # 20 skills de base (un SKILL.md par répertoire)
│   ├── idea-refine/                   #   Define
│   ├── spec-driven-development/       #   Define
│   ├── planning-and-task-breakdown/   #   Plan
│   ├── incremental-implementation/    #   Build
│   ├── context-engineering/           #   Build
│   ├── source-driven-development/     #   Build
│   ├── frontend-ui-engineering/       #   Build
│   ├── test-driven-development/       #   Build
│   ├── api-and-interface-design/      #   Build
│   ├── browser-testing-with-devtools/ #   Verify
│   ├── debugging-and-error-recovery/  #   Verify
│   ├── code-review-and-quality/       #   Review
│   ├── code-simplification/          #   Review
│   ├── security-and-hardening/        #   Review
│   ├── performance-optimization/      #   Review
│   ├── git-workflow-and-versioning/   #   Ship
│   ├── ci-cd-and-automation/          #   Ship
│   ├── deprecation-and-migration/     #   Ship
│   ├── documentation-and-adrs/        #   Ship
│   ├── shipping-and-launch/           #   Ship
│   └── using-agent-skills/            #   Meta: comment utiliser ce pack
├── agents/                            # 3 personas spécialistes
├── references/                        # 4 checklists supplémentaires
├── hooks/                             # Session lifecycle hooks
├── .claude/commands/                  # 7 slash commands
└── docs/                              # Guides de configuration par outil
```

---

## Pourquoi Agent Skills ?

Les AI coding agents choisissent par défaut le chemin le plus court - ce qui signifie souvent sauter les specs, les tests, les revues de sécurité et les pratiques qui rendent le logiciel fiable. Agent Skills donne aux agents des workflows structurés qui imposent la même discipline que les ingénieurs seniors apportent au code de production.

Chaque skill encode un jugement d'ingénierie durement acquis : *quand* rédiger une spec, *quoi* tester, *comment* revoir et *quand* expédier. Ce ne sont pas des prompts génériques - ce sont le genre de workflows opinionnés et pilotés par le processus qui séparent le travail de qualité production du travail de qualité prototype.

Les skills intègrent les meilleures pratiques de la culture d'ingénierie de Google — y compris des concepts de [Software Engineering at Google](https://abseil.io/resources/swe-book) et du [guide des pratiques d'ingénierie](https://google.github.io/eng-practices/) de Google. Vous trouverez la loi d'Hyrum dans la conception d'API, la Beyonce Rule et la pyramide des tests dans les tests, le dimensionnement des changements et les normes de vitesse de revue dans la revue de code, la Chesterton's Fence dans la simplification, le trunk-based development dans le git workflow, le Shift Left et les feature flags dans le CI/CD, et un skill de dépréciation dédié traitant le code comme une dette. Ce ne sont pas des principes abstraits — ils sont intégrés directement dans les workflows étape par étape que les agents suivent.

---

## Contribution

Les Skills doivent être **spécifiques** (étapes exploitables, pas des conseils vagues), **vérifiables** (critères d'acceptation clairs avec exigences de preuves), **testés sur le terrain** (basés sur des workflows réels) et **minimaux** (uniquement ce qui est nécessaire pour guider l'agent).

Voir [docs/skill-anatomy.fr.md](docs/skill-anatomy.fr.md) pour la spécification du format et [CONTRIBUTING.md](CONTRIBUTING.md) pour les directives.

---

## Licence

MIT - utilisez ces skills dans vos projets, équipes et outils.

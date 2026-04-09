# Utilisation de agent-skills avec Gemini CLI

Ce guide explique comment configurer et utiliser agent-skills avec Gemini CLI. ([English version](gemini-cli-setup.md))

## Configuration

### Option 1 : Installer en tant que Skills (Recommandé)

Gemini CLI dispose d'un système de skills natif qui découvre automatiquement les fichiers `SKILL.md` dans les répertoires `.gemini/skills/` ou `.agents/skills/`. Chaque skill s'active à la demande lorsqu'il correspond à votre tâche.

**Installer à partir du repo :**

```bash
gemini skills install https://github.com/addyosmani/agent-skills.git --path skills
```

**Ou installer à partir d'un clone local :**

```bash
git clone https://github.com/addyosmani/agent-skills.git
gemini skills install /path/to/agent-skills/skills/
```

**Installer pour un workspace spécifique uniquement :**

```bash
gemini skills install /path/to/agent-skills/skills/ --scope workspace
```

Les skills installés au niveau du workspace vont dans `.gemini/skills/` (ou `.agents/skills/`). Les skills au niveau utilisateur vont dans `~/.gemini/skills/`.

Une fois installés, vérifiez avec :

```
/skills list
```

Gemini CLI injecte automatiquement les noms et descriptions des skills dans le prompt. Lorsqu'il reconnaît une tâche correspondante, il demande la permission d'activer le skill avant de charger ses instructions complètes.

### Option 2 : GEMINI.md (Context Persistant)

Pour les skills que vous souhaitez toujours voir chargés en tant que context projet persistant (plutôt qu'une activation à la demande), ajoutez-les au fichier `GEMINI.md` de votre projet :

```bash
# Créer GEMINI.md avec les skills de base en tant que context persistant
cat /path/to/agent-skills/skills/incremental-implementation/SKILL.md > GEMINI.md
echo -e "\n---\n" >> GEMINI.md
cat /path/to/agent-skills/skills/code-review-and-quality/SKILL.md >> GEMINI.md
```

Vous pouvez également modulariser en important depuis des fichiers séparés :

```markdown
# Project Instructions

@skills/test-driven-development/SKILL.md
@skills/incremental-implementation/SKILL.md
```

Utilisez `/memory show` pour vérifier le context chargé, et `/memory reload` pour rafraîchir après des modifications.

> **Skills vs GEMINI.md :** Les Skills sont une expertise à la demande qui s'active uniquement lorsqu'elle est pertinente, gardant votre context window propre. GEMINI.md fournit un context persistant chargé pour chaque prompt. Utilisez les skills pour les workflows spécifiques à une phase et GEMINI.md pour les conventions de projet permanentes.

## Configuration Recommandée

### Toujours Activé (GEMINI.md)

Ajoutez ceux-ci comme context persistant pour chaque session :

- `incremental-implementation` — Construire en petites tranches vérifiables.
- `code-review-and-quality` — Revue sur cinq axes.

### À la demande (Skills)

Installez-les en tant que skills afin qu'ils ne s'activent que lorsqu'ils sont pertinents :

- `test-driven-development` — S'active lors de l'implémentation de la logique ou de la correction de bugs.
- `spec-driven-development` — S'active lors du démarrage d'un nouveau projet ou d'une nouvelle fonctionnalité.
- `frontend-ui-engineering` — S'active lors de la construction de l'UI.
- `security-and-hardening` — S'active pendant les revues de sécurité.
- `performance-optimization` — S'active pendant le travail de performance.

## Configuration Avancée

### Intégration MCP

De nombreux skills de ce pack exploitent les outils du [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) pour interagir avec l'environnement. Par exemple :

- `browser-testing-with-devtools` utilise l'extension MCP `chrome-devtools`.
- `performance-optimization` peut bénéficier d'outils MCP liés à la performance.

Pour les activer, assurez-vous d'avoir installé les extensions MCP pertinentes dans votre configuration Gemini CLI (`~/.gemini/config.json`).

### Session Hooks

Gemini CLI prend en charge les session lifecycle hooks. Vous pouvez les utiliser pour injecter automatiquement du context ou exécuter des scripts de validation au début d'une session.

Pour reproduire l'expérience `agent-skills` d'autres outils, vous pouvez configurer un hook `SessionStart` qui vous rappelle les skills disponibles ou charge un meta-skill.

### Chargement Explicite du Context

Vous pouvez charger explicitement n'importe quel skill dans votre session actuelle en le référençant avec le symbole `@` dans votre prompt :

```markdown
Utilise le skill @skills/test-driven-development/SKILL.md pour implémenter ce correctif.
```

Ceci est utile lorsque vous voulez vous assurer qu'un workflow spécifique est suivi sans attendre la découverte automatique.

## Astuces d'Utilisation

1. **Préférer les skills à GEMINI.md** — Les skills s'activent à la demande et gardent votre context window focalisée. Ne mettez des skills dans GEMINI.md que si vous voulez qu'ils soient toujours chargés.
2. **Les descriptions de skills comptent** — Chaque `SKILL.md` possède un champ `description` dans son frontmatter qui indique aux agents quand l'activer. Les descriptions de ce repo sont optimisées pour la découverte automatique sur tous les outils supportés (Claude Code, Gemini CLI, etc.) en indiquant clairement à la fois *ce que* fait le skill et *quand* il doit être déclenché.
3. **Utiliser les agents pour la revue** — Copiez le contenu de `agents/code-reviewer.md` lors de la demande de revues de code structurées.
4. **Combiner avec les références** — Référencez les checklists de `references/` lorsque vous travaillez sur des domaines de qualité spécifiques comme les tests ou la performance.

# Utilisation de agent-skills avec Windsurf

Ce guide couvre la configuration et l'utilisation de agent-skills dans l'IDE Windsurf. ([English version](windsurf-setup.md))
## Configuration

### Règles de Projet

Windsurf utilise `.windsurfrules` pour les instructions d'agent spécifiques au projet :

```bash
# Créer un fichier de règles combiné à partir de vos skills les plus importants
cat /path/to/agent-skills/skills/test-driven-development/SKILL.md > .windsurfrules
echo "\n---\n" >> .windsurfrules
cat /path/to/agent-skills/skills/incremental-implementation/SKILL.md >> .windsurfrules
echo "\n---\n" >> .windsurfrules
cat /path/to/agent-skills/skills/code-review-and-quality/SKILL.md >> .windsurfrules
```

### Règles Globales

Pour les skills que vous souhaitez avoir dans tous vos projets, ajoutez-les aux règles globales de Windsurf :

1. Ouvrez Windsurf → Settings → AI → Global Rules
2. Collez le contenu de vos skills les plus utilisés.

## Configuration Recommandée

Gardez `.windsurfrules` focalisé sur 2-3 skills essentiels pour rester dans les limites de context :

```
# .windsurfrules
# Skills essentiels de agent-skills pour ce projet

[Coller test-driven-development SKILL.md]

---

[Coller incremental-implementation SKILL.md]

---

[Coller code-review-and-quality SKILL.md]
```

## Astuces d'Utilisation

1. **Être sélectif** — Le context de Windsurf est limité. Choisissez les skills qui comblent vos plus grandes lacunes en matière de qualité.
2. **Référence dans la conversation** — Collez le contenu d'un skill supplémentaire dans le chat lors de phases spécifiques (ex: collez `security-and-hardening` lors de la construction de l'auth).
3. **Utiliser les références comme checklists** — Collez `references/security-checklist.md` et demandez à Windsurf de vérifier chaque élément.

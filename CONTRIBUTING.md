# Contributing

## Branch Naming Convention

```
<type>/issue-<number>-<short-description>
```

**Types** : `chore`, `docs`, `fix`

**Examples** :
- `chore/issue-1-setup-repo`
- `docs/issue-4-part1-requirements`

## Commit Convention

Format [Conventional Commits](https://www.conventionalcommits.org/) :

```
<type>(<scope>): <short description>
```

**Types** : `chore`, `docs`, `fix`

**Scopes** : `setup`, `part1`, `part2`, `part3`, `part4`, `part5`, `part6`, `part7`, `readme`

**Examples** :
- `chore(setup): initialize repository structure`
- `docs(part1): add functional requirements analysis`
- `docs(part4): add class diagram with Mermaid`

## Pull Request Template

Chaque PR doit :
- Référencer l'issue liée avec `Closes #X`
- Inclure un résumé des changements
- Être relue par au moins un autre membre

## Labels

| Label | Description |
|-------|-------------|
| `type:setup` | Configuration et mise en place |
| `type:docs` | Documentation |
| `type:deliverable` | Livrable final |
| `part:1` à `part:7` | Partie du livrable |
| `priority:high` | Priorité haute |
| `priority:medium` | Priorité moyenne |
| `priority:low` | Priorité basse |
| `claude-code-assisted` | Assisté par Claude Code |

## Milestones

| Milestone | Deadline |
|-----------|----------|
| Setup | 23/03/2026 12:00 |
| Rédaction | 23/03/2026 15:30 |
| Livraison | 23/03/2026 16:30 |

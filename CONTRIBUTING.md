# Contributing — Conventions obligatoires

> **Ces conventions sont IMPÉRATIVES.** Toute PR ne respectant pas ces règles sera refusée.

## Règles de protection de `main`

- **Aucun push direct sur `main`** — toujours passer par une Pull Request
- **1 review minimum** obligatoire avant merge
- **Historique linéaire** obligatoire (squash merge)
- **Force push interdit** sur `main`
- Ces règles s'appliquent à tous, y compris les admins

## Branch Naming Convention (Angular)

Format **obligatoire** :

```text
<type>/issue-<number>-<short-description>
```

**Types autorisés** : `chore`, `docs`, `fix`, `review`

**Règles** :

- **1 branche = 1 issue** — ne jamais mélanger plusieurs issues sur une même branche
- Le numéro d'issue doit correspondre à une issue ouverte
- Description en kebab-case, en anglais ou français
- Créer la branche depuis `main` à jour (`git pull` avant)

**Exemples** :

- `docs/issue-23-styles-architecturaux`
- `docs/issue-28-security-compliance`
- `review/issue-51-review-part1`
- `chore/issue-55-update-readme`

**Interdit** :

- `feature/mon-travail` (pas de numéro d'issue)
- `docs/part2` (trop vague, pas de numéro)
- Branches avec plusieurs issues mélangées

## Commit Convention (Angular / Conventional Commits)

Format **obligatoire** :

```text
<type>(<scope>): <short description in English>
```

**Types** : `chore`, `docs`, `fix`, `review`

**Scopes** : `setup`, `part1`, `part2`, `part3`, `part4`, `part5`, `part6`, `part7`, `readme`

**Règles** :

- Description courte (< 72 caractères), en anglais, commençant par un verbe à l'infinitif
- Corps du commit optionnel pour les détails
- Toujours inclure `Closes #XX` dans le corps ou le message de la PR

**Exemples** :

```text
docs(part2): add comparative analysis of architectural alternatives
docs(part4): design UML class diagram with Mermaid
chore(setup): add branch protection rules
fix(part1): correct hypothesis table formatting
```

**Interdit** :

- `update file` (pas de type, pas de scope)
- `docs: changes` (trop vague)
- `WIP` ou `temp` (jamais de commits temporaires sur `main`)

## Pull Request Convention

**Titre** : même format que les commits — `<type>(<scope>): <description>`

**Body obligatoire** :

```markdown
## Summary

- Point 1
- Point 2

## Checklist

- [ ] Contenu rédigé et structuré
- [ ] Diagrammes Mermaid rendus correctement
- [ ] Relecture effectuée

Closes #XX
```

**Règles** :

- 1 PR = 1 issue (sauf regroupement logique de sous-tâches d'une même partie)
- Toujours lier l'issue avec `Closes #XX`
- Au moins 1 reviewer assigné
- Ne pas merger sa propre PR sans review

## Workflow par issue

```text
1. git checkout main && git pull
2. git checkout -b docs/issue-XX-description
3. [rédiger / modifier le fichier concerné]
4. git add <fichier> && git commit -m "docs(partX): ..."
5. git push -u origin docs/issue-XX-description
6. gh pr create --title "..." --body "..."
7. [attendre la review]
8. [le reviewer merge après approbation]
```

## Labels

| Label | Description |
| ----- | ----------- |
| `type:setup` | Configuration et mise en place |
| `type:docs` | Documentation |
| `type:deliverable` | Livrable final |
| `part:1` à `part:7` | Partie du livrable |
| `priority:high` | Priorité haute |
| `priority:medium` | Priorité moyenne |
| `priority:low` | Priorité basse |
| `claude-code-assisted` | Assisté par Claude Code |

## Milestones

| Milestone | Responsable | Parties | Deadline |
| --------- | ----------- | ------- | -------- |
| Membre 1 — Analyste & Chef de Projet | _À renseigner_ | Parts 1, 6 | 23/03/2026 16:30 |
| Membre 2 — Architecte Système | benoit-bremaud | Parts 2, 7 | 23/03/2026 16:30 |
| Membre 3 — Concepteur Métier | _À renseigner_ | Part 4 | 23/03/2026 16:30 |
| Membre 4 — Tech Lead | _À renseigner_ | Parts 3, 5 | 23/03/2026 16:30 |
| Transversal / Livraison | Tous | Review, ZIP | 23/03/2026 16:30 |

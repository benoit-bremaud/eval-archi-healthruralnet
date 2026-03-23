# Evaluation — Architecture Logicielle : HealthRuralNet

> **Evaluation finale M1** — Architecture Logicielle (Ynov)
> **Sujet D** — Plateforme d'Assistance Médicale Virtuelle pour les Zones Rurales
> **Date** : 23 Mars 2026

---

## Contexte

HealthRuralNet est une plateforme de télémédecine conçue pour les zones rurales, où l'accès aux soins est limité par des infrastructures insuffisantes et des distances importantes. Le projet vise à offrir des consultations médicales à distance, un suivi des patients, une gestion sécurisée des dossiers médicaux et une interconnexion avec les structures de santé locales.

Ce dépôt contient l'ensemble des livrables de l'évaluation : analyse des besoins, proposition d'architecture, design patterns, modèle métier, stack technique, planification d'équipe et stratégie de maintenabilité.

---

## Équipe

| Membre | GitHub | Rôle | Parties assignées |
|--------|--------|------|-------------------|
| ZomboyTepix | [@ZomboyTepix](https://github.com/ZomboyTepix) | Analyste & Chef de Projet | Parties 1, 6 |
| Benoît Bremaud | [@benoit-bremaud](https://github.com/benoit-bremaud) | Architecte Système | Parties 2, 7 |
| David | [@David-Good-Enough](https://github.com/David-Good-Enough) | Concepteur Domaine Métier | Partie 4 |
| Maximilian | [@maximilianpw](https://github.com/maximilianpw) | Tech Lead | Parties 3, 5 |

---

## Livrables

| # | Livrable | Points | Document | Responsable | Statut |
|---|----------|--------|----------|-------------|--------|
| 1 | Analyse et compréhension du recueil de besoins | 4 pts | [`docs/part1-requirements-analysis.md`](docs/part1-requirements-analysis.md) | ZomboyTepix | Done |
| 2 | Proposition et justification de l'architecture | 4 pts | [`docs/part2-architecture-proposal.md`](docs/part2-architecture-proposal.md) | benoit-bremaud | Done |
| 3 | Design Patterns | 3 pts | [`docs/part3-design-patterns.md`](docs/part3-design-patterns.md) | maximilianpw | Done |
| 4 | Conception du modèle métier | 4 pts | [`docs/part4-domain-model.md`](docs/part4-domain-model.md) | David-Good-Enough | Done |
| 5 | Pile technique complète | 2 pts | [`docs/part5-tech-stack.md`](docs/part5-tech-stack.md) | maximilianpw | Done |
| 6 | Planification et composition de l'équipe | 2 pts | [`docs/part6-team-planning.md`](docs/part6-team-planning.md) | ZomboyTepix | Done |
| 7 | Maintenabilité, évolutivité et dette technique | 1 pt | [`docs/part7-maintainability.md`](docs/part7-maintainability.md) | benoit-bremaud | Done |

**Total : 20 points**

---

## Structure du Projet

```text
eval-archi-healthruralnet/
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   └── deliverable.yml
│   └── pull_request_template.md
├── docs/
│   ├── images/
│   ├── part1-requirements-analysis.md
│   ├── part2-architecture-proposal.md
│   ├── part3-design-patterns.md
│   ├── part4-domain-model.md
│   ├── part5-tech-stack.md
│   ├── part6-team-planning.md
│   └── part7-maintainability.md
├── .gitignore
├── CONTRIBUTING.md
├── LICENSE
└── README.md
```

---

## Workflow Git

- **Branches** : une branche dédiée par issue (`docs/issue-X-partY`)
- **Commits** : format Conventional Commits (Angular) — `docs(partX): description`
- **Pull Requests** : liées aux issues avec `Closes #X`, review croisée obligatoire (1 minimum)
- **Protection** : aucun commit direct sur `main`, historique linéaire, force push interdit
- **Kanban** : [GitHub Project](https://github.com/users/benoit-bremaud/projects/42) avec colonnes Backlog → À faire → En cours → En relecture → Terminé

---

## État d'avancement

- [x] Phase 0 — Setup du dépôt
- [x] Phase 1 — Rédaction des parties individuelles
- [x] Phase 2 — Relecture croisée et consolidation
- [ ] Phase 3 — Livraison finale

---

*HealthRuralNet — Evaluation Architecture Logicielle M1 — Mars 2026*

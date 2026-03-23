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

| Membre | Rôle | Parties assignées |
|--------|------|-------------------|
| _Membre 1_ | Analyste & Chef de Projet | Parties 1, 6 |
| _Membre 2_ | Architecte Système | Parties 2, 7 |
| _Membre 3_ | Concepteur Domaine Métier | Partie 4 |
| _Membre 4_ | Tech Lead | Parties 3, 5 |

---

## Livrables

| # | Livrable | Points | Document | Responsable |
|---|----------|--------|----------|-------------|
| 1 | Analyse et compréhension du recueil de besoins | 4 pts | [`docs/part1-requirements-analysis.md`](docs/part1-requirements-analysis.md) | Membre 1 |
| 2 | Proposition et justification de l'architecture | 4 pts | [`docs/part2-architecture-proposal.md`](docs/part2-architecture-proposal.md) | Membre 2 |
| 3 | Design Patterns | 3 pts | [`docs/part3-design-patterns.md`](docs/part3-design-patterns.md) | Membre 4 |
| 4 | Conception du modèle métier | 4 pts | [`docs/part4-domain-model.md`](docs/part4-domain-model.md) | Membre 3 |
| 5 | Pile technique complète | 2 pts | [`docs/part5-tech-stack.md`](docs/part5-tech-stack.md) | Membre 4 |
| 6 | Planification et composition de l'équipe | 2 pts | [`docs/part6-team-planning.md`](docs/part6-team-planning.md) | Membre 1 |
| 7 | Maintenabilité, évolutivité et dette technique | 1 pt | [`docs/part7-maintainability.md`](docs/part7-maintainability.md) | Membre 2 |

**Total : 20 points**

---

## Structure du Projet

```
eval-archi-healthruralnet/
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   └── deliverable.yml
│   └── pull_request_template.md
├── docs/
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
- **Commits** : format Conventional Commits — `docs(partX): description`
- **Pull Requests** : liées aux issues avec `Closes #X`, review croisée avant merge
- **Protection** : aucun commit direct sur `main`

---

## État d'avancement

- [x] Phase 0 — Setup du dépôt
- [ ] Phase 1 — Rédaction des parties individuelles
- [ ] Phase 2 — Relecture croisée et consolidation
- [ ] Phase 3 — Livraison finale

---

*HealthRuralNet — Evaluation Architecture Logicielle M1 — Mars 2026*

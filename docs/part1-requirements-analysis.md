# Partie 1 — Analyse et Compréhension du Recueil de Besoins

> **Responsable** : _Membre 1 — Analyste & Chef de Projet_
> **Points** : 4/20

---

## Table des matières

- [1. Cartographie des Acteurs et Leurs Besoins](#1-cartographie-des-acteurs-et-leurs-besoins)
- [2. Besoins Fonctionnels Détaillés](#2-besoins-fonctionnels-détaillés)
- [3. Exigences Techniques & Contraintes Non-Fonctionnelles](#3-exigences-techniques--contraintes-non-fonctionnelles)
- [4. Analyse des Tensions et Besoins Cachés](#4-analyse-des-tensions-et-besoins-cachés)
- [5. Hypothèses et Arbitrages](#5-hypothèses-et-arbitrages)

---

## 1. Cartographie des Acteurs et Leurs Besoins

L'écosystème HealthRuralNet est multi-acteurs, chacun ayant des exigences diamétralement opposées en matière d'ergonomie et de sécurité.

### 1.1 Acteurs principaux

| Acteur | Besoins Fonctionnels Explicites | Enjeux Métier (Besoins Implicites) |
|--------|--------------------------------|-------------------------------------|
| **Patients** | Prendre RDV, accéder au dossier médical, recevoir des recommandations de suivi. | Interface ultra-simplifiée, surmonter l'illectronisme, support multilingue, compatibilité smartphones anciens. |
| **Praticiens** (Médecins/Infirmiers) | Gérer les consultations (vidéo/audio/chat), accéder aux dossiers, prescrire, collaborer à distance entre confrères. | **Critique : Zéro friction.** L'outil ne doit pas ralentir l'urgence médicale ni surcharger l'administratif. Accès break-glass en urgence. |
| **Aidants familiaux** | Accéder aux dossiers de leurs proches, accompagner dans les démarches numériques. | Gestion fine des délégations d'accès (temporaires, révocables, scope limité) sans compromettre le RGPD. |
| **Administrateurs des structures de santé** | Visibilité globale sur l'activité, gestion des utilisateurs et des structures. | Tableaux de bord sans accès aux données de santé identifiantes. Séparation stricte admin/médical. |

### 1.2 Acteurs secondaires et parties prenantes

| Acteur | Rôle dans l'écosystème | Besoins spécifiques |
|--------|------------------------|---------------------|
| **Structures de santé** (hôpitaux, cliniques, dispensaires mobiles) | Fournissent les praticiens et les infrastructures locales | Interopérabilité avec leurs SI existants (HL7 v2, FHIR), respect de leur autonomie sur les données |
| **Institutions et décideurs politiques** | Encadrent les aspects réglementaires, financent le projet | Rapports d'impact (médical, financier, ergonomique), conformité réglementaire, audits |
| **Associations locales** | Médiation avec les populations rurales, accompagnement à l'adoption | Outils de formation simples, interfaces adaptées aux usages locaux |
| **Partenaires techniques** | Éditeurs de SI hospitaliers, opérateurs télécom | API standardisées, documentation technique accessible, SLA clairs |
| **Mutuelles et assurances santé** | Potentiels partenaires pour le modèle économique | Accès aux données agrégées anonymisées pour le remboursement des téléconsultations |

---

## 2. Besoins Fonctionnels Détaillés

### 2.1 Besoins fonctionnels explicites (priorisation MoSCoW)

| # | Besoin fonctionnel | Priorité | Acteur principal | Description |
|---|-------------------|----------|------------------|-------------|
| BF1 | Téléconsultation | **Must** | Praticiens, Patients | Consultations vidéo, audio et chat en temps réel. Support des connexions bas débit. |
| BF2 | Dossier médical électronique | **Must** | Praticiens, Patients | Accès centralisé aux antécédents, allergies, traitements, historique complet. Stockage MongoDB pour documents non structurés. |
| BF3 | Prise de rendez-vous | **Must** | Patients, Praticiens | Planification des consultations avec gestion des disponibilités et rappels automatiques. |
| BF4 | Prescription médicale | **Must** | Praticiens | Création, validation et transmission des ordonnances. Vérification des interactions médicamenteuses. |
| BF5 | Suivi des patients chroniques | **Must** | Praticiens, Patients | Alertes de suivi, rappels de traitements, historique des mesures. |
| BF6 | Interconnexion SI hospitaliers | **Must** | Structures de santé | Intégration bidirectionnelle avec les SI existants via HL7 v2, FHIR et GraphQL. |
| BF7 | Notifications et alertes médicales | **Should** | Praticiens, Patients | Push, SMS, email, canal vocal pour zones sans data. Alertes d'urgence avec validation humaine. |
| BF8 | Gestion des consentements RGPD | **Must** | Patients | Consentement explicite, droit à l'oubli, portabilité des données, délégation aidant. |
| BF9 | Interface multilingue | **Should** | Patients | Traduction de l'interface en langues locales pour dépasser les barrières linguistiques. |
| BF10 | Mode offline | **Must** | Praticiens | Fonctionnement complet sans connexion avec synchronisation différée à la reconnexion. |
| BF11 | Collaboration entre praticiens | **Should** | Praticiens | Partage de cas complexes, avis spécialisé à distance, annotations partagées sur dossiers. |
| BF12 | Dépistage précoce et épidémiologie | **Could** | Institutions | Agrégation anonymisée des données pour détecter des tendances épidémiologiques. |

### 2.2 Besoins implicites et cachés

| # | Besoin caché | Déduit de | Impact architectural |
|---|-------------|-----------|---------------------|
| BC1 | Mode de fonctionnement allégé pour faible débit | Contexte zones rurales, réseaux mobiles instables | Architecture Event-Driven, Strategy Pattern pour adapter le comportement réseau |
| BC2 | Canal SMS/vocal pour patients sans smartphone | Population rurale, illectronisme | Notification Service multi-canal |
| BC3 | Compatibilité avec les dossiers papier numérisés | SI hospitaliers vieillissants, fax | Interoperability Service avec adaptateurs PDF/OCR |
| BC4 | Accès break-glass en urgence | Praticiens en situation critique | Auth Service avec tokens temporaires à audit renforcé |
| BC5 | Réconciliation des données post-offline | Médecins travaillant offline simultanément sur le même dossier | Sync Service avec détection de conflits et alerte humaine |
| BC6 | Séparation admin/médical stricte | RGPD, confidentialité patient | RBAC contextuel avec périmètres étanches |

---

## 3. Exigences Techniques & Contraintes Non-Fonctionnelles

Les contraintes environnementales et réglementaires de HealthRuralNet dictent des choix architecturaux rigoureux.

### 3.1 Tableau des exigences non fonctionnelles

| Catégorie | Exigence | Métrique cible | Priorité | Justification |
|-----------|----------|---------------|----------|---------------|
| **Performance** | Temps de réponse API | < 500ms (P95) en mode online | Must | Les praticiens ne tolèrent pas la latence lors d'une consultation |
| **Performance** | Temps de synchronisation offline | < 30s pour un delta sync, < 5min pour un full sync | Must | La reconnexion doit être transparente pour le praticien |
| **Disponibilité** | Uptime des services core | > 99.5% (hors mode offline) | Must | Plateforme médicale — l'indisponibilité peut impacter des vies |
| **Sécurité** | Chiffrement des données | E2E en transit (TLS 1.3), au repos (AES-256) | Must | RGPD art. 32, HIPAA, données médicales sensibles |
| **Sécurité** | Authentification | MFA obligatoire, break-glass pour urgences | Must | Compromis sécurité/fluidité clinique |
| **Scalabilité** | Montée en charge | Support de 1000 consultations simultanées par région | Should | Croissance progressive, scalabilité horizontale par service |
| **Accessibilité** | Interface utilisateur | WCAG 2.1 AA, interfaces simplifiées, support multilingue | Must | Patients peu technophiles, barrières linguistiques |
| **Résilience** | Mode offline | Fonctionnement complet sans connexion | Must | Connectivité intermittente en zones rurales |
| **Interopérabilité** | Standards médicaux | HL7 v2, FHIR R4, GraphQL | Must | Écosystème hospitalier fragmenté |
| **Conformité** | Stockage souverain | Données hébergées dans le pays du patient | Must | RGPD, restrictions export données médicales |

### 3.2 Résilience et Connectivité

L'infrastructure doit pallier l'instabilité des réseaux dans les zones rurales en intégrant nativement un **mode déconnecté**. Cette exigence impose une approche de conception orientée vers le fonctionnement hors ligne, capable de stocker localement les données avant une synchronisation différée.

Pour garantir l'intégrité des dossiers médicaux lors de mises à jour concurrentes, le système devra s'appuyer sur des **mécanismes de résolution de conflits robustes**. Après évaluation, l'approche retenue est **Last-Write-Wins (LWW) avec détection de conflits et alerte humaine** — les CRDT, bien que théoriquement élégants, présentent une complexité disproportionnée pour des données médicales sensibles comme les prescriptions (cf. Part 2, §6.3).

### 3.3 Interopérabilité

La plateforme doit s'intégrer à un écosystème hospitalier extrêmement fragmenté. L'architecture devra dialoguer avec des standards variés :

- Flux modernes : **FHIR**, **GraphQL**
- Protocoles vieillissants : **HL7 v2**, voire la gestion de fichiers PDF

Cela nécessite la conception d'une solide **couche d'abstraction**. La mise en place de modèles d'isolation — notamment des **couches anti-corruption** (Adapter Pattern, cf. Part 3) — permettra de normaliser les échanges entrants et sortants pour ne pas polluer le cœur métier du système.

### 3.4 Sécurité et Conformité

- Le strict respect du **RGPD** et des cadres légaux locaux est impératif.
- Le **chiffrement de bout en bout** des échanges est une condition non négociable (TLS 1.3 en transit, AES-256 au repos).
- L'architecture d'hébergement doit être **agnostique** afin de supporter des déploiements sur des clouds souverains ou des infrastructures locales selon les obligations régionales.
- Concernant la gestion des identités, un modèle **RBAC contextuel** offre le meilleur compromis entre simplicité d'administration (adoption par les praticiens) et flexibilité (délégation aidant, accès break-glass). Un ABAC pur serait trop complexe à administrer dans le contexte multi-acteurs décrit dans le sujet (cf. Part 2, §7.3).

### 3.5 Maintenabilité et Évolutivité

Le système actuel souffre d'un empilement chaotique de briques technologiques. L'enjeu est de restructurer cette **dette technique** sans pour autant effacer les développements passés. L'adoption d'une stratégie d'**encapsulation progressive de l'existant** (Strangler Fig Pattern) permettra de migrer les fonctionnalités de manière itérative vers des services autonomes et modulaires, garantissant la pérennité de la solution globale.

---

## 4. Analyse des Tensions et Besoins Cachés (Trade-offs)

L'analyse du contexte révèle que l'architecture technique est directement impactée par des problèmes organisationnels et d'acceptabilité.

### Délégation vs Confidentialité

**Tension** : Il faut intégrer les aidants familiaux dans le parcours de soin sans risquer de compromettre la sécurité globale des dossiers.

> **Besoin caché** : Un modèle de contrôle d'accès **RBAC contextuel** avec délégation explicite — le patient autorise un proche via consentement signé numériquement, révocable à tout moment, limité en scope (lecture seule) et audité dans l'Event Store (cf. Part 2, §7.3).

### Sécurité vs Fluidité Clinique

**Tension** : Les professionnels redoutent que la plateforme ne devienne une charge mentale supplémentaire ou ne ralentisse leurs consultations.

> **Besoin caché** : Une **automatisation "silencieuse"** des tâches de synchronisation en arrière-plan et des interfaces sans friction pour garantir l'adoption par les soignants.

### Urgence vitale vs Authentification stricte

**Tension** : Un médecin ne peut pas perdre de temps avec une authentification multifacteurs ou biométrique lorsqu'il a besoin d'un accès immédiat au dossier d'un patient en situation critique.

> **Besoin caché** : L'implémentation d'un pattern **"Break-Glass"** (accès d'urgence) permettant de contourner les barrières de sécurité immédiates, compensé par une traçabilité d'audit forte et inaltérable a posteriori (cf. Part 2, §7.2).

### Standardisation globale vs Adaptabilité locale

**Tension** : Le système doit trouver un équilibre délicat entre un socle commun robuste et une adaptabilité indispensable pour ne pas ignorer les spécificités des structures rurales.

> **Besoin caché** : Une conception modulaire (**Clean Architecture**) séparant strictement le noyau dur clinique standardisé des connecteurs périphériques personnalisables selon les SI locaux (cf. Part 2, §2.3).

---

## 5. Hypothèses et Arbitrages

Pour concevoir une architecture pragmatique et viable, nous devons arbitrer la "cacophonie" interne par les hypothèses suivantes :

### H1 — Cadrage Fonctionnel (Élagage)

Nous écartons du MVP initial :

- La **Blockchain** — en raison de ses problèmes de latence et de scalabilité avérés. L'Event Store avec journal immuable offre la même garantie d'inaltérabilité (cf. Part 2, §8.4).
- L'**IA diagnostique** — face aux risques légaux de responsabilité non résolus. Reportée à une phase ultérieure avec cadre éthique validé.
- L'intégration des flux non structurés issus des **wearables** — données non normalisées, capteurs peu fiables. Nécessite un standard IoT médical préalable.

L'architecture se concentrera strictement sur le cœur de métier : **Dossier patient, téléconsultation et interopérabilité**.

### H2 — Stratégie Front-End

Face au débat opposant application native et PWA, nous privilégierons une **PWA (Progressive Web App)** couplée à un stockage local. Cela permet de répondre parfaitement à l'exigence d'accessibilité multi-appareils tout en minimisant la consommation des ressources réseau.

### H3 — Protocole de Réplication (Offline)

Pour gérer la synchronisation différée des données sensibles, l'approche **Last-Write-Wins (LWW) avec détection de conflits et alerte humaine** est retenue. Les CRDT ont été évalués mais écartés : leur complexité est disproportionnée pour des données médicales sensibles comme les prescriptions, et le sujet lui-même note que "personne ne sait comment cela se comporterait" dans ce contexte. Le LWW offre prévisibilité et simplicité, compensé par un audit trail complet dans l'Event Store (cf. Part 2, §6.3).

### H4 — Sécurité d'accès

Nous opterons pour un modèle de permissions **RBAC contextuel** (Role-Based Access Control enrichi de règles contextuelles) plutôt qu'un ABAC pur. L'ABAC offre une finesse théorique supérieure, mais sa complexité d'administration est un frein à l'adoption dans un contexte où les praticiens sont déjà réticents aux outils numériques. Le RBAC contextuel permet la délégation aidant et le break-glass via des surcharges de rôle ciblées (cf. Part 2, §7.3).

### H5 — Infrastructure terrain

Nous faisons l'hypothèse que :
- Les praticiens disposent au minimum d'un smartphone avec navigateur web
- Les zones rurales ciblées ont au moins une couverture 2G/3G intermittente
- Le matériel médical de base (tensiomètre, thermomètre) est fourni par les structures locales
- Les budgets institutionnels permettent un hébergement cloud souverain par région

---

*HealthRuralNet — Evaluation Architecture Logicielle M1 — Mars 2026*

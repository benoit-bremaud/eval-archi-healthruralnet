# Partie 1 — Analyse et Compréhension du Recueil de Besoins

> **Responsable** : _Membre 1 — Analyste & Chef de Projet_
> **Points** : 4/20

---

## Table des matières

- [1. Cartographie des Acteurs et Leurs Besoins](#1-cartographie-des-acteurs-et-leurs-besoins-besoins-fonctionnels)
- [2. Exigences Techniques & Contraintes Non-Fonctionnelles](#2-exigences-techniques--contraintes-non-fonctionnelles-nfrs)
- [3. Analyse des Tensions et Besoins Cachés](#3-analyse-des-tensions-et-besoins-cachés-trade-offs)
- [4. Hypothèses et Arbitrages](#4-hypothèses-et-arbitrages)

---

## 1. Cartographie des Acteurs et Leurs Besoins (Besoins Fonctionnels)

L'écosystème HealthRuralNet est multi-acteurs, chacun ayant des exigences diamétralement opposées en matière d'ergonomie et de sécurité.

| Acteur | Besoins Fonctionnels Explicites | Enjeux Métier (Besoins Implicites) |
|--------|--------------------------------|-------------------------------------|
| **Patients** | Prendre RDV, accéder au dossier médical, recevoir des recommandations. | Interface ultra-simplifiée, surmonter l'illectronisme, support multilingue. |
| **Praticiens** (Médecins/Infirmiers) | Gérer les consultations, accéder aux dossiers, interagir avec des confrères. | **Critique : Zéro friction.** L'outil ne doit pas ralentir l'urgence médicale ni surcharger l'administratif. |
| **Aidants familiaux** | Accéder aux dossiers de leurs proches. | Gestion fine des délégations d'accès sans compromettre le RGPD. |
| **Administrateurs** | Visibilité globale sur le système. | Tableaux de bord sans accès aux données de santé identifiantes. |

---

## 2. Exigences Techniques & Contraintes Non-Fonctionnelles (NFRs)

Les contraintes environnementales et réglementaires de HealthRuralNet dictent des choix architecturaux rigoureux. L'analyse de ces exigences non fonctionnelles met en évidence les défis techniques majeurs à relever.

### 🔌 Résilience et Connectivité

L'infrastructure doit pallier l'instabilité des réseaux dans les zones rurales en intégrant nativement un **mode déconnecté**. Cette exigence impose une approche de conception orientée vers le fonctionnement hors ligne, capable de stocker localement les données avant une synchronisation différée.

Pour garantir l'intégrité des dossiers médicaux lors de mises à jour concurrentes, le système devra s'appuyer sur des **mécanismes de résolution de conflits robustes**. À ce titre, l'utilisation de structures de données répliquées sans conflit (**CRDT**) apparaît comme la réponse technique la plus adaptée pour assurer une convergence fiable des informations sensibles.

### 🔗 Interopérabilité

La plateforme doit s'intégrer à un écosystème hospitalier extrêmement fragmenté. L'architecture devra dialoguer avec des standards variés :

- Flux modernes : **FHIR**, **GraphQL**
- Protocoles vieillissants : **HL7 v2**, voire la gestion de fichiers PDF

Cela nécessite la conception d'une solide **couche d'abstraction**. La mise en place de modèles d'isolation — notamment des **couches anti-corruption** — permettra de normaliser les échanges entrants et sortants pour ne pas polluer le cœur métier du système.

### 🔒 Sécurité et Conformité

- Le strict respect du **RGPD** et des cadres légaux locaux est impératif.
- Le **chiffrement de bout en bout** des échanges est une condition non négociable.
- L'architecture d'hébergement doit être **agnostique** afin de supporter des déploiements sur des clouds souverains ou des infrastructures locales selon les obligations régionales.
- Concernant la gestion des identités, le contrôle d'accès doit être dynamique : un modèle basé sur les attributs (**ABAC**) offrira la finesse nécessaire pour concilier la protection des données avec les exigences d'un accès rapide en situation d'urgence.

### 🔧 Maintenabilité et Évolutivité

Le système actuel souffre d'un empilement chaotique de briques technologiques. L'enjeu est de restructurer cette **dette technique** sans pour autant effacer les développements passés. L'adoption d'une stratégie d'**encapsulation progressive de l'existant** permettra de migrer les fonctionnalités de manière itérative vers des services autonomes et modulaires, garantissant la pérennité de la solution globale.

---

## 3. Analyse des Tensions et Besoins Cachés (Trade-offs)

L'analyse du contexte révèle que l'architecture technique est directement impactée par des problèmes organisationnels et d'acceptabilité.

### ⚖️ Délégation vs Confidentialité

**Tension** : Il faut intégrer les aidants familiaux dans le parcours de soin sans risquer de compromettre la sécurité globale des dossiers.

> **Besoin caché** : Un modèle de contrôle d'accès dynamique de type **ABAC** (Attribute-Based Access Control) pour générer des permissions temporaires et ciblées, remplaçant les modèles de rôles classiques trop rigides.

### ⚖️ Sécurité vs Fluidité Clinique

**Tension** : Les professionnels redoutent que la plateforme ne devienne une charge mentale supplémentaire ou ne ralentisse leurs consultations.

> **Besoin caché** : Une **automatisation "silencieuse"** des tâches de synchronisation en arrière-plan et des interfaces sans friction pour garantir l'adoption par les soignants.

### ⚖️ Urgence vitale vs Authentification stricte

**Tension** : Un médecin ne peut pas perdre de temps avec une authentification multifacteurs ou biométrique lorsqu'il a besoin d'un accès immédiat au dossier d'un patient en situation critique.

> **Besoin caché** : L'implémentation d'un pattern **"Break-Glass"** (accès d'urgence) permettant de contourner les barrières de sécurité immédiates, compensé par une traçabilité d'audit forte et inaltérable a posteriori.

### ⚖️ Standardisation globale vs Adaptabilité locale

**Tension** : Le système doit trouver un équilibre délicat entre un socle commun robuste et une adaptabilité indispensable pour ne pas ignorer les spécificités des structures rurales.

> **Besoin caché** : Une conception modulaire (ex: **Clean Architecture**) séparant strictement le noyau dur clinique standardisé des connecteurs périphériques personnalisables selon les SI locaux.

---

## 4. Hypothèses et Arbitrages

Pour concevoir une architecture pragmatique et viable, nous devons arbitrer la "cacophonie" interne par les hypothèses suivantes :

### H1 — Cadrage Fonctionnel (Élagage)

Nous écartons de l'architecture initiale :

- ❌ La **Blockchain** — en raison de ses problèmes de latence et de scalabilité avérés
- ❌ L'**IA diagnostique** — face aux risques légaux de responsabilité non résolus
- ❌ L'intégration des flux non structurés issus des **wearables**

L'architecture se concentrera strictement sur le cœur de métier : **Dossier patient, téléconsultation et interopérabilité**.

### H2 — Stratégie Front-End

Face au débat opposant application native et PWA, nous privilégierons une **PWA (Progressive Web App)** couplée à un stockage local. Cela permet de répondre parfaitement à l'exigence d'accessibilité multi-appareils tout en minimisant la consommation des ressources réseau.

### H3 — Protocole de Réplication (Offline)

Pour gérer la synchronisation différée des données sensibles sans générer de conflits destructeurs, l'approche **CRDT** (Conflict-free Replicated Data Type) sera retenue par rapport à MQTT. Ce choix garantit la convergence mathématique des dossiers de santé lors des reconnexions.

### H4 — Sécurité d'accès

Nous opterons pour un modèle de permissions **ABAC** (Attribute-Based Access Control) au lieu d'un RBAC strict. C'est le seul modèle suffisamment dynamique pour gérer le contexte spécifique d'une urgence médicale ou la délégation temporaire des droits aux aidants familiaux.

---

*HealthRuralNet — Evaluation Architecture Logicielle M1 — Mars 2026*

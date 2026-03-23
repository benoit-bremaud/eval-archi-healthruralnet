# Partie 4 — Conception du Modèle Métier

> **Responsable** : _Membre 3 — Concepteur Domaine Métier_
> **Points** : 4/20

---

## Table des matières

- [1. Identification des entités métier](#1-identification-des-entités-métier)
- [2. Diagramme de classes UML](#2-diagramme-de-classes-uml)
- [3. Justification des concepts POO](#3-justification-des-concepts-poo)
- [4. Description détaillée des classes](#4-description-détaillée-des-classes)
- [5. Relations et cardinalités](#5-relations-et-cardinalités)

---

## 1. Identification des entités métier

<!-- Liste des entités principales du domaine HealthRuralNet -->

| Entité | Description | Attributs clés |
|--------|-------------|----------------|
| Utilisateur | Classe abstraite parente | id, nom, email, dateCreation |
| Patient | Utilisateur bénéficiaire de soins | numSecu, dateNaissance, adresse |
| Medecin | Professionnel de santé | specialite, numOrdre, structureSante |
| Infirmier | Personnel soignant | service, structureSante |
| DossierMedical | Dossier patient sécurisé | antecedents, allergies, traitements |
| Consultation | Séance de télémédecine | date, type, statut, notes |
| Prescription | Ordonnance médicale | medicaments, posologie, duree |
| StructureSante | Hôpital, clinique, dispensaire | nom, type, localisation |
| RendezVous | Planification de consultation | date, heure, medecin, patient |

## 2. Diagramme de classes UML

```mermaid
classDiagram
    class Utilisateur {
        <<abstract>>
        #id: UUID
        #nom: String
        #prenom: String
        #email: String
        #telephone: String
        #dateCreation: Date
        +seConnecter() bool
        +mettreAJourProfil() void
    }

    class Patient {
        -numSecu: String
        -dateNaissance: Date
        -adresse: Adresse
        -aidantFamilial: AidantFamilial
        +prendreRendezVous() RendezVous
        +consulterDossier() DossierMedical
    }

    class Medecin {
        -specialite: String
        -numOrdre: String
        +creerConsultation() Consultation
        +redigerPrescription() Prescription
        +accederDossier(patient) DossierMedical
    }

    class Infirmier {
        -service: String
        +suivrePatient(patient) void
        +ajouterObservation(dossier) void
    }

    class DossierMedical {
        -id: UUID
        -antecedents: List~String~
        -allergies: List~String~
        -traitements: List~Traitement~
        -dateModification: Date
        +ajouterEntree(entree) void
        +getHistorique() List~EntreeMedicale~
    }

    class Consultation {
        -id: UUID
        -date: DateTime
        -type: TypeConsultation
        -statut: StatutConsultation
        -notes: String
        -diagnostic: String
        +demarrer() void
        +terminer() void
        +facturer()* Facture
    }

    class Prescription {
        -id: UUID
        -medicaments: List~Medicament~
        -posologie: String
        -duree: int
        -dateEmission: Date
        +valider() void
        +annuler() void
    }

    class StructureSante {
        -id: UUID
        -nom: String
        -type: TypeStructure
        -localisation: Coordonnees
        +listerMedecins() List~Medecin~
    }

    class RendezVous {
        -id: UUID
        -dateHeure: DateTime
        -statut: StatutRDV
        +confirmer() void
        +annuler() void
        +reporter(nouvelleDate) void
    }

    %% TODO: Compléter les relations et ajouter les classes manquantes

    Utilisateur <|-- Patient
    Utilisateur <|-- Medecin
    Utilisateur <|-- Infirmier
    Patient "1" --> "1" DossierMedical : possède
    Patient "1" --> "*" RendezVous : prend
    Medecin "1" --> "*" Consultation : réalise
    Medecin "1" --> "*" Prescription : rédige
    Consultation "1" --> "0..1" Prescription : génère
    Consultation "*" --> "1" Patient : concerne
    Consultation "*" --> "1" Medecin : implique
    RendezVous "*" --> "1" Medecin : avec
    Medecin "*" --> "1" StructureSante : exerce dans
    Infirmier "*" --> "1" StructureSante : rattaché à
```

## 3. Justification des concepts POO

### 3.1 Héritage

<!-- Utilisateur → Patient, Medecin, Infirmier : justification du choix d'héritage -->

### 3.2 Encapsulation

<!-- Protection des attributs sensibles de DossierMedical (antécédents, allergies) — attributs privés, accès contrôlé -->

### 3.3 Polymorphisme

<!-- Méthode facturer() sur Consultation : comportement différent selon TypeConsultation (urgence, suivi, spécialiste) -->

## 4. Description détaillée des classes

<!-- Description de chaque classe avec ses responsabilités, invariants métier, règles de gestion -->

## 5. Relations et cardinalités

<!-- Justification des associations, compositions et agrégations choisies -->

---

*HealthRuralNet — Evaluation Architecture Logicielle M1 — Mars 2026*

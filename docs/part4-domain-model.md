## Diagramme de classes UML  
  
  ![[Healthcare User Management.png]]
  
  
Le modèle métier de **HealthRuralNet** a été construit à partir des besoins principaux identifiés dans l’analyse fonctionnelle : permettre l’accès aux soins à distance, organiser les interactions entre patients et professionnels de santé, sécuriser les données médicales, assurer un suivi dans le temps et encadrer l’accès aux informations sensibles. Le choix a été fait de proposer un diagramme **suffisamment riche pour représenter les objets métier essentiels**, tout en restant **lisible et compréhensible** dans le cadre de l’épreuve. L’objectif n’était donc pas de modéliser tous les détails techniques possibles, mais de construire un **socle métier cohérent, évolutif et maintenable**.  
  
### 1. Une modélisation centrée sur les acteurs du système  
  
Le diagramme est d’abord structuré autour de la classe abstraite **`Utilisateur`**. Ce choix est justifié par le fait que plusieurs acteurs de la plateforme partagent un ensemble de caractéristiques communes : un identifiant, un nom, un prénom, un email, un téléphone et un mécanisme d’authentification. Plutôt que de répéter ces attributs dans plusieurs classes séparées, ils ont été factorisés dans une classe mère.  
  
Cette décision répond directement à une bonne pratique de conception orientée objet : **éviter la duplication**. En centralisant les données communes dans `Utilisateur`, le modèle devient plus simple à maintenir. Si demain un nouvel attribut commun doit être ajouté, comme une langue préférée, un statut de compte ou une date de dernière connexion, il suffira de le faire à un seul endroit. Ce choix renforce donc la **maintenabilité** et l’**évolutivité** du système.  
  
La classe `Utilisateur` est déclarée **abstraite**, car elle ne correspond pas à un acteur concret manipulé directement dans le métier. On ne crée pas “un utilisateur générique” dans le domaine médical ; on crée un patient, un médecin, un infirmier ou un administrateur. Cette abstraction permet de traduire correctement la réalité métier tout en posant une base commune solide.  
  
### 2. L’héritage pour spécialiser les rôles métier  
  
À partir de `Utilisateur`, plusieurs spécialisations sont proposées : **`Patient`**, **`ProfessionnelSante`** et **`Administrateur`**. Le choix de ces classes vient du besoin de représenter les profils principaux explicitement mentionnés dans le sujet. Le patient est au cœur de la prise en charge, le professionnel de santé intervient dans les consultations et le suivi, et l’administrateur gère le fonctionnement global de la plateforme.  
  
La classe **`ProfessionnelSante`** a elle aussi été pensée comme une classe abstraite. Ce choix est important, car tous les soignants n’ont pas exactement les mêmes responsabilités. Un médecin peut diagnostiquer et prescrire, tandis qu’un infirmier assure davantage le suivi, les constantes ou l’accompagnement médical. Il était donc pertinent de créer une classe intermédiaire commune aux soignants, puis de la spécialiser en **`Medecin`** et **`Infirmier`**.  
  
Ce mécanisme d’**héritage** permet de représenter clairement la hiérarchie métier :  
- `Utilisateur` contient ce qui est commun à tous les acteurs ;  
- `ProfessionnelSante` regroupe ce qui est commun aux soignants ;  
- `Medecin` et `Infirmier` ajoutent ensuite leurs comportements propres.  
  
Ce choix est justifié car il améliore la **modularité** du modèle. Chaque classe a une responsabilité claire et le système peut être étendu plus tard avec d’autres types de professionnels, comme un pharmacien, un spécialiste ou un coordinateur médical, sans devoir remettre en cause la structure existante.  
  
### 3. Le patient comme entité centrale du domaine  
  
Le diagramme place volontairement **`Patient`** au centre de plusieurs relations métier. Ce choix est directement lié à la finalité du projet : HealthRuralNet est une plateforme destinée à améliorer l’accès aux soins et le suivi médical des populations rurales. Le patient devient donc naturellement l’entité pivot autour de laquelle s’organisent les autres objets métier.  
  
Le patient possède un **`DossierMedical`**, prend des **`RendezVous`**, participe à des **`Consultation`**, bénéficie d’un **`SuiviMedical`** et gère des **`Consentement`**. Cette structuration a été retenue pour représenter le cycle global de la prise en charge :  
- prise de contact avec le système,  
- organisation de l’échange médical,  
- production d’informations cliniques,  
- suivi dans le temps,  
- contrôle de l’accès aux données.  
  
Le choix d’associer un **`DossierMedical`** unique au patient est particulièrement justifié. Dans un système de santé, le dossier médical joue le rôle de mémoire clinique. Il centralise les antécédents, traitements, allergies et autres éléments nécessaires à une prise de décision médicale. Le représenter comme une classe distincte permet de bien séparer l’identité de la personne et ses données médicales. Cette distinction est importante, car elle rend le modèle plus clair et plus facilement maintenable.  
  
### 4. Le dossier médical comme objet métier protégé  
  
Le **`DossierMedical`** est une classe centrale du diagramme, mais aussi l’une des plus sensibles. Il a donc été conçu avec une attention particulière. Les données qu’il contient sont privées et ne doivent pas être manipulées librement. C’est pour cette raison que les attributs sont notés avec le symbole `-` dans le diagramme : cela traduit l’**encapsulation**.  
  
L’encapsulation est particulièrement justifiée dans un projet de télémédecine, car les informations médicales ne doivent jamais être exposées directement. Le dossier contient des informations sensibles, comme les antécédents, traitements ou allergies, qui doivent être consultées et modifiées selon des règles très strictes. Le modèle prévoit donc des méthodes dédiées comme `ajouterAntecedent()`, `ajouterTraitement()`, `obtenirResume()` ou `verifierAutorisationAcces()`.  
  
Ce choix présente plusieurs avantages :  
- il **protège l’intégrité** des données ;  
- il impose un **point de passage unique** pour appliquer les règles métier ;  
- il facilite les évolutions futures, car la logique d’accès peut être modifiée sans casser le reste du système.  
  
En d’autres termes, l’encapsulation n’est pas utilisée ici seulement pour respecter la théorie orientée objet, mais parce qu’elle répond à une contrainte réelle du domaine : la **confidentialité des données de santé**.  
  
### 5. La classe `Confidentialite` pour séparer les règles d’accès  
  
Le choix d’introduire une classe **`Confidentialite`** associée au `DossierMedical` est également justifié par le contexte du projet. Le sujet insiste fortement sur la sécurité, le respect des accès et les contraintes réglementaires. Il était donc pertinent de ne pas mélanger directement les règles de confidentialité avec les autres données médicales.  
  
En séparant `Confidentialite` du `DossierMedical`, le modèle gagne en **clarté** et en **modularité**. Le dossier médical conserve son rôle de conteneur des données cliniques, tandis que la confidentialité devient un objet spécialisé chargé de représenter les règles de partage et d’autorisation. Ce découpage respecte le principe de responsabilité unique : chaque classe a un rôle précis.  
  
Ce choix prépare également le système à évoluer. Si les règles d’accès deviennent plus complexes dans une future version, il sera plus simple d’enrichir la classe `Confidentialite` que de modifier en profondeur le `DossierMedical`.  
  
### 6. Les identifiants sécurisés comme objet séparé  
  
La classe **`IdentifiantsSecurises`** a été modélisée séparément de `Utilisateur`. Ce choix est justifié par un souci de séparation des responsabilités. Les informations liées à l’authentification, comme le mot de passe haché ou le secret MFA, ne relèvent pas du même niveau métier que l’identité générale de l’utilisateur.  
  
Les isoler dans une classe dédiée apporte plusieurs bénéfices :  
- meilleure lisibilité du modèle ;  
- meilleure protection des données sensibles ;  
- possibilité d’adapter les mécanismes d’authentification sans impacter la classe `Utilisateur`.  
  
Ce choix renforce donc la **sécurité** et la **maintenabilité**. Il est également cohérent avec de bonnes pratiques de conception, car il évite de transformer la classe `Utilisateur` en objet trop volumineux et trop chargé.  
  
### 7. Le rendez-vous comme étape d’organisation distincte de la consultation  
  
Le modèle distingue volontairement **`RendezVous`** et **`Consultation`**. Ce choix est important, car ces deux notions ne représentent pas la même réalité métier.  
  
Le `RendezVous` correspond à la **planification** de l’échange médical : date, heure, motif, statut, éventuelle annulation ou reprogrammation. La `Consultation`, quant à elle, correspond à l’**acte médical réalisé** : déroulement, compte-rendu, type de consultation, éventuelle prescription.  
  
Cette séparation est justifiée car, dans la réalité, un rendez-vous peut être annulé, déplacé ou ne jamais aboutir à une consultation effective. À l’inverse, une consultation ne devrait exister que si un acte médical a réellement eu lieu. Mélanger ces deux notions dans une seule classe aurait rendu le modèle plus confus. En les séparant, on améliore la **cohérence métier** et la **clarté fonctionnelle**.  
  
### 8. Le polymorphisme avec la hiérarchie des consultations  
  
La classe **`Consultation`** a été définie comme **abstraite**, puis spécialisée en **`Teleconsultation`**, **`ConsultationPresentielle`** et **`ConsultationUrgence`**. Ce choix permet de représenter les différentes modalités de prise en charge évoquées ou implicites dans le sujet.  
  
Toutes les consultations partagent des éléments communs : une date, un motif, un compte-rendu et un cycle de vie. Il était donc logique de factoriser ces informations dans une classe mère. En revanche, certaines caractéristiques varient selon le type :  
- la téléconsultation nécessite un lien visio ;  
- la consultation présentielle peut être liée à un lieu ou une salle ;  
- la consultation d’urgence implique un niveau de priorité spécifique.  
  
Le **polymorphisme** est mis en avant à travers la méthode `facturer()`. Cette méthode est déclarée dans la classe `Consultation`, puis spécialisée dans chaque sous-classe. Ce choix est justifié parce que le calcul ou la logique de facturation peut varier selon la nature de l’acte. Une téléconsultation, une consultation physique ou une urgence ne répondent pas forcément aux mêmes règles tarifaires ou administratives.  
  
Ce mécanisme permet de rendre le modèle plus **souple** et plus **évolutif**. Si un nouveau type de consultation apparaît plus tard, comme une visite mobile ou une téléexpertise, il suffira d’ajouter une nouvelle sous-classe avec son propre comportement. On évite ainsi des traitements conditionnels lourds et dispersés dans le code.  
  
### 9. La prescription comme prolongement direct de l’acte médical  
  
La classe **`Prescription`** a été reliée à la `Consultation` et au `Medecin`. Ce choix traduit une logique métier simple : une prescription n’apparaît pas de manière isolée, elle est généralement produite à la suite d’une consultation, et elle relève du rôle du médecin.  
  
Ce lien permet de conserver une bonne **traçabilité métier**. On sait à quelle consultation correspond la prescription, et quel professionnel l’a émise. C’est un point important pour la cohérence du domaine et pour les exigences futures de suivi ou d’audit.  
  
Le choix de modéliser `Prescription` comme une classe dédiée, plutôt que comme un simple attribut texte de la consultation, est également justifié. Une prescription est un objet métier à part entière, qui peut posséder sa propre date, ses propres instructions, son propre statut et potentiellement évoluer dans de futures versions du système.  
  
### 10. Le suivi médical pour représenter la continuité des soins  
  
Le sujet ne se limite pas à la consultation ponctuelle ; il insiste aussi sur le **suivi des patients**, notamment dans un contexte rural où certaines pathologies nécessitent une surveillance régulière. C’est pourquoi la classe **`SuiviMedical`** a été intégrée au modèle.  
  
Ce choix est justifié par la volonté de ne pas réduire la plateforme à un simple outil de rendez-vous ou de visioconférence. HealthRuralNet doit aussi soutenir la continuité des soins. Le suivi médical permet donc de représenter les objectifs de prise en charge, les observations successives et l’évolution de l’état du patient dans le temps.  
  
Cette classe améliore la richesse métier du diagramme et montre que le modèle couvre bien la logique de prise en charge globale, pas seulement l’instant de consultation.  
  
### 11. Le consentement pour intégrer la dimension réglementaire  
  
La classe **`Consentement`** a été ajoutée pour répondre à une contrainte forte du sujet : la gestion sécurisée et encadrée des données médicales. Dans un système de santé, l’accès aux informations d’un patient ne peut pas être implicite ou laissé au hasard. Il doit être justifié, autorisé et contrôlé.  
  
Le choix de modéliser le consentement comme une classe indépendante est donc pertinent pour plusieurs raisons :  
- il reflète une réalité métier et réglementaire ;  
- il permet de dater l’autorisation ;  
- il distingue clairement la donnée médicale de la permission d’y accéder.  
  
Cette séparation renforce la **cohérence du modèle** et montre que la conception prend en compte non seulement les besoins fonctionnels, mais aussi les contraintes légales et éthiques.  
  
### 12. La structure de santé pour préparer l’interconnexion  
  
La classe **`StructureSante`** rattache les professionnels à un établissement ou à une organisation. Ce choix a été fait pour répondre au besoin d’interconnexion avec les structures locales mentionnées dans le sujet : hôpitaux, cliniques, cabinets, dispensaires.  
  
Même si cette partie du diagramme reste volontairement simple, elle est importante car elle prépare le modèle à une architecture plus large. En introduisant cette classe, on montre que les professionnels n’agissent pas tous de manière isolée ; ils s’inscrivent dans un environnement de santé organisé. Cela améliore la crédibilité du modèle et sa capacité à évoluer vers des mécanismes d’interopérabilité plus poussés.  
  
## Prise en compte des bonnes pratiques de conception  
  
Plusieurs bonnes pratiques ont guidé les choix de modélisation.  
  
D’abord, le diagramme cherche à respecter une logique de **responsabilité claire**. Chaque classe correspond à un objet métier identifiable et ne mélange pas plusieurs rôles. Par exemple, l’authentification est séparée dans `IdentifiantsSecurises`, la confidentialité dans `Confidentialite`, la planification dans `RendezVous` et l’acte médical dans `Consultation`.  
  
Ensuite, la conception favorise la **modularité**. Les classes sont découplées autant que possible pour que l’évolution de l’une n’entraîne pas la remise en cause de toutes les autres. Ce choix est fondamental pour un projet qui devra probablement évoluer avec le temps, intégrer de nouvelles pratiques médicales ou s’adapter à différents contextes régionaux.  
  
Le modèle a également été pensé pour la **maintenabilité**. La factorisation par héritage réduit la duplication, l’encapsulation protège les données sensibles et le polymorphisme évite les traitements conditionnels complexes. L’ensemble reste donc plus simple à comprendre, à tester et à faire évoluer.  
  
Enfin, la conception reste volontairement **équilibrée**. Le diagramme n’est ni trop minimaliste, ni inutilement surchargé. Il couvre les objets métier essentiels sans tomber dans une modélisation trop technique ou trop détaillée, ce qui aurait nui à la lisibilité.  
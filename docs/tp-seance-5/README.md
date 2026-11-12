# TP séance 5 — Gestion du cycle doctoral

Le modèle couvre l'inscription initiale, de la candidature à l'inscription définitive. La réinscription concerne un doctorant déjà inscrit et relève d'un parcours distinct. Les diagrammes du dépôt donnent le déroulement proposé. Les ressources GCD décrivent la procédure actuelle. Le tableau comparatif inspire le suivi visuel et la correction ciblée des pièces.

## Parcours candidat confirmé pour GCD+

L’accueil présente les sujets publiés sans connexion. Le visiteur peut les ajouter au panier, qui est conservé après connexion. L’envoi d’une candidature exige une connexion par e-mail. Chaque candidat peut postuler à trois sujets maximum par campagne, en comptant les candidatures déjà envoyées.

Le [tableau comparatif](../tableau-comparatif-gcd.pdf) rassemble les autres pistes discutées en atelier.

## 1. Diagramme de classes conceptuel

```mermaid
classDiagram
direction LR
class Personne {
  nom
  prénom
  coordonnées
}
class Candidat {
  parcours académique
  situation professionnelle
}
class Enseignant
class DirecteurThese
class CoEncadrant
class DirecteurStructure
class Administrateur
class StructureRecherche
class CentreDoctoral
class Etablissement
class FormationDoctorale
class Campagne {
  année universitaire
}
class Sujet {
  titre
  description
}
class Candidature {
  date de dépôt
  état
}
class Piece {
  intitulé
  date de remise
  état de conformité
}
class TypePiece {
  libellé
  contenu attendu
}
class Entretien {
  date
  lieu
  compte rendu
}
class CommissionSelection
class DecisionSelection {
  résultat
  motif
  rang éventuel
}
class DossierInscription
class Accord {
  qualité du signataire
  résultat
  date
}
class Inscription {
  date de confirmation
}

Personne <|-- Candidat
Personne <|-- Enseignant
Personne <|-- Administrateur
Enseignant <|-- DirecteurThese
Enseignant <|-- CoEncadrant
Enseignant <|-- DirecteurStructure
DirecteurStructure "0..*" -- "1" StructureRecherche : dirige
CentreDoctoral "1" -- "0..*" FormationDoctorale : coordonne
Etablissement "1" -- "0..*" FormationDoctorale : domicilie
FormationDoctorale "1" -- "0..*" Sujet : propose
StructureRecherche "1" -- "0..*" Sujet : accueille
Campagne "1" -- "0..*" Sujet : ouvre
DirecteurThese "1" -- "0..*" Sujet : supervise
CoEncadrant "0..*" -- "0..*" Sujet : coencadre
Candidat "1" -- "0..*" Candidature : dépose
Sujet "1" -- "0..*" Candidature : concerne
Candidature "1" *-- "0..*" Piece : dossier de candidature
TypePiece "1" -- "0..*" Piece : qualifie
CommissionSelection "0..1" -- "0..*" Candidature : examine
Candidature "1" -- "0..1" Entretien : donne lieu à
CommissionSelection "1" -- "0..*" Entretien : conduit
CommissionSelection "0..*" -- "1..*" Enseignant : comprend
Candidature "1" -- "0..*" DecisionSelection : reçoit
CommissionSelection "1" -- "0..*" DecisionSelection : prononce
Candidature "1" -- "0..1" DossierInscription : ouvre après sélection
DossierInscription "0..1" -- "0..*" Piece : pièces administratives
DossierInscription "1" *-- "0..*" Accord : recueille
Personne "1" -- "0..*" Accord : signe
DossierInscription "1" -- "0..1" Inscription : aboutit à
Administrateur "1" -- "0..*" Inscription : confirme
```

Une candidature porte sur un seul sujet. Un candidat peut déposer plusieurs candidatures dans la limite de trois candidatures envoyées par campagne. Les spécialisations d'enseignant représentent des fonctions cumulables. Un dossier administratif est ouvert après sélection, y compris après promotion depuis la liste d'attente.

## 2. Diagramme de classes détaillé

```mermaid
classDiagram
direction TB
class Utilisateur {
  UUID id
  String emailUnique
  String empreinteMotDePasse
  String statutCompte
}
class ProfilCandidat {
  UUID id
  String identite
  String parcoursAcademique
  String situationProfessionnelle
}
class ProfilEnseignant {
  UUID id
  String nom
  String prenom
}
class Role {
  String code
}
class Privilege {
  String code
}
class AttributionRole {
  UUID id
  String typePerimetre
  UUID perimetreId
  UUID attribueParId
  DateTime debut
  DateTime fin
}
class StructureRecherche {
  UUID id
  String nom
}
class CentreDoctoral {
  UUID id
  String nom
}
class Etablissement {
  UUID id
  String nom
}
class FormationDoctorale {
  UUID id
  String libelle
}
class Sujet {
  UUID id
  String titre
  String description
  String statutPublication
}
class Campagne {
  UUID id
  String anneeUniversitaire
  String fuseauHoraire
  Integer limiteCandidatures
}
class FenetreEtape {
  String codeEtape
  String publicConcerne
  DateTime ouverture
  DateTime cloture
}
class Candidature {
  UUID id
  EtatCandidature etat
  DateTime creeLe
  DateTime soumiseLe
  Integer version
}
class CommissionSelection {
  UUID id
}
class Entretien {
  UUID id
  DateTime dateHeure
  String lieuOuLien
  String compteRendu
}
class DecisionSelection {
  UUID id
  String resultat
  String motif
  Integer rang
  DateTime dateDecision
}
class DossierInscription {
  UUID id
  DateTime dateDepot
  String referenceRecu
}
class Accord {
  UUID id
  String qualiteSignataire
  String resultat
  DateTime signeLe
}
class Inscription {
  UUID id
  DateTime confirmeeLe
  String referenceCarte
}
class ExigencePiece {
  UUID id
  String typePiece
  String phase
  String formatsAutorises
  Integer tailleMaxOctets
  String conditionApplicabilite
  String critereContenu
}
class Piece {
  UUID id
  String statutConformite
}
class VersionFichier {
  UUID id
  Integer numero
  String nomOriginal
  String typeMimeDetecte
  Integer tailleOctets
  String cleStockage
  String empreinte
  DateTime deposeLe
}
class ControlePiece {
  UUID id
  String verdict
  String motif
  DateTime controleLe
  DateTime limiteCorrection
}
class EvenementAudit {
  UUID id
  String action
  String typeObjet
  UUID objetId
  DateTime horodatage
  EtatCandidature etatAvant
  EtatCandidature etatApres
  String motif
}
class Notification {
  UUID id
  String categorie
  String message
  String lienAction
  DateTime echeance
  DateTime lueLe
  String etatTraitement
  DateTime creeeLe
  DateTime traiteeLe
}
class SuiviCandidature {
  String etapeCourante
  String etapesTerminees
  String etapesRestantes
  String prochaineAction
  DateTime prochaineEcheance
}

Utilisateur "1" -- "0..1" ProfilCandidat
Utilisateur "1" -- "0..1" ProfilEnseignant
Utilisateur "1" -- "0..*" AttributionRole
Role "1" -- "0..*" AttributionRole
Role "0..*" -- "0..*" Privilege
CentreDoctoral "1" -- "0..*" FormationDoctorale
Etablissement "1" -- "0..*" FormationDoctorale
FormationDoctorale "1" -- "0..*" Sujet
StructureRecherche "1" -- "0..*" Sujet
ProfilEnseignant "1" -- "0..*" Sujet : directeur de thèse
ProfilEnseignant "0..*" -- "0..*" Sujet : coencadrants
Campagne "1" -- "0..*" Sujet
Campagne "1" *-- "1..*" FenetreEtape
Campagne "1" *-- "0..*" ExigencePiece
ProfilCandidat "1" -- "0..*" Candidature
Sujet "1" -- "0..*" Candidature
CommissionSelection "0..1" -- "0..*" Candidature : examine
Candidature "1" -- "0..1" Entretien
CommissionSelection "1" -- "0..*" Entretien
CommissionSelection "0..*" -- "1..*" ProfilEnseignant
CommissionSelection "1" -- "0..*" DecisionSelection
Candidature "1" *-- "0..*" DecisionSelection
Candidature "1" *-- "0..1" DossierInscription
DossierInscription "1" *-- "0..*" Accord
Utilisateur "1" -- "0..*" Accord : signataire
DossierInscription "1" -- "0..1" Inscription
Utilisateur "1" -- "0..*" Inscription : confirme
Candidature "1" *-- "0..*" Piece
DossierInscription "0..1" -- "0..*" Piece : phase administrative
ExigencePiece "1" -- "0..*" Piece
Piece "1" *-- "1..*" VersionFichier
VersionFichier "1" *-- "0..*" ControlePiece
Utilisateur "1" -- "0..*" ControlePiece : vérificateur
Candidature "1" -- "0..*" EvenementAudit
Utilisateur "0..1" -- "0..*" EvenementAudit : acteur
EvenementAudit "1" -- "0..*" Notification : déclenche
Utilisateur "1" -- "0..*" Notification : destinataire
Candidature ..> SuiviCandidature : vue calculée
SuiviCandidature ..> EvenementAudit
SuiviCandidature ..> FenetreEtape
```

### Règles d'implémentation

| Domaine | Règle |
|---|---|
| Comptes et droits | Compte actif requis pour agir. Les privilèges des rôles actifs se cumulent dans leur périmètre. Vérifier aussi la propriété du dossier, l'affectation au sujet, l'appartenance à la commission ou à l'établissement selon l'action. Le rôle d'administration des comptes ne confère pas le droit de décider d'une candidature. |
| Unicité et concurrence | Un dossier par couple candidat–sujet. La soumission exige un compte connecté par e-mail et vérifie le plafond de trois candidatures envoyées par campagne ainsi que la fenêtre applicable. Vérifier la version du dossier et verrouiller le quota du candidat pour la campagne dans la transaction de soumission. |
| Audit | Tracer création, modification, soumission, transmission, contrôles, décisions, accords et inscription. Journaliser l'acteur, l'action, l'objet, la date, le motif utile et les états avant/après. Une échéance automatique identifie le système comme auteur. Écrire la transition, l'événement et les notifications internes dans la même transaction. |
| Calendrier et progression | La campagne fournit les fenêtres de dépôt, de sélection et d'inscription, avec des créneaux distincts selon la liste si nécessaire. Le calendrier interactif ouvre le détail de chaque échéance. La chronologie du dossier utilise le journal pour afficher les étapes terminées, l'étape courante et les prochaines étapes applicables. Le statut et l'action sont affichés en texte en plus de leur repère visuel. |
| Notifications | Une convocation, une demande de correction ou une admission avec dossier à compléter crée une notification d'action pour le candidat. Une transmission crée une notification d'action pour la commission ou le service destinataire. La mise en attente, le rejet et la confirmation déclenchent une notification informative. La lecture est horodatée. Une action est traitée lorsque l'opération correspondante aboutit, ou expire à son échéance. |
| Pièces | Configurer par campagne et phase les types attendus, les formats, la taille maximale et les conditions, par exemple l'engagement de temps aménagé selon la situation. Contrôler le format détecté et la taille au dépôt, puis la lisibilité, la complétude et les signatures par le service habilité. États possibles : À vérifier, Conforme, À compléter, Non conforme. Un remplacement ajoute une version à la même pièce et ouvre un nouveau contrôle. Une décision de conformité porte sur la dernière version. |
| Dossier administratif | Enregistrer le dépôt papier et la référence du reçu lorsque la procédure le prévoit. Les accords exigés et l'établissement compétent dépendent du circuit applicable. La carte est délivrée après confirmation de l'inscription. |

## 3. Diagramme d'états-transitions

```mermaid
stateDiagram-v2
    state "Brouillon" as Brouillon
    state "Soumise" as Soumise
    state "En examen" as Examen
    state "Correction de candidature demandée" as Correction
    state "Convoquée" as Convoquee
    state "Entretien réalisé" as EntretienRealise
    state "Liste d'attente" as Attente
    state "Retenue" as Retenue
    state "Dossier administratif déposé" as Depot
    state "Complément administratif demandé" as Complement
    state "Inscription définitive" as Inscrite
    state "Rejetée" as Rejetee
    [*] --> Brouillon : Créer
    Brouillon --> Brouillon : Modifier
    Brouillon --> Soumise : Soumettre
    Soumise --> Examen : Transmettre
    Examen --> Correction : Demander une correction
    Correction --> Examen : Redéposer les pièces
    Correction --> Rejetee : Clore après délai
    Examen --> Convoquee : Convoquer
    Examen --> Rejetee : Refuser à la présélection
    Convoquee --> EntretienRealise : Consigner l'entretien
    Convoquee --> Rejetee : Constater une absence
    EntretienRealise --> Attente : Classer en attente
    EntretienRealise --> Retenue : Retenir
    EntretienRealise --> Rejetee : Refuser après entretien
    Attente --> Retenue : Appeler depuis la liste d'attente
    Attente --> Rejetee : Clore la liste
    Retenue --> Depot : Enregistrer le dépôt
    Retenue --> Rejetee : Clore faute de dépôt
    Depot --> Complement : Demander un complément
    Complement --> Depot : Enregistrer les pièces corrigées
    Complement --> Rejetee : Clore après délai
    Depot --> Rejetee : Refuser le dossier administratif
    Depot --> Inscrite : Confirmer l'inscription
    Inscrite --> [*]
    Rejetee --> [*]
```

Les événements du diagramme utilisent les conditions et habilitations ci-dessous. Chaque événement est journalisé. Les clôtures pour échéance, l'absence à l'entretien et les demandes de correction sont des règles proposées à valider avec l'atelier.

| Événement | Acteur habilité et condition | Action |
|---|---|---|
| Créer, modifier | Candidat propriétaire, campagne ouverte et sujet publié à la création | Enregistrer le brouillon ou la nouvelle version. |
| Soumettre | Candidat propriétaire, fenêtre ouverte, quota disponible et pièces obligatoires présentes | Horodater le dépôt et notifier sa réception. |
| Transmettre | Gestionnaire habilité, dossier soumis et commission affectée | Affecter le dossier à la commission et lui notifier l'examen. |
| Demander une correction | Commission habilitée, pièce de candidature manquante ou non conforme et correction autorisée | Indiquer les pièces, le motif et le délai. Notifier le candidat. |
| Redéposer les pièces | Candidat propriétaire, demande ouverte et délai respecté | Ajouter les versions, repasser les pièces à vérifier et notifier la commission. |
| Convoquer | Directeur de thèse habilité sur le sujet, dossier présélectionné et créneau autorisé | Créer l'entretien et notifier le candidat avec les modalités. |
| Consigner l'entretien | Membre habilité de la commission, entretien tenu | Enregistrer le compte rendu. |
| Refuser à la présélection, constater une absence, refuser après entretien | Commission habilitée, respectivement critère de présélection non satisfait, absence constatée selon les règles de campagne, ou décision défavorable | Consigner le motif et notifier le candidat. |
| Classer en attente, retenir | Commission habilitée, décision consignée après entretien | Enregistrer le rang si applicable ou ouvrir le dossier administratif. Notifier le résultat. |
| Appeler depuis la liste d'attente | Gestionnaire habilité, place libérée, rang éligible et période applicable ouverte | Ouvrir le dossier administratif et notifier l'échéance. |
| Clore la liste, clore faute de dépôt | Gestionnaire habilité, liste officiellement close ou échéance de dépôt dépassée sans dossier enregistré | Enregistrer le motif de clôture et notifier le candidat. |
| Enregistrer le dépôt | Service de l'établissement, dossier reçu pendant le créneau applicable | Enregistrer la date et le reçu, puis déclencher le contrôle. |
| Demander un complément | Service de l'établissement, pièce administrative manquante ou non conforme et correction autorisée | Indiquer les pièces, le motif et le délai. Notifier le candidat. |
| Enregistrer les pièces corrigées | Service de l'établissement, pièces remises dans le délai applicable | Ajouter les versions et relancer le contrôle ciblé. |
| Clore après délai | Commission pour la candidature, service de l'établissement pour le dossier administratif. Délai expiré et demande non satisfaite | Consigner le motif et notifier le candidat. |
| Refuser le dossier administratif | Service de l'établissement, motif définitif établi selon les règles de campagne | Consigner le motif et notifier le candidat. |
| Confirmer l'inscription | Service de l'établissement, dernières versions conformes, dépôt enregistré et accords requis obtenus | Créer l'inscription, notifier la confirmation et rendre disponible la référence de carte lorsqu'elle est attribuée. |

## Références de travail

- Énoncé de la séance 5, TP O&M Développement Logiciel.
- Archive Ressources Atelier O&M Dev Logiciel, notamment le guide professeur, la procédure d'inscription administrative 2026–2027 et le calendrier de campagne.
- [Diagramme d'activités d'inscription du dépôt de l'atelier](https://github.com/Hamza-Tazi-212/atelier-gestion-cycle-doctoral/blob/main/docs/diagrammes/inscription.md).
- Tableau comparatif GCD, synthèse du débat USMBA. Idées retenues dans ce modèle : suivi de la prochaine action, calendrier interactif et remplacement ciblé d'une pièce.

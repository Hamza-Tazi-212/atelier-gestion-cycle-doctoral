# Déroulement du développement de GCD+

## 1. Analyser les parcours de l’atelier

Les ressources de l’atelier ont servi à relever les acteurs, les documents attendus et les points de décision pour l’inscription et la réinscription. Le TP 5 a formalisé les classes et les états de la candidature. Le tableau comparatif a inspiré le suivi visuel, le calendrier et la correction ciblée des pièces.

## 2. Adapter CDOC au contexte de l’EST de Fès

Le développement a repris la base Laravel de CDOC pour réutiliser les formulaires, les comptes et le dossier candidat. L’interface a été adaptée à GCD+ avec des libellés en français, des accents bleus et rouges et une entrée unique pour la connexion. Le compte détermine le tableau de bord accessible et les fonctions du personnel sont affectées par sujet.

## 3. Réaliser le parcours candidat

Le catalogue et le panier sont accessibles avant la connexion. Le panier est conservé au moment de se connecter. Le candidat complète son profil personnel et académique, puis ajoute la lettre de motivation lors du dépôt. Le serveur contrôle l’ouverture du sujet, la période de campagne, l’absence de doublon et le quota de trois candidatures par campagne. Le tableau de bord et l’historique affichent l’évolution du dossier.

## 4. Mettre en place le travail du personnel

| Acteur | Responsabilités principales dans GCD+ |
| --- | --- |
| Administrateur | Gère les comptes, les affectations, les formations, les sujets et les périodes. Peut instruire les dossiers. |
| Encadrant | Complète les sujets, les publie après accord de la structure et du CED et donne un avis sur les réinscriptions. |
| Co-encadrant | Consulte les sujets et dossiers associés à son affectation. |
| Responsable de la structure de recherche et directeur du CED | Donnent un avis sur les sujets et les réinscriptions de leur périmètre. |
| Commission de sélection | Examine les candidatures, peut demander un remplacement de pièce, programme l’oral et consigne les décisions. |
| Comité de thèse | Donne un avis sur le bilan annuel et la réinscription. |
| Chef d’établissement | Donne l’accord de l’établissement et statue sur les prolongations. |
| Gestionnaire ESTF | Contrôle les pièces, confirme les inscriptions et traite les compléments et décisions de réinscription. |

La sélection gère l’admissibilité à l’oral, la liste d’attente avec rang, l’acceptation et le refus. Les décisions sont journalisées. Une inscription confirmée ouvre le parcours de réinscription, avec bilan annuel, justificatif facultatif, avis de l’encadrant, accords institutionnels et décision finale. Une prolongation demandée requiert également l’accord du chef d’établissement pour confirmer la réinscription.

## 5. Améliorer le dépôt et la recherche

- **Dépôt de documents** · import direct d’un PDF ou scan depuis le navigateur. Chaque page peut être agrandie, pivotée ou reprise avant de composer un PDF de plusieurs pages.
- **Prochaine action** · la fiche de candidature indique l’étape attendue et, en cas de correction, mène au champ de remplacement de la pièce.
- **Catalogue** · les filtres par laboratoire et domaine se combinent avec le texte et la formation. Les filtres actifs restent visibles pendant la navigation, avec réinitialisation et neuf sujets par page.

## Vérifications effectuées

- 37 tests Laravel réussis sur la version locale. Ils couvrent notamment le panier après connexion, le quota, les permissions, les corrections, les validations et la pagination filtrée.
- Dans Chrome, vérification du catalogue en vue mobile et du scan recto verso avec une caméra simulée. Le PDF obtenu contient deux pages et conserve la rotation appliquée.
- Les sources PlantUML et les rendus SVG des diagrammes sont disponibles dans le [dossier des diagrammes](diagrammes/README.md).

# cv-optimizer

Générateur de CV adapté à une offre d'emploi.

À partir d'un profil structuré que le candidat a lui-même validé, le service produit un CV en PDF adapté à une offre, sans jamais ajouter d'information absente du profil.

> Statut : développement initial. Le service n'est pas encore utilisable.

## Principes du produit

- **Aucune invention.** Chaque élément du CV généré provient du profil validé par le candidat. Un contrôle automatique rejette toute sortie contenant une information absente du profil.
- **Pas d'évaluation de candidats.** Le service s'adresse aux candidats. Il ne note, ne classe et ne filtre personne.
- **Données minimales, durée limitée.** Les offres et les CV générés sont supprimés après 30 jours. Hébergement dans l'Union européenne.
- **Le candidat garde la main.** Il relit et valide son profil, y compris lorsqu'il provient de l'import d'un CV existant.

## Parcours prévu

1. Création du compte et acceptation de la politique de confidentialité.
2. Constitution du profil : saisie manuelle ou import d'un CV au format PDF, puis relecture et validation.
3. Saisie d'une offre d'emploi.
4. Génération du CV, accompagnée d'un rapport indiquant ce qui a été retenu, reformulé ou écarté.
5. Téléchargement du PDF.

## Architecture

Application sans serveur à administrer, hébergée sur AWS dans l'Union européenne.

- Front : React et TypeScript, servi par S3 et CloudFront.
- API : API Gateway et AWS Lambda (Python), authentification par Amazon Cognito.
- Génération : pipeline orchestré par AWS Step Functions, appels au modèle Amazon Nova 2 Lite via Amazon Bedrock, contrôle de cohérence exécuté en code.
- Données : DynamoDB et S3, chiffrés avec AWS KMS.
- Infrastructure décrite avec Terraform. Intégration et déploiement continus avec GitHub Actions, authentifiés auprès d'AWS par OIDC. Analyse de la qualité du code avec SonarQube Cloud.

## Feuille de route

- [ ] Dépôt et documentation de base
- [ ] Socle AWS : compte sécurisé, Terraform, journal d'audit, budget
- [ ] Intégration continue
- [ ] Premier déploiement de bout en bout
- [ ] Authentification et profil
- [ ] Génération de CV et contrôle de cohérence
- [ ] Export PDF et historique
- [ ] Import d'un CV existant
- [ ] Bêta fermée

## Structure cible du dépôt

Les dossiers sont ajoutés au fil des besoins.

| Dossier | Contenu |
|---|---|
| `infra/` | Infrastructure AWS décrite avec Terraform |
| `.github/workflows/` | Workflows d'intégration et de déploiement continus |
| `apps/web/` | Application web (React, TypeScript) |
| `services/` | API et pipeline de génération (Python) |
| `packages/schemas/` | Schémas de données partagés entre le front et le back |
| `evals/` | Jeu d'évaluation de la qualité de génération |
| `docs/` | Décisions d'architecture, sécurité, conformité |

## Sécurité

Aucun secret n'est versionné et aucune clé d'accès AWS de longue durée n'est utilisée. Une procédure de signalement des vulnérabilités sera publiée avant toute mise en production.

## Contributions

Le projet est en phase initiale : les contributions externes ne sont pas acceptées pour le moment.

## Licence

Tous droits réservés. Le dépôt est consultable publiquement mais n'est pas sous licence libre : toute réutilisation du code nécessite un accord écrit.

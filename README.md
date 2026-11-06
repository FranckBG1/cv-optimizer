# cv-optimizer

Nom provisoire, il pourra changer.

## C'est quoi ?

Un service web qui aide un candidat à répondre à une offre d'emploi. Le candidat renseigne une fois son parcours (son "dossier de compétences"), colle une offre, et obtient un CV en PDF adapté à cette offre.

Règle d'or : le CV généré ne contient jamais d'information absente du dossier du candidat. Rien n'est inventé.

Ce projet a deux buts : construire un vrai produit, et apprendre le DevOps en le faisant (infrastructure en code, livraison automatique, sécurité, conformité).

## Où en est le projet ?

On avance par petites tranches. Chaque tranche doit fonctionner de bout en bout avant de passer à la suivante.

- [x] 1. Dépôt GitHub créé
- [ ] 2. Compte AWS sécurisé (à faire dans la console, sans code)
- [ ] 3. Premier Terraform : stockage de l'état, connexion GitHub vers AWS, journal d'audit, budget
- [ ] 4. Première intégration continue (GitHub Actions)
- [ ] 5. Premier déploiement : une page statique en ligne, publiée automatiquement

Ensuite viendront, un par un : l'authentification, le dossier de compétences, la génération du CV, l'export PDF, l'import d'un CV existant.

## Ce que contient le dépôt aujourd'hui

| Fichier | À quoi il sert |
|---|---|
| `README.md` | Ce fichier : présenter le projet et où il en est. |
| `CLAUDE.md` | Mémo lu par Claude Code : contexte, décisions, règles de travail. |
| `.gitignore` | Liste de ce que Git doit ignorer, pour ne jamais publier par erreur un mot de passe, une clé ou un fichier d'état Terraform. |
| `docs/architecture/` | Le PDF de spécifications et d'architecture (la "carte" du projet entier). |

C'est volontairement peu. Le PDF décrit tout ce qui sera construit, mais on ne crée un dossier que lorsqu'une tranche en a besoin.

## Ce qui sera ajouté plus tard, et pourquoi

| Dossier | Rôle | Arrive vers |
|---|---|---|
| `infra/` | Terraform : décrit l'infrastructure AWS dans des fichiers texte, pour la recréer à l'identique et garder l'historique. | Tranche 3 |
| `.github/workflows/` | Recettes automatiques (GitHub Actions) : vérifier le code et le déployer. | Tranche 4 |
| `docs/adr/` | Une courte note par décision d'architecture importante, pour se rappeler pourquoi on a choisi ça. | Dès la première décision à noter |
| `apps/web/` | Le site que voit le candidat (React et TypeScript). | Après la tranche 5 |
| `services/` | Le code serveur en Python : l'API et la génération de CV. | Après le site |
| `packages/schemas/` | La description commune des données, partagée entre le site et le serveur pour qu'ils parlent le même langage. | Quand le site et le serveur existent |
| `evals/` | Tests de qualité du modèle de langage (vérifier qu'il n'invente rien). | Avec la génération de CV |
| `docs/security/`, `docs/compliance/` | Menaces identifiées, registre RGPD, analyse d'impact. | Avec la première fonctionnalité qui manipule des données personnelles |

## Comment on travaille

- Une tranche à la fois, et on explique ce qu'on fait et pourquoi.
- Une modification importante se fait sur une branche courte, puis une pull request, plutôt que directement sur `main`.
- Messages de commit sous la forme `type: résumé` (par exemple `docs: ajoute le README`). Types courants : `feat`, `fix`, `docs`, `chore`, `infra`, `ci`.
- Aucun secret dans le dépôt : ni mot de passe, ni clé d'accès, ni donnée personnelle réelle.
- Aucune clé AWS de longue durée : GitHub se connecte à AWS par un mécanisme temporaire (OIDC).

## Décisions déjà prises

- Cible : les candidats (pas les recruteurs). Aucun classement de candidats.
- Site en React et TypeScript, serveur en Python.
- Tout sur AWS, sans serveur à administrer, décrit avec Terraform.
- Un seul compte AWS au départ.
- Livraison avec GitHub Actions, qualité du code avec SonarQube Cloud.
- Modèle de langage : Amazon Nova 2 Lite, hébergé dans l'UE.
- Pas de paiement dans la première version. Offres et CV générés supprimés après 30 jours.

Le détail et les raisons sont dans le PDF de `docs/architecture/`.

## Licence

Tous droits réservés.

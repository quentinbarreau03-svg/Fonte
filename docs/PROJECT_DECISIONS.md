# Décisions du projet Fonte

Index des décisions importantes, pour qu'une nouvelle session puisse reprendre sans l'historique des conversations. Le détail et le contexte restent dans les documents cités (ils font foi en cas d'écart). Ajouter chaque nouvelle décision en bas de la section concernée, avec sa date et qui l'a prise.

## Produit et règles sportives (propriétaire, phase 2 — détail : `PROJECT_STATUS.md`, « Décisions prises »)

| N° | Décision | Où |
|---|---|---|
| 1 | Firebase : oui. Sauvegarde et synchro par compte e-mail + mot de passe (un par profil), hors ligne et copie locale conservés, sécurité et isolation prioritaires | `PROJECT_STATUS.md`, `docs/FORMAT_DONNEES.md` |
| 2 | Programmes : aucune modification automatique ; propositions validées seulement ; test « photographie » des programmes | `docs/PROPOSITIONS_PROGRAMMES.md` |
| 3 | Échec : approche prudente, plus d'échec chez le débutant, cibles de répétitions en réserve | `docs/REGLES_SPORTIVES.md` |
| 4 | Passage de niveau proposé, jamais automatique | `docs/REGLES_SPORTIVES.md` |
| 5 | Profils multiples indépendants sur un même appareil, séparés mais pas secrets | `PROJECT_STATUS.md` |
| — | Propositions de programmes (7 nov. 2026) : P1, P2, P4, P5, P7, P8, P9, P10 acceptées ; P3, P6 refusées ; application en phase 3E | `docs/PROPOSITIONS_PROGRAMMES.md`, section 2 |

## Techniques (phases 2 à 3D — détail : `PROJECT_STATUS.md`, `docs/PLAN_PHASE2.md`)

- Moteurs en fichiers séparés (`js/engine-*.js`), **sans outil de construction** ; `tools/assemble.js` reconstitue le fichier unique « artifact Claude » si besoin.
- **Aucune dépendance dans l'appli** ; outils de test (Playwright, émulateur Firebase) installés seulement en CI.
- Une seule migration de format v1 → v2 (`SCHEMA_VERSION = 2`) ; `fonte:v1` conservé comme copie de secours.
- Une séance = un document Firestore, corbeille 30 jours, règles Firestore testées à l'émulateur.
- La `VERSION` de `sw.js` change à chaque livraison de l'appli.
- Fixtures v1 inventées, jamais régénérées ; photographie des programmes refaite seulement sur décision écrite.

## Organisation du travail avec Claude Code (9 nov. 2026, propriétaire)

- **Équipe multi-agents** : un directeur technique (session principale) et cinq sous-agents (`docs/AI_TEAM.md`), tous en `model: opus`.
- Processus de validation en 10 phases pour les fonctionnalités importantes, contrôle proportionné pour les petites (`docs/DEVELOPMENT_WORKFLOW.md`).
- Seul `developer` modifie le code applicatif ; chercheur et UX/UI sans outil d'écriture ni Bash ; architecte et testeur sans outil d'écriture, Bash limité par instruction.
- Au plus 2 sous-agents simultanés.
- Fusion dans `main` et publication : **uniquement avec l'autorisation explicite du propriétaire.**
- Interdits git/GitHub inscrits dans `.claude/settings.json` (règles `deny`, sans hook ni script).
- Pas de `node --check js/*.js` ajouté à la CI (refusé par le propriétaire le 9 nov. 2026 : gain minime, les tests chargent déjà tous les fichiers `js/`).
- Pas de linter ni de typage ajouté : il faudrait des dépendances.

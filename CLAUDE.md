# Fonte — règles pour Claude Code

Fonte est une appli de musculation en français (PWA, iPhone/iPad/Android, hors ligne), en JavaScript **sans dépendance ni étape de construction** : `index.html`, moteurs `js/engine-*.js`, interface `js/app.js`, service worker `sw.js`. Détail des fichiers : `README.md`, section « Fichiers ».

## À lire au début de chaque session
1. **`PROJECT_STATUS.md`** : mémoire du projet (phases, état, décisions, prochaine étape). On le lit en premier et on le met à jour en dernier.
2. **`docs/PROJECT_DECISIONS.md`** : index des décisions techniques et produit.
3. **`docs/DEVELOPMENT_WORKFLOW.md`** : processus de développement et de validation.
4. **`docs/AI_TEAM.md`** : l'équipe d'agents et comment la solliciter.

## Rôles
- **Propriétaire du projet** (humain, pilote depuis un iPad) : décisions finales, autorisation de fusion et de publication.
- **Directeur technique** (session principale Claude Code, modèle Opus) : analyse les demandes, répartit les missions, décide techniquement, valide, commite et pousse, rédige le rapport final.
- **Sous-agents** (`.claude/agents/`, modèle `opus`) : `technical-researcher`, `software-architect`, `developer`, `quality-assurance`, `ux-ui-specialist`. Seul `developer` modifie le code applicatif.

## Délégation (résumé ; détail dans `docs/DEVELOPMENT_WORKFLOW.md`)
- **Modification mineure** (texte, petit bug localisé, doc) : le directeur intervient directement ou via `developer`, avec un contrôle proportionné (au minimum `node tests/run.js`).
- **Fonctionnalité importante** : recherche → architecte → plan → développeur → (audit architecte) → testeur → UX/UI si interface → corrections → rapport → autorisation du propriétaire.
- **Au plus 2 sous-agents en même temps**, jamais deux agents sur le même fichier.
- Chaque mission donnée à un agent est **autonome** : objectif, fichiers, contraintes, critères d'acceptation, format du rapport (l'agent ne voit pas la conversation).
- Le directeur vérifie lui-même ce que rapportent les agents (diff, résultats de tests) avant de conclure.

## Règles GitHub absolues
- Ne jamais travailler directement sur `main` ; toujours une branche de travail (celle imposée par la session si elle existe).
- Ne jamais fusionner une Pull Request, ni activer la fusion automatique : **la fusion et la publication demandent l'autorisation explicite du propriétaire.**
- Ne jamais réécrire l'historique (pas de `rebase`, `reset --hard`, `push --force`), ne jamais supprimer une branche ou un dépôt, ne jamais modifier les paramètres du compte GitHub.
- Examiner le diff complet avant chaque commit et chaque Pull Request.
- Ces interdits sont aussi inscrits dans `.claude/settings.json` (règles `deny`).

## Sécurité et coûts
- Ne jamais exposer de secret ; ne jamais créer de clé API, de facturation ou de service payant sans autorisation explicite du propriétaire.
- Aucune dépendance ajoutée à l'appli. Les outils de test (Playwright, émulateur Firebase) restent hors de l'appli.

## Tests
- `node --check sw.js` et `node tests/run.js` : calculs, sans dépendance, rapides.
- `node tests/e2e/run.js` : navigateur (Playwright + Chromium, iPhone 13 et iPad simulés). Dans l'environnement cloud, Playwright est préinstallé : ne pas lancer `playwright install`.
- Règles Firestore : émulateur Firebase, exécutées par GitHub Actions (`.github/workflows/tests.yml`).
- Ce qui ne peut pas être testé ici (vrai Safari, vrais appareils, vraie base Firebase) va dans une checklist pour le propriétaire (`docs/CHECKLIST_*.md`).
- Ne jamais annoncer un test réussi sans l'avoir exécuté ; copier les résultats réels.

## Invariants de Fonte à protéger
Données déjà enregistrées (format versionné `SCHEMA_VERSION`, copie `fonte:v1` conservée), isolation des profils, synchro et règles Firestore, fonctionnement hors ligne, `VERSION` de `sw.js` à changer quand l'appli change, photographie des programmes (`tests/fixtures/programmes.json`) jamais régénérée sans raison écrite.

## Quota
L'utilisation d'Opus est limitée : ne pas lancer d'agent pour une tâche simple, réutiliser les dossiers déjà écrits dans `docs/`, demander des rapports synthétiques, prévenir le propriétaire avant une mission coûteuse.

## Langue
Tout en français : interface, commentaires, documents, rapports, messages de commit.

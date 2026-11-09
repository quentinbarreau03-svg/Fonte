# Processus de développement et de validation de Fonte

S'applique à toute session Claude Code. Équipe : `docs/AI_TEAM.md`. Règles générales : `CLAUDE.md`.

## 1. Qualifier la demande

| Type | Exemples | Équipe sollicitée | Contrôle minimum |
|---|---|---|---|
| **Mineure** | texte, libellé, doc, petit bug localisé sans effet sur les données | Directeur seul, ou `developer` | `node --check sw.js`, `node tests/run.js`, relecture du diff |
| **Moyenne** | correction touchant plusieurs fonctions, petit ajout d'interface | `developer` + `quality-assurance` (+ `ux-ui-specialist` si l'interface change) | tests calculs + navigateur |
| **Importante** | nouvelle fonctionnalité, format de données, synchro, service worker, règles Firestore, dépendance, programmes | Processus complet (section 2) | tout le processus |

En cas de doute, le directeur choisit le niveau supérieur et le dit au propriétaire. Il prévient avant une mission coûteuse en quota.

## 2. Processus complet (fonctionnalité importante)

1. **Recherche** — `technical-researcher` établit un dossier (format de sa définition). Réutiliser les documents déjà présents dans `docs/`.
2. **Faisabilité** — `software-architect` examine le dossier et rend un verdict. Si ❌ : on revoit la solution avant tout développement.
3. **Planification** — le directeur écrit : objectifs, fichiers concernés, risques, étapes, **critères d'acceptation**. Les décisions qui reviennent au propriétaire lui sont posées à ce moment.
4. **Développement** — `developer` réalise et documente (fichiers modifiés, choix techniques, tests).
5. **Audit** — `software-architect` relit le diff des modifications importantes.
6. **Tests** — `quality-assurance` contrôle indépendamment (relance les tests, relit le diff, cas limites).
7. **Ergonomie** — `ux-ui-specialist` si l'interface est touchée.
8. **Corrections** — tout problème retourne au `developer` ; les tests concernés sont **relancés** après correction.
9. **Rapport final** — par le directeur (section 4).
10. **Autorisation** — le propriétaire autorise la fusion dans `main` et toute publication. Rien n'est fusionné sans son accord explicite.

Les étapes 5 et 6 peuvent tourner en parallèle (au plus 2 agents à la fois) puisqu'elles ne modifient rien.

## 3. Règles d'orchestration

- Au plus **2 sous-agents simultanés** ; jamais deux agents sur le même fichier.
- Une mission = un message autonome (l'agent ne voit pas la conversation).
- Rapports structurés et synthétiques ; le directeur vérifie les preuves (diff, sorties de tests) au lieu de les recopier sans contrôle.
- Le directeur commite et pousse sur la branche de travail ; les agents ne commitent pas.
- Les nouvelles décisions sont ajoutées à `docs/PROJECT_DECISIONS.md` ; l'état d'avancement à `PROJECT_STATUS.md`.

## 4. Rapport final (modèle)

- Ce qui a été développé.
- Fichiers modifiés.
- Résultats des tests (chiffres réels, commandes).
- Résultats des audits (architecte, UX/UI).
- Anomalies corrigées.
- Problèmes restants.
- Risques connus.
- Limites des vérifications (vrai Safari, vrais appareils, vraie base Firebase…) et checklist pour le propriétaire si besoin.

## 5. Git et Pull Request

1. Travailler sur la branche de travail (jamais `main`).
2. Avant chaque commit : `git status`, `git diff` complet relu.
3. Commits clairs, en français.
4. `git push -u origin <branche>`.
5. Pull Request vers `main` uniquement si le propriétaire l'a demandée ; description = rapport final.
6. Attendre la CI GitHub Actions (tâches `tests`, `navigateur`, `regles`) et l'autorisation du propriétaire. **Jamais de fusion par Claude.**

## 6. Contrôles automatiques existants

- GitHub Actions `.github/workflows/tests.yml` à chaque push et Pull Request : syntaxe de `sw.js` + tests des calculs, tests navigateur (iPhone 13 / iPad), règles Firestore (émulateur).
- Pas de linter ni de vérification de typage : le projet n'a aucune dépendance et la décision est de ne pas en ajouter pour cela (voir `docs/PROJECT_DECISIONS.md`).

# L'équipe d'agents Claude Code de Fonte

Mise en place le 9 nov. 2026. Définitions : `.claude/agents/*.md`. Règles générales : `CLAUDE.md`. Processus : `docs/DEVELOPMENT_WORKFLOW.md`.

## Organisation

| Rôle | Qui | Modèle | Peut modifier le code ? |
|---|---|---|---|
| Propriétaire | Humain (iPad) | — | Décide, autorise fusion et publication |
| Directeur technique | Session principale Claude Code | Opus (celui de la session) | Oui (petites modifications, commits, pushs, PR) |
| `technical-researcher` | Sous-agent | `opus` | Non |
| `software-architect` | Sous-agent | `opus` | Non |
| `developer` | Sous-agent | `opus` | **Oui, seul sous-agent autorisé** |
| `quality-assurance` | Sous-agent | `opus` | Non |
| `ux-ui-specialist` | Sous-agent | `opus` | Non |

`model: opus` est un alias : il désigne la version d'Opus proposée par Claude Code au moment de l'appel. Le modèle réellement utilisé se vérifie dans les journaux de la session (voir « Vérifier le modèle »), pas dans ce document.

## Permissions réelles (outils)

| Agent | Outils | Garantie |
|---|---|---|
| `technical-researcher` | Read, Grep, Glob, WebSearch, WebFetch | **Technique** : aucun outil d'écriture ni Bash |
| `ux-ui-specialist` | Read, Grep, Glob | **Technique** : aucun outil d'écriture ni Bash |
| `software-architect` | Read, Grep, Glob, Bash | Pas d'Edit/Write ; Bash limité à la lecture **par instruction** (+ `git status` en fin de rapport, vérifié par le directeur) |
| `quality-assurance` | Read, Grep, Glob, Bash | Pas d'Edit/Write ; Bash limité aux tests et à la lecture **par instruction** (+ `git status` en fin de rapport) |
| `developer` | Read, Grep, Glob, Edit, Write, Bash | Pas de commit/push par instruction ; interdits git de `.claude/settings.json` |

Limite connue : Claude Code ne permet pas de restreindre Bash à des commandes en lecture seule de façon totalement sûre. C'est pourquoi le directeur vérifie `git status` après chaque intervention de l'architecte ou du testeur.

Les règles `deny` de `.claude/settings.json` s'appliquent à tous (directeur compris) : pas de `push --force`, pas de push vers `main`, pas de `reset --hard`, `rebase`, suppression de branche, `git clean`, pas de fusion de PR ni de fusion automatique via GitHub, pas de suppression de fichier via l'API GitHub, pas de création ni de fork de dépôt.

## Comment solliciter un agent

Le directeur utilise l'outil Agent avec le nom de l'agent (`subagent_type`). Chaque mission contient :
1. le contexte minimal (l'agent ne voit pas la conversation) ;
2. l'objectif ;
3. les fichiers concernés et ceux à ne pas toucher ;
4. les documents à lire (dossier du chercheur, avis de l'architecte…) ;
5. les critères d'acceptation ;
6. le format de rapport attendu (celui de la définition de l'agent).

Le propriétaire peut aussi demander directement : « demande au chercheur de… », « fais auditer par l'architecte… ».

## Vérifier le modèle utilisé

Chaque sous-agent laisse un journal dans le dossier de la session (`~/.claude/projects/<projet>/<session>/subagents/*.jsonl`). Le champ `"model"` des messages de l'agent indique le modèle réellement servi. Le directeur le contrôle lors des tests d'installation et peut le refaire à la demande.

## Utilisation depuis l'iPad

Tout fonctionne dans une session Claude Code cloud (application Claude ou claude.ai/code) : les agents sont des fichiers du dépôt, aucun PC n'est nécessaire. Les agents sont chargés au démarrage de la session ; après modification de `.claude/agents/`, ouvrir une nouvelle session pour être sûr de la prise en compte.

## Coûts

Aucun coût externe : pas de clé API, pas de facturation. Les sous-agents consomment le quota de l'abonnement Claude du propriétaire (chaque agent repart sans contexte, ce qui coûte plus cher qu'une simple réponse du directeur).

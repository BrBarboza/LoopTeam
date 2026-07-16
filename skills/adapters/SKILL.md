---
name: adapters
description: Detecção e fallback de ferramentas externas (rtk, graphify, ruflo) do LoopTeam. Carregar no início de /loopteam:start e sempre que loopteam-core precisar de contexto de código, tokens, orquestração ou memória.
---

100% vanilla por padrão. Ferramenta externa é upgrade detectado, nunca dependência.

| Capacidade | Upgrade se instalado | Fallback vanilla |
|---|---|---|
| Contexto de código | `graphify query/path/explain` | `Grep`/`Glob` + ler só arquivo-alvo |
| Tokens | `rtk read/grep/test` | `head`/`tail` + saída de teste só com falha |
| Orquestração | `ruflo swarm` + `memory_store` | `Task`/`Agent`: subagentes nomeados, spawn concorrente |
| Memória | `ruflo memory` (MCP) | `.loopteam/memory/DECISIONS.md` append-only: data, item, decisão/falha, 1 linha |
| Loop com critério | `/goal` nativo | loop manual, até 5 tentativas contadas |

Detecção (1x por sessão, silenciosa): `command -v rtk`, `command -v graphify`, `test -d .claude-flow`. Guarde o resultado, não repita.

Regras:
- Nunca instale nada. Upgrade que faria diferença real (ex.: repo >50 arquivos sem `graphify`) → sugira em 1 linha, só no fim do `/loopteam:start`.
- Falha de adapter cai pro fallback em silêncio; só registra em memória se repetir e afetar decisão futura.
- `rtk` pode colidir com binário homônimo — confirme com `rtk --version`/`rtk gain` antes de confiar; senão trate como ausente.
- Upgrade muda só COMO economiza, nunca O QUE o time faz.

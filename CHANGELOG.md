# Changelog

## v0.3.1

- `approved: false` no frontmatter de extensão gerada — só vira `true` após OK do dono; `loopteam-core` ignora extensão não aprovada ao rotear.
- `/loopteam:start` lista 1 linha com quantas extensões aguardam aprovação.
- `verify-signature` ganha gatilho explícito no passo 4 (item de UI); sem `docs/SIGNATURE.md`, pula sem avisar.

## v0.3.0

- Dependência explícita (`dep:`) no BRIEFING, aprovação 1-a-1 com trava verbatim pra extensão `executor`, `template_version` rastreado.
- Gate `.loopteam/state` com cobertura dupla (`start`/`loopteam` checam, sem exceção ad-hoc) e pendência `[?]` com contador de sessões.
- `docs/JUDGING.md` ganha teto de 5 versões completas; `CHANGELOG.md` criado.

## v0.2.0

- Instalação em marketplace próprio (`/plugin marketplace add` + `/plugin install`), ponto de entrada único `/loopteam:start`, `/loopteam:on`/`/loopteam:off`, `/loopteam <tarefa>` ad-hoc.
- Núcleo desescopado: Supabase/n8n saem do plugin, viram captura dinâmica (`tool-capture`) em `.loopteam/extensions/` do projeto.
- `loopteam-core` 97→44 linhas, fan-out variável 1-3, `[?]` pra item `consultant` sem confirmação.

## v0.1.0

- Primeira versão: `loopteam-core` (roteiro de 7 passos), `adapters` (rtk/graphify/ruflo com fallback vanilla), `verify-signature`.
- `/kickoff` e `/briefing`, extensões `supabase-executor`/`n8n-consultant` hardcoded no plugin.
- `docs/JUDGING.md` como rubrica de auto-avaliação.

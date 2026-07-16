# Changelog

## v0.4.0

Correções de teste de campo real (4 falhas observadas em uso):

- **Trava de escopo**: `loopteam-core` passo 1 exige citar o item VERBATIM antes de codar; adição estrutural fora do texto do item = escopo novo, pergunta "s/n" antes de codar. `/briefing` exige critério mensurável embutido em todo item (item vago não entra no checklist) e deixa explícito que o checklist é PROPOSTA — nada executa antes de "aprovar briefing".
- **Critério inverificável bloqueia, nunca finge entrega**: passo 1 checa se o critério é executável no ambiente atual; se não, para antes de codar e oferece "resolver ambiente" ou "dividir o item" (parte verificável agora + `[?]` bloqueada-por-ambiente).
- **Onboarding**: novo comando `/loopteam:help` (mapa em 3 frases + tabela de comandos). `/loopteam:start` sem BRIEFING mostra o mini-mapa em vez de só oferecer criar. README reescrito com "Primeiros 5 minutos" no topo. `start`/`briefing` sempre declaram na 1ª linha o que vão fazer.
- **Higiene de contexto**: passo 6 (Registrar) sugere `/clear` + `/loopteam:start` quando a sessão já está longa — estado vive 100% em arquivo.
- **Prova honesta**: linha de prova na Entrega declara o tipo (`executada` vs `estática`); prova estática nunca fecha `[x]`, no máximo `[?]` bloqueado-por-ambiente.

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

# Changelog

## v0.6.1

Campo rodada 2, patch — 3 achados:

- **Seções REFERÊNCIA** (G1): `templates/BRIEFING.md` ganha seção opcional `## REFERÊNCIA: <tema>` — material de contexto colado pelo dono (specs de outro projeto, contrato de API, modelo a replicar). Itens podem citar pelo título; Lead trata como fonte de verdade DESCRITIVA, nunca supõe acesso ao original; referência nunca é item, nunca ganha estado. `/briefing` oferece em 1 linha mover material colado pra essa seção.
- **Item composto se divide** (G2): `/briefing` divide item proposto que mistura entregável executável + decisão humana — parte executável vira item normal, parte de decisão vai pra "Decisões Pendentes". `run`/`start` fazem o mesmo pra item composto achado em BRIEFING legado: executam só a parte executável, decisão vira pendência no relatório — nunca fecham `[x]` engolindo a decisão.
- **Prova ecoa o critério** (G3): `loopteam-core` passo 6 + Entrega exigem que a prova referencie o critério específico do item, não genérico ("build ok"). "build+tsc limpos" só aceitável quando o critério É de tipo/compilação. Item legado sem critério: Lead define no Mapear, registra em memória, prova ecoa esse critério. `run`, Cluster: agente com prova genérica repetida em ≥3 itens → Lead devolve pro agente reverificar item a item antes de consolidar.

## v0.6.0

Campo rodada 2 — run inventou escopo estrutural sem regra ("partição por cluster") e `verify-signature` virou 8% do custo do plugin:

- **Modo cluster oficial** (E1): lote `/loopteam:run` com >8 itens abertos pode particionar em até 3 agentes de cluster (teto duro), agrupados por área de código. Prova sempre por item, nunca por cluster. Cadeia `dep:` nunca cruza cluster. Item de decisão humana fica fora de qualquer cluster. Lead consolida e confirma antes de marcar estado no BRIEFING.
- **verify-signature barata** (E2): carrega 1x por run (nunca por item/subagente); Lead extrai resumo operacional ≤10 linhas de `docs/SIGNATURE.md` e injeta nos agentes; skill completa só relida 1x, na consolidação final. `start`/ad-hoc continuam carregando inteira (item único não justifica compressão).
- **Decisões Pendentes** (E3): item cujo entregável é decisão do dono ("reavaliar X", "decidir se Y") nunca entra em "Escopo" — vai pra nova seção "Decisões Pendentes" do BRIEFING, listada (não executada) em todo `run`/`start` até o dono resolver.
- **Higiene reforçada** (E4): relatório final do `run` SEMPRE recomenda `/clear` quando o lote teve >5 itens — não mais condicional a "contexto pesado".

## v0.5.1

Patch — auditoria externa achou 2 vazamentos de pergunta:

- `/briefing`: teto duro de 2 interações (ideal 1). Resposta vaga/incompleta nunca gera follow-up — vira "(assumido)" no checklist, corrigido na aprovação. Atalho `/briefing <descrição>`: contexto suficiente em `$ARGUMENTS` pula as perguntas direto pra proposta.
- `loopteam-core` passo 1: empate real de roteamento em modo run não pergunta mais — escolhe o mais específico (ordem alfabética se empatar de novo), registra "roteado: X (empate assumido)" pro relatório final, veto via "reroteia: Y". Regra "Perguntar" reescrita literal: em run, só destrutivo executor e SIGNATURE global param; empate e 3ª falha não param mais o loop.
- README: comando `/briefing <descrição>` na tabela; nota sobre respostas vagas não gerarem pergunta em "Primeiros 5 minutos".

## v0.5.0

Modo RUN (one-shot) — feedback de campo: v0.4 segura mas friccionada, pergunta demais item por item. Concentra toda decisão humana na aprovação do briefing; travas C1/C2/C5 continuam, mas viram "resolver antes ou pular e reportar" em vez de "perguntar na hora":

- Novo `/loopteam:run`: lê o BRIEFING uma vez, executa TODOS os itens `[ ]` em sequência sem parar entre eles. Escopo extra vira sugestão anotada (não pergunta), critério inverificável vira `[?]` e segue, 3ª falha vira `[!]` e segue. Únicas paradas duras: trava destrutiva de extensão `executor` e conflito global de SIGNATURE. Relatório único no fim + "Aprovar tudo, revisar item N, ou descartar item N?". Retomável por design — `/clear` + `/loopteam:run` continua de onde parou.
- `/briefing` vira a única sessão de perguntas: as 4 perguntas da IDEIA agrupadas numa mensagem; novo campo opcional `auto:` (decisão pré-tomada) e checagem de ambiente no próprio briefing (`requer:` no item, não mais no run).
- `/loopteam:start` renomeia semanticamente para modo supervisionado (um item, para pra aprovar) — comportamento igual, agora é a alternativa deliberada ao `run`, inclusive pra retomar item `[!]`.
- `loopteam-core`: regra única de "Perguntar" substitui perguntas espalhadas — só interrompe por destrutivo em dados/dinheiro, conflito global de SIGNATURE, ou 3ª falha em modo supervisionado. Novo estado `[!]` (falhou 3x, escalado) no template do BRIEFING.

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

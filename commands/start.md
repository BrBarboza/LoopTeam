---
description: Modo supervisionado — executa UM item e para pra aprovação. Pra rodar todos os itens de uma vez, use /loopteam:run (padrão).
disable-model-invocation: true
argument-hint: "[item ou vazio]"
---

**Gate (1ª linha, antes de tudo)**: leia `.loopteam/state`. `off` → responda exatamente "loopteam off — use `/loopteam:on`" e PARE, nada mais roda. Ausente/`on` → declare em 1 linha o que este comando vai fazer (ex.: "Lendo docs/BRIEFING.md — N itens abertos, começando por...") e segue. Cobertura dupla com o gate de `loopteam-core` — não pule por já ter sido checado antes.

Carregue `loopteam-core`, `adapters`, `tool-capture` (e `verify-signature` se houver UI no escopo).

## Pendências primeiro

Seção "Decisões Pendentes" do BRIEFING não vazia → 1 linha "decisões pendentes: N (edite o item ou remova pra liberar)" — nunca vira item executável sozinho. Sincronize `.loopteam/pending` com os itens `[?]` do BRIEFING (adiciona novo com contagem 1, incrementa existente, remove os que viraram `[x]`/`[ ]`). Se algum item tem contagem ≥3: liste ANTES de qualquer outra coisa, formato "[?] <item> (há N sessões)" — sem reverter `[?]` sozinho, nunca. Contagem <3: liste depois do bloco abaixo, mesma linha só com "pendente: N item(ns) [?]".

## Checks (gate já feito acima + fase, ≤3 linhas somadas)

**Fase do projeto** (classifique 1 linha interna, nunca no chat, pelos sinais: nº arquivos de código, presença de teste, `.loopteam/` já existe, % `[x]` no BRIEFING):
- **INÍCIO** (repo vazio/scaffold): sem `docs/BRIEFING.md` ainda → não ofereça só "criar"; mostre o mini-mapa: "1) `/briefing` cria o escopo (só escreve o arquivo, não executa nada). 2) `/loopteam:run` lê o BRIEFING e executa todos os itens de uma vez (padrão); `/loopteam:start` faz um item por vez, supervisionado. 3) `/loopteam:on`/`off` liga/desliga, `/loopteam <tarefa>` roda algo avulso." e instrua a rodar `/briefing` agora. Primeiro contato nunca termina em beco. Sem fan-out até existir base real.
- **MEIO** (código real, sem LoopTeam prévio): rode `tool-capture` (aprovação 1-a-1 se `executor`, lote se `consultant`, ver `tool-capture`). Mapeie via `adapters` e proponha `docs/BRIEFING.md` rascunhado do código + entrevista curta de `briefing` pro que falta. UI presente → ofereça capturar `docs/SIGNATURE.md`, só grava com aprovação.
- **FIM** (código maduro, `.loopteam/` já existia, ou BRIEFING quase todo `[x]`): manutenção — fan-out raro, verificação pesada.

Cada extensão em `.loopteam/extensions/<tool>/` com `template_version` diferente do `_TEMPLATE` atual do plugin → avise em 1 linha ("extensão <tool> desatualizada: v<X> vs v<Y> do template"), sem re-rodar nada sozinho. Conte `approved: false` em `.loopteam/extensions/*/SKILL.md` → 1 linha ("N extensão(ões) aguardando aprovação").

## Selecionar item

Argumento (`$ARGUMENTS`) → trate como o item; sem match exato, assuma o mais próximo em 1 linha. Argumento casando item `[!]` → retoma dali, contagem de falha reinicia (passo 2 em diante). Sem argumento: se algum item `[ ]` tem `dep:`, ordene por dependência (item só entra depois do seu `dep:` estar `[x]`) e declare "ordem: dep" em 1 linha; nenhum item com `dep:` → heurística (bloqueador citado em Notas, senão 1º `[ ]` do arquivo) e declare "ordem: heurística". Tudo `[x]`/`[?]`/`[!]` sem alvo específico → diga "BRIEFING completo" (+ pendências já listadas acima, `[!]` incluso), pare.

## Executar

Roteiro completo de `loopteam-core` (Memória → Mapear → Executar por porte → Julgar → Verificar → Falha se houver → Registrar), formato de ENTREGA de lá.

## Fim

Só "Aprovar, ajustar ou descartar?". Sugestão de adapter (1 linha, só se fizer diferença real) vai no fim, nunca antes.

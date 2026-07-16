---
description: Modo padrão — lê o BRIEFING e executa TODOS os itens abertos em sequência (ou em cluster, se o lote for grande), sem parar entre eles. Relatório único no fim.
disable-model-invocation: true
argument-hint: "[vazio]"
---

**Gate (1ª linha)**: leia `.loopteam/state`. `off` → "loopteam off — use `/loopteam:on`", PARE. Ausente/`on` → declare em 1 linha o que vai fazer ("Lendo docs/BRIEFING.md — N itens abertos, rodando em sequência...") e segue.

Carregue `loopteam-core` em **modo run**, `adapters`, `tool-capture`. Sem `docs/BRIEFING.md` ainda → mesmo mini-mapa do `/loopteam:start` (3 frases) + instrua `/briefing`. Seção "Decisões Pendentes" do BRIEFING não vazia → 1 linha "decisões pendentes: N (fora do lote, ver relatório final)" — esses itens nunca entram no lote, nem em cluster.

## UI barata (verify-signature)

Há item de UI no lote → carregue `verify-signature` UMA VEZ aqui, nunca por item, nunca dentro de subagente/cluster: extraia de `docs/SIGNATURE.md` um resumo operacional ≤10 linhas (regras verificáveis) e injete esse resumo em cada item/agente que toca UI. A skill inteira só é relida no fim, numa única auditoria de consolidação sobre todas as telas tocadas no lote.

## Ordem

Itens com `dep:` primeiro, na ordem de dependência (item só entra depois do seu `dep:` estar `[x]`); sem `dep:` em nenhum, heurística (bloqueador em Notas, senão ordem do arquivo). Só itens `[ ]` entram no lote — `[x]`/`[?]`/`[!]`/decisão pendente ficam de fora, exceto se citados em `$ARGUMENTS` como retomada explícita.

## Cluster (lote com >8 itens abertos)

Lote > 8 itens → Lead PODE particionar em até 3 agentes de cluster (teto duro 3, nunca 4+ mesmo com mais itens), agrupados por área de código. Regras invioláveis:
- Cada agente recebe, por item do seu cluster, o texto VERBATIM + critério (+ resumo de SIGNATURE se UI); dentro do cluster roda o roteiro do core item a item (Memória → Mapear → Executar → Verificar), mesmas travas de modo run já embutidas no core.
- Prova é POR ITEM, nunca por cluster — "build ok" do cluster inteiro não fecha nada; cada item roda e reporta seu próprio critério.
- Itens ligados pela mesma cadeia `dep:` nunca se separam em clusters diferentes — vão juntos, na ordem da cadeia.
- Item de decisão humana (verbo tipo "reavaliar"/"decidir"/"planejar" sem entregável executável) não entra em cluster nenhum — fica fora do lote, vira pergunta agrupada no relatório final.
- Spawn concorrente dos agentes de cluster, aguardar todos, nunca poll. Lead consolida: revisa o reporte item a item de cada cluster antes de marcar qualquer estado no BRIEFING — agente propõe, Lead confirma. Reporte de agente com prova genérica repetida em ≥3 itens (ex.: "build+tsc limpos" colada em todo item do cluster, sem ecoar o critério de cada um) = Lead NÃO consolida — devolve o cluster inteiro pro agente reverificar item a item, prova ecoando o critério específico de cada um, antes de marcar qualquer estado.

Lote ≤ 8 itens → sem cluster, loop sequencial normal (seção abaixo).

## Loop (sem cluster, sem parar entre itens)

Para cada item: roteiro completo de `loopteam-core` (Memória → Mapear → Executar → Julgar → Verificar → Falha), regras de modo run já embutidas lá — escopo extra vira sugestão anotada, critério inverificável vira `[?]` e segue, 3ª falha vira `[!]` e segue, `mode: consultant` gera guia e marca `[?]`. As DUAS únicas paradas duras: trava destrutiva de extensão `executor` (mostra verbatim, espera OK) e conflito global de SIGNATURE (para, 3 linhas, espera resposta) — resolvida a parada, o loop retoma do próximo item.

Item composto em BRIEFING legado (escrito à mão, mistura entregável executável + decisão humana no mesmo texto) → executa SÓ a parte executável, com prova própria; a parte de decisão vira linha em "Decisões Pendentes" do relatório final, nunca vira `[x]` junto — fechar `[x]` engolindo a parte de decisão é proibido.

## Relatório final (única saída no chat)

Tabela 1 linha por item (POR ITEM mesmo em modo cluster, nunca por cluster): `estado | prova (tipo) | nota`. Depois, só se houver: suposições assumidas, sugestões de escopo extra anotadas, itens de decisão humana como pergunta agrupada, pendências `[?]`/`[!]` com o que falta. Lote teve >5 itens → SEMPRE, sem condição, a linha: "Sessão pesada — recomendo `/clear` e `/loopteam:run` de novo (retoma exatamente de onde parou, estado é 100% arquivo)." Fim: "Aprovar tudo, revisar item N, ou descartar item N?".

---
description: Mapa mental do LoopTeam em 3 frases + tabela dos comandos.
disable-model-invocation: true
---

Mostrando o mapa do LoopTeam.

1. `/briefing` cria/edita o escopo — só escreve `docs/BRIEFING.md`, não executa nada. Única sessão de perguntas.
2. `/loopteam:run` lê o BRIEFING e executa TODOS os itens de uma vez — modo padrão, relatório único no fim.
3. O resto é opcional: `/loopteam:start` faz um item por vez, supervisionado; `on`/`off` liga/desliga; `/loopteam <tarefa>` roda algo avulso.

| Comando | Faz |
|---|---|
| `/briefing` | Cria/continua `docs/BRIEFING.md` — proposta, só executa após "aprovar briefing". |
| `/loopteam:run` | Executa todos os itens abertos em lote, sem parar entre eles. Modo padrão. |
| `/loopteam:start` | Executa um item e para pra aprovação. Supervisão de perto, retomada de `[!]`. |
| `/loopteam:on` | Ativa o LoopTeam neste projeto. |
| `/loopteam:off` | Desativa — Claude age normal até o próximo `on`. |
| `/loopteam <tarefa>` | Roda UMA tarefa avulsa pelo roteiro completo, sem tocar o BRIEFING. |

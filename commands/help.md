---
description: Mapa mental do LoopTeam em 3 frases + tabela dos 5 comandos.
disable-model-invocation: true
---

Mostrando o mapa do LoopTeam.

1. `/briefing` cria/edita o escopo — só escreve `docs/BRIEFING.md`, não executa nada.
2. `/loopteam:start` lê o BRIEFING e trabalha os itens, um de cada vez.
3. O resto é opcional: `on`/`off` liga/desliga o time; `/loopteam <tarefa>` roda algo avulso sem tocar o BRIEFING.

| Comando | Faz |
|---|---|
| `/briefing` | Cria/continua `docs/BRIEFING.md` — proposta, só executa após "aprovar briefing". |
| `/loopteam:start` | Ponto de entrada — gate, fase do projeto, executa o item de maior dependência. |
| `/loopteam:on` | Ativa o LoopTeam neste projeto. |
| `/loopteam:off` | Desativa — Claude age normal até o próximo `on`. |
| `/loopteam <tarefa>` | Roda UMA tarefa avulsa pelo roteiro completo, sem tocar o BRIEFING. |

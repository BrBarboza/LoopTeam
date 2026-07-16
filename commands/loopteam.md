---
description: Roda UMA tarefa avulsa pelo roteiro completo do time (rotear → executar por porte → verificar → entregar), sem tocar o BRIEFING.
disable-model-invocation: true
argument-hint: "<tarefa>"
---

**Gate (1ª linha, antes de tudo)**: leia `.loopteam/state`. `off` → responda exatamente "loopteam off — use `/loopteam:on`" e PARE, nada mais roda, nem ad-hoc. Ausente/`on` → segue. Cobertura dupla com o gate de `loopteam-core`.

Carregue `loopteam-core` e `adapters`; carregue `tool-capture`/liste `.loopteam/extensions/` só se `$ARGUMENTS` casar prefixo/termo de alguma extensão já capturada.

Trate `$ARGUMENTS` como o item único. Rode o roteiro completo de `loopteam-core` a partir do passo 1 (Mapear) — pule o passo 0 (memória de BRIEFING) só se não houver `.loopteam/memory/DECISIONS.md` relevante, e pule a parte de marcar `[x]`/`[?]` no BRIEFING do passo 6 (não existe item de BRIEFING aqui). Registre a decisão/falha em `.loopteam/memory/DECISIONS.md` normalmente.

Entrega no formato padrão (código → prova → próximo passo, sem linha de `[x]`). Fim: só "Aprovar, ajustar ou descartar?".

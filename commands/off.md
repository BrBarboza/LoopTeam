---
description: Desliga o LoopTeam neste projeto — Claude volta a agir normal até /loopteam:on.
disable-model-invocation: true
---

Grave `off` em `.loopteam/state` (crie o diretório/arquivo se não existir). Confirme em 1 linha: "LoopTeam dormente — `/loopteam:on` pra reativar." A partir daqui, `loopteam-core` não roda até o próximo `on`; use `/loopteam <tarefa>` se quiser a lógica do time só uma vez, ad-hoc.

---
name: verify-signature
description: Verifica UI contra docs/SIGNATURE.md. Carregar quando o item tocar tela, componente visual, layout, tom de UI.
---

`docs/SIGNATURE.md` = padrão visual do dono. 2ª fonte de verdade: BRIEFING > SIGNATURE > código existente.

**Qualidade de design não é papel desta skill.** Se plugin de design (frontend-design, ui-ux-pro-max, etc.) estiver instalado, ele julga estética/UX — esta skill audita só CONFORMIDADE com `docs/SIGNATURE.md`: cor, tipografia, espaçamento, componente padrão, tom de voz batem com o que está escrito lá.

## Custo: carregar 1x por run, nunca por item

Em `/loopteam:run` (com ou sem cluster): esta skill carrega UMA vez, no Lead, no início do run — nunca dentro de subagente, nunca recarregada item a item. Nesse carregamento único, extraia de `docs/SIGNATURE.md` um RESUMO OPERACIONAL de ≤10 linhas (só regras verificáveis: cor, componente, posição, tom — nada de prosa) e injete esse resumo em cada item/agente que toca UI. A skill completa só é relida depois, uma única vez, na consolidação final do run — auditoria única sobre todas as telas tocadas no lote, não uma auditoria por item. Em `/loopteam:start`/`/loopteam <tarefa>` (item único): carrega inteira normalmente, sem resumo — custo de 1 item só não justifica a compressão.

## Passos (modo completo — start/ad-hoc, ou consolidação final do run)

1. `SIGNATURE.md` ausente → nada a auditar, siga convenção já no código, não bloqueie.
2. Existe → leia inteiro, extraia regra aplicável ao item antes de gerar código.
3. Gere já respeitando as regras — não gera e ajusta depois.
4. Checagem silenciosa pós-geração: bate com cada regra extraída (ou com o resumo, se veio injetado)? Sim → segue entrega normal.
5. Diverge pontual (1 tela foge por pedido do BRIEFING): BRIEFING vence, declare exceção em 1 linha na entrega, registre em `.loopteam/memory/DECISIONS.md`.
6. Diverge global (mudaria identidade visual inteira): PARE, pergunte em 3 linhas — conflito, 2 opções, recomendação. Trava dura mesmo em modo run.
7. `SIGNATURE.md` só muda com aprovação explícita do dono — nunca como efeito colateral de item.

## Critério de pronto

Critério mensurável do item + zero divergência de `SIGNATURE.md` sem exceção declarada.

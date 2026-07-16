# BRIEFING

Fonte de verdade do escopo deste projeto. O LoopTeam trabalha item por item. Nunca remover um item. Marcadores: `[ ]` aberto, `[x]` concluído e verificado, `[?]` aguardando o dono (guia manual de `mode: consultant`, ou critério com ambiente faltando, sem timeout), `[!]` falhou 3x e foi escalado — retome com `/loopteam:start`.

## Sobre o projeto

<!-- 1-2 frases: o que é o produto/projeto e o que ele faz. -->

## Escopo

<!--
Formato de cada item:
- [ ] <descrição do item> — pronto quando: <critério mensurável>

Prefixo opcional de extensão quando o item é de domínio capturado em .loopteam/extensions/<tool>/:
- [ ] [supabase] <item> — pronto quando: <critério>
- [ ] [n8n] <item> — pronto quando: <critério>

Campo opcional `dep:` — texto exato de outro item do qual este depende.
/loopteam:run e /loopteam:start ordenam por dep quando presente; sem dep em
nenhum item, cai pra heurística (bloqueador citado em Notas, ou 1º [ ] do arquivo).
- [ ] <item> — pronto quando: <critério> — dep: <texto do item do qual depende>

Campo opcional `auto:` — decisão que o /briefing já antecipou pra não perguntar
durante o run (ex.: "auto: usar shadcn pros componentes").
- [ ] <item> — pronto quando: <critério> — auto: <decisão pré-tomada>

Item cujo critério depende de ambiente ainda ausente nasce marcado `requer:`
(checado no /briefing, não no run) — o dono decide na aprovação se resolve
antes ou deixa pra fechar como [?] bloqueado-por-ambiente.
- [ ] <item> — pronto quando: <critério> — requer: <conexão/credencial/comando X>

Critério de pronto tem que ser mensurável: um teste que passa, um build sem erro,
um comando que retorna X, um endpoint que responde Y. Nunca "funciona bem".
-->

- [ ] <item de exemplo — apague ao preencher> — pronto quando: <critério mensurável>

## Fora de escopo

<!-- O que explicitamente NÃO entra nesta v1/milestone — resposta da 4ª pergunta de /briefing. -->

## Notas

<!-- Decisões de escopo que não cabem em 1 linha de item, mas que o time precisa saber. Ex.: qual item é bloqueador/maior dependência pro /loopteam:start priorizar. -->

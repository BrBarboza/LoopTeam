# BRIEFING

Fonte de verdade do escopo deste projeto. O LoopTeam trabalha item por item. Nunca remover um item. Marcadores: `[ ]` aberto, `[x]` concluído e verificado, `[?]` aguardando o dono (guia manual de `mode: consultant`, ou critério com ambiente faltando, sem timeout), `[!]` falhou 3x e foi escalado — retome com `/loopteam:start`. Item cujo entregável é decisão do dono, não código, vive em "Decisões Pendentes", nunca em "Escopo" — nunca executa sozinho.

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

## REFERÊNCIA: <tema>

<!--
Opcional, quantas seções REFERÊNCIA: <tema> forem necessárias. Material de
contexto colado pelo dono (specs de outro projeto, contrato de API, modelo a
replicar) — cola aqui tal como veio, sem editar.

Regras: itens do Escopo podem citar a referência pelo título ("conforme
REFERÊNCIA: <tema>"). O Lead trata o conteúdo como fonte de verdade
DESCRITIVA — usa só o que está escrito aqui, nunca supõe acesso ao original
(arquivo, API, repo). Referência NUNCA é item, nunca ganha estado
[ ]/[x]/[?]/[!].
-->

## Decisões Pendentes

<!--
Item cujo entregável é uma decisão do dono, não código ("reavaliar tela X",
"decidir se Y", "planejar Z" sem forma executável) NUNCA entra em "Escopo" —
entra aqui. /loopteam:run e /loopteam:start listam esta seção (se não vazia)
sem executar nada dela. Resolver = o dono edita o item pra forma executável
(com critério mensurável, aí ele migra pra "Escopo") ou remove a linha.
- [ ] <decisão a tomar>
-->

## Fora de escopo

<!-- O que explicitamente NÃO entra nesta v1/milestone — resposta da 4ª pergunta de /briefing. -->

## Notas

<!-- Decisões de escopo que não cabem em 1 linha de item, mas que o time precisa saber. Ex.: qual item é bloqueador/maior dependência pro /loopteam:start priorizar. -->

---
name: _TEMPLATE
description: "TEMPLATE — não usar direto. Copie esta pasta para extensions/<seu-dominio>/ e preencha: (1) quando carregar — prefixo de item do BRIEFING (ex. [meudominio]) e/ou termos semânticos que o Lead deve casar contra a description; (2) mode: executor ou consultant."
mode: executor
template_version: "1.0"
approved: false
---

<!--
CONTRATO que toda extensão de domínio do LoopTeam segue. Não é skill ativa do
plugin — é o molde que a skill `tool-capture` usa pra gerar extensão de verdade
em .loopteam/extensions/<tool>/SKILL.md, dentro do PROJETO do dono (não aqui).
Comunidade/dono também pode preencher isso à mão pro mesmo destino.

`loopteam-core` lê o arquivo gerado via Read ao rotear (passo 1) — não precisa
registrar em .claude-plugin/plugin.json nem em skills/, funciona sem reload.
Ver exemplos de output em examples/captured/ (supabase, n8n).

`template_version` no frontmatter acima é DESTE contrato — copie o valor atual
pra extensão gerada. /loopteam:start compara e avisa em 1 linha se desatualizada.
`approved` nasce `false`; só o dono (via OK de tool-capture) vira `true`. Sem
isso, loopteam-core ignora a extensão ao rotear.

Preencha as 3 seções abaixo, apague comentários e placeholders <...>.
-->

# Extensão: <nome do domínio>

`mode: executor` — esta extensão executa mudanças reais (código, infraestrutura,
configuração) dentro do seu domínio. Use `mode: consultant` em vez disso se a
extensão só deve orientar o dono a aplicar a mudança em outro lugar (ex.: um
editor externo que a extensão não tem permissão de escrever).

## Quando o Lead roteia para cá

- Prefixo explícito no item do BRIEFING: `[<seu-dominio>]`.
- Sem prefixo: quando o item casar semanticamente com esta `description` — o
  Lead declara "roteado: <seu-dominio>" em 1 linha na entrega; o dono pode vetar
  com "reroteia: <outra>".

## Trava destrutiva

<!--
Liste aqui, em bullets objetivos, o que esta extensão NUNCA faz sem aprovação
explícita do dono antes de agir — a "trava destrutiva" do domínio. Exemplos de
formato (veja examples/captured/supabase-executor e examples/captured/n8n-consultant
para exemplos reais preenchidos):
- Nunca <ação irreversível ou de alto risco> sem mostrar <o que muda> e pedir aprovação.
- Nunca <expor segredo/credencial> em <lugar indevido>.
-->

- <regra invioável 1>
- <regra invioável 2>

## Critério de verificação

<!--
Como esta extensão prova que o item está pronto — precisa ser quantitativo,
igual ao critério de pronto do loopteam-core (passo 4: Loop de verificação).
Ex.: "migration aplicada + query de smoke test retorna linha esperada",
"workflow testado manualmente retorna payload X".
-->

<critério mensurável específico do domínio>

## Formato de entrega

<!--
Se mode: executor — normalmente segue o formato padrão de ENTREGA do
loopteam-core (código + prova + [x] + roteado + próximo passo), com o que for
específico do domínio (ex.: SQL a aprovar) incluído na prova.

Se mode: consultant — este é o único lugar, junto com skills/loopteam-core/SKILL.md,
onde a REGRA DE SILÊNCIO abre exceção: o guia pode ser didático e longo, porque
o dono precisa executar a ação manualmente fora do Claude Code. Estruture o
guia em passos numerados, com valores exatos, e feche com o teste manual que
o dono deve rodar para confirmar.
-->

<formato de entrega específico do domínio>

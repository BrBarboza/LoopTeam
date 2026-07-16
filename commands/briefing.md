---
description: Cria ou continua docs/BRIEFING.md — entrevista mínima sobre a IDEIA, o Lead monta o resto sozinho.
disable-model-invocation: true
argument-hint: "[texto livre de escopo novo, ou vazio]"
---

Vou montar/continuar o BRIEFING com você — nada será executado agora.

Única skill de domínio onde a REGRA DE SILÊNCIO abre exceção — aqui ainda não há código pra entregar, só escopo pra fechar.

## Sem `docs/BRIEFING.md` ainda

Entrevista socrática MÍNIMA: até 4 perguntas sobre a IDEIA, AGRUPADAS numa única mensagem (não uma por vez):
1. O quê (o que é o projeto, 1-2 frases).
2. Pra quem (quem usa/se beneficia).
3. Pronto = o quê (o que precisa existir/rodar pro dono considerar a v1 feita).
4. Fora de escopo (o que explicitamente NÃO entra agora).

Com a IDEIA fechada, monte sozinho o resto, sem novas perguntas: copie `templates/BRIEFING.md`, escreva os itens como `- [ ] <item> — pronto quando: <critério mensurável>`, use prefixo `[tool]` se `tool-capture` já tiver gerado extensão pro domínio, preencha a seção "Fora de escopo" com a resposta 4. Item sem critério mensurável embutido no texto NÃO entra no checklist ("Desenhar schema real" é inválido; "migration das tabelas A,B,C aplicada + policies testadas" é válido) — reescreva ou devolva pro dono antes de incluir. Pra cada item, resolva JÁ o que causaria pergunta no `/loopteam:run`: `dep:` se houver, prefixo de extensão se houver, e `auto: <decisão pré-tomada>` quando há escolha óbvia a antecipar. Cheque se o critério é executável hoje (conexão/comando/credencial existe); se não, marque `requer: <o que falta>` no próprio item — o dono decide na aprovação se resolve antes ou deixa `[?]`. Apresente tudo em bloco único e peça aprovação — não pergunte item por item.

## Já existe, com `$ARGUMENTS` (escopo novo)

Leia o BRIEFING inteiro primeiro. Nunca remova/reescreva item existente. Traduza `$ARGUMENTS` em item(ns) novo(s) no mesmo formato, acrescente ao final. Conflito com item já `[x]`: pontual → aponte em 1 linha e siga; identidade visual inteira → pare, 3 linhas. Confirme em 1-2 linhas o que foi adicionado.

## Já existe, sem argumento ("continua o briefing")

Leia inteiro. Pergunte objetivamente: escopo novo ou só revisar aberto? Revisão: liste `[ ]`/`[?]` abertos e sugira `/loopteam:start`.

## Regras

Nunca remover item. Nunca critério vago ("funciona bem") — proponha mensurável e confirme. Nunca codar aqui — fim é sempre BRIEFING atualizado + sugestão de `/loopteam:start`.

O checklist que você monta aqui é PROPOSTA. Nada executa — nenhum `/loopteam:run`, nenhum `/loopteam:start`, nenhum código — antes do dono aprovar o BRIEFING inteiro, dizendo exatamente "aprovar briefing". Aprovado, libera o lote inteiro via `/loopteam:run`.

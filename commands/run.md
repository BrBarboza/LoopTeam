---
description: Modo padrão — lê o BRIEFING e executa TODOS os itens abertos em sequência, sem parar entre eles. Relatório único no fim.
disable-model-invocation: true
argument-hint: "[vazio]"
---

**Gate (1ª linha)**: leia `.loopteam/state`. `off` → "loopteam off — use `/loopteam:on`", PARE. Ausente/`on` → declare em 1 linha o que vai fazer ("Lendo docs/BRIEFING.md — N itens abertos, rodando em sequência...") e segue.

Carregue `loopteam-core` em **modo run**, `adapters`, `tool-capture` (e `verify-signature` se houver UI no escopo).

Sem `docs/BRIEFING.md` ainda → mesmo mini-mapa do `/loopteam:start` (3 frases) + instrua `/briefing`. Nada de rodar sem BRIEFING aprovado.

## Ordem

Itens com `dep:` primeiro, na ordem de dependência (item só entra depois do seu `dep:` estar `[x]`); sem `dep:` em nenhum, heurística (bloqueador em Notas, senão ordem do arquivo). Só itens `[ ]` entram no lote — `[x]`/`[?]`/`[!]` ficam de fora, exceto se citados em `$ARGUMENTS` como retomada explícita.

## Loop (sem parar entre itens)

Para cada item: roteiro completo de `loopteam-core` (Memória → Mapear → Executar → Julgar → Verificar → Falha), regras de modo run já embutidas lá — escopo extra vira sugestão anotada, critério inverificável vira `[?]` e segue, 3ª falha vira `[!]` e segue, `mode: consultant` gera guia e marca `[?]`. As DUAS únicas paradas duras: trava destrutiva de extensão `executor` (mostra verbatim, espera OK) e conflito global de SIGNATURE (para, 3 linhas, espera resposta) — resolvida a parada, o loop retoma do próximo item. A cada 2-3 itens fechados, se o contexto já está pesado: não pare, só marque o aviso pro relatório final.

## Relatório final (única saída no chat)

Tabela 1 linha por item: `estado | prova (tipo) | nota`. Depois, só se houver: suposições assumidas (registradas na memória), sugestões de escopo extra anotadas, pendências `[?]`/`[!]` com o que falta. Se o aviso de sessão pesada foi marcado: "Sessão pesada — recomendo `/clear` e `/loopteam:run` de novo (retoma exatamente de onde parou, estado é 100% arquivo)." Fim: "Aprovar tudo, revisar item N, ou descartar item N?".

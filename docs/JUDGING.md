# JUDGING — rubrica de auto-avaliação do LoopTeam

Usada ao final de mudanças estruturais no plugin (não por item de BRIEFING de um projeto que usa o LoopTeam — essa rubrica avalia o LoopTeam em si). Escala 0-10 por critério, nota final = média ponderada pelos pesos abaixo.

## Critérios e pesos

| Critério | Peso | O que mede |
|---|---|---|
| Tokens | 2x | Quanto do trabalho acontece fora do chat (fan-out, arquivos, memória) vs. quanto vaza pra conversa. Nota alta = deliberação e execução silenciosas, chat só recebe código+prova. |
| Resultado | 1x | O item entregue realmente satisfaz o critério de pronto mensurável definido no passo 1 do `loopteam-core`. |
| Praticidade | 1x | Fricção real de usar no dia a dia: comandos claros, respostas do dono previsíveis (aprovar/ajustar/descartar/reroteia/funcionou/continua o briefing), sem passos manuais desnecessários. |
| Comunicação/entendimento | 1x | A entrega de ≤8 linhas dá ao dono o suficiente pra decidir sem precisar perguntar "o que você fez?". |
| Alucinação | 1x | Nenhuma afirmação de "pronto" sem prova quantitativa rodada; nenhum comando/API inventado; adapters ausentes tratados como ausentes, nunca simulados. |
| Portabilidade | 2x | Funciona 100% vanilla, sem nenhuma ferramenta externa instalada — upgrades são estritamente aditivos e opcionais. |
| Extensibilidade | 1x | Domínio novo é capturado (`tool-capture`) ou copiado de `extensions/_TEMPLATE/` sem tocar `loopteam-core`; núcleo não conhece nome de ferramenta nenhuma. |
| Time-to-first-loop | 1x | Do install até o primeiro item do BRIEFING entregue e verificado — quantos passos e perguntas isso exige. |

## Meta

- Média ponderada ≥ **8.8**.
- Tokens ≥ **9**.
- Portabilidade ≥ **9**.

Se qualquer meta não for atingida, o item de melhoria correspondente entra no BRIEFING do próprio repo LoopTeam antes de considerar a versão pronta.

## Como pontuar

Para cada critério, 1 linha de justificativa citando evidência concreta do repo (arquivo, comportamento, ou cenário simulado) — nunca uma nota sem justificativa. Uma nota 10 exige zero ressalva; qualquer ressalva desce a nota proporcionalmente (ressalva pequena → 8-9, ressalva média → 6-7, ressalva grande → abaixo de 6).

## Histórico (append-only, com teto)

Recalcular a cada mudança estrutural do plugin. Appendar tabela nova datada — nunca editar/apagar tabela de versão anterior. Teto: manter as últimas 5 versões com tabela completa; versão que sair da janela das 5 mais recentes comprime pra 1 linha só (data, versão, média ponderada), no topo da seção, em ordem cronológica.

### v1 — 2026-07-15

| Critério | Nota | Nota×peso |
|---|---|---|
| Tokens (2x) | 9 | 18 |
| Resultado (1x) | 8 | 8 |
| Praticidade (1x) | 9 | 9 |
| Comunicação (1x) | 8 | 8 |
| Alucinação (1x) | 9 | 9 |
| Portabilidade (2x) | 9 | 18 |
| Extensibilidade (1x) | 9 | 9 |
| Time-to-first-loop (1x) | 8 | 8 |

Média ponderada: 87/10 = **8.7**. Abaixo da meta por 0.1 — puxado por Resultado e Time-to-first-loop, que só sobem com uso real.

### v2 — 2026-07-15

Mudanças: instalação em 1 marketplace próprio (M1), núcleo desescopado (M4, extensões viram captura em `.loopteam/extensions/`), `loopteam-core` 97→44 linhas, fan-out variável 1-3, `[?]` sem timeout, `docs/JUDGING.md` vira log.

| Critério | Nota | Nota×peso |
|---|---|---|
| Tokens (2x) | 9.5 | 19 |
| Resultado (1x) | 8.5 | 8.5 |
| Praticidade (1x) | 9.5 | 9.5 |
| Comunicação (1x) | 8.5 | 8.5 |
| Alucinação (1x) | 8 | 8 |
| Portabilidade (2x) | 9.5 | 19 |
| Extensibilidade (1x) | 9.5 | 9.5 |
| Time-to-first-loop (1x) | 9 | 9 |

Média ponderada: 91/10 = **9.1** (+0.4 vs v1). Bate as 3 metas (média ≥8.8, tokens ≥9, portabilidade ≥9). Alucinação é o novo teto baixo: "item de maior dependência" em `/loopteam:start` é heurística sem grafo de dependência real — risco de ordenação errada declarada com confiança indevida.

### v3 — 2026-07-15

Mudanças: `dep:` explícito no BRIEFING (R1, "ordem: dep" vs "ordem: heurística" declarado), aprovação 1-a-1 com trava verbatim pra extensão `executor` (R2), `template_version` rastreado com aviso de desatualização (R3), pendência `[?]` com contador de sessões e abre a saída em N≥3 (R4), `docs/JUDGING.md` ganha teto de 5 versões completas (R5), gate `.loopteam/state` com cobertura dupla — `start.md`/`loopteam.md` checam na 1ª linha, sem exceção pra ad-hoc (R6), roteamento força `ls .loopteam/extensions/` antes de rotear, nunca de memória (R7).

| Critério | Nota | Nota×peso |
|---|---|---|
| Tokens (2x) | 9.3 | 18.6 |
| Resultado (1x) | 9 | 9 |
| Praticidade (1x) | 9.3 | 9.3 |
| Comunicação (1x) | 9 | 9 |
| Alucinação (1x) | 9 | 9 |
| Portabilidade (2x) | 9.5 | 19 |
| Extensibilidade (1x) | 9.5 | 9.5 |
| Time-to-first-loop (1x) | 9 | 9 |

Média ponderada: 92.4/10 = **9.2** (+0.1 vs v2). Bate as 3 metas. Alucinação sobe (8→9): `ls` obrigatório antes de rotear fecha o gap que v2 deixou aberto. Tokens cede um pouco (9.5→9.3): aprovação 1-a-1 de extensão `executor` custa mais turnos que o lote antigo — trade-off aceito pela segurança da trava verbatim.

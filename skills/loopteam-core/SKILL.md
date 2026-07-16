---
name: loopteam-core
description: Processo LoopTeam — item de BRIEFING vira código verificado. Fan-out variável, julgamento silencioso, loop de verificação. Carregar sempre que houver item de BRIEFING ou tarefa ad-hoc a executar.
---

Lead de time interno. Lead julga/roteia/revisa, nunca coda rotina. Devs = subagentes fan-out. Caro julga, barato executa.

**Gate**: ler `.loopteam/state`. `off` → dormente, aja normal, pare aqui — sem exceção, nem em ad-hoc (`/loopteam:start`, `/loopteam:run` e `/loopteam` também checam isso na própria primeira linha; dupla cobertura, nunca 1 leitor só). Ausente ou `on` → ativo.

**Silêncio**: deliberação e roteamento internos, nunca no chat. Chat = código + prova. Exceção: extensão `mode: consultant`.

**Fontes**: BRIEFING > SIGNATURE > código existente. Conflito pontual → BRIEFING vence, exceção 1 linha + memória. Conflito global (identidade visual inteira) → trava dura (ver Perguntar).

**Modo**: `/loopteam:start` e `/loopteam <tarefa>` = supervisionado (1 item, para pra "Aprovar, ajustar ou descartar?"). `/loopteam:run` = autônomo (todos os itens abertos em sequência, relatório único no fim — formato em `run.md`). Roteiro por item é o mesmo nos dois; o que muda é o que cada trava faz ao disparar, marcado abaixo.

**Perguntar** (regra única, substitui perguntas espalhadas): em modo run, SÓ 2 coisas param o loop pra perguntar — destrutivo em dados/dinheiro (trava de extensão `executor`) e conflito global de SIGNATURE; nada mais, nem empate de roteamento, nem 3ª falha. Em modo supervisionado, essas 2 mais a 3ª falha do item. Todo o resto, nos dois modos: decide, registra em `.loopteam/memory/DECISIONS.md`, segue.

## Roteiro por item

0. **Memória**: ler `.loopteam/memory/DECISIONS.md` por falha/decisão do mesmo tema.
1. **Mapear**: cite o texto VERBATIM do item — o contrato interno cobre SÓ esse texto. Contexto via skill `adapters` + contrato (escopo, arquivos, critério MENSURÁVEL). Adição estrutural fora do item (tabela nova, multi-tenant, trigger, tela extra) = escopo novo: supervisionado → 1 linha "isso exige X além do item; incluo? (s/n)" antes de codar; run → não pergunta, executa só o texto, registra a sugestão pro relatório final. Na dúvida entre mais ou menos, faça MENOS. Critério inexecutável no ambiente atual (conexão/comando/credencial ausente): supervisionado → para antes de codar, pergunta 2 linhas (o que falta / resolver ambiente ou dividir item); run → marca `[?]` bloqueado-por-ambiente com 1 linha do que falta, segue pro próximo item sem codar o resto. Antes de rotear: liste `.loopteam/extensions/` (`ls`), leia a `description` de cada `SKILL.md` — nunca roteie de memória. Ignore `SKILL.md` com `approved: false`. Prefixo `[tool]` ou termo semântico casando → ler o `SKILL.md` inteiro e seguir suas regras. Empate: especificidade decide (prefixo > termo exato > termo genérico); empate real: supervisionado → pergunta 1 linha; run → NÃO pergunta, escolhe a mais específica (empate de especificidade também → ordem alfabética), declara "roteado: X (empate assumido)" pro relatório final, dono veta depois com "reroteia: Y" (reabre só aquele item).
2. **Executar por porte**: 1-2 arquivos → edit direto, sem fan-out. 3+/feature/cross-module → fan-out de 1 a 3 subagentes nomeados pela incerteza real: 1 = caminho óbvio, 2 = trade-off claro, 3 = incerteza genuína. Spawn concorrente, aguardar todos, nunca poll.
3. **Julgar** (silencioso): compara, escolhe ou funde, aplica. Trade-off em ≤1 linha só se relevante pro dono decidir.
4. **Verificar**: roda critério do passo 1, quantitativo, até 5 tentativas. `mode: consultant`: sem execução própria — critério é a resposta do dono ("funcionou" = passa; qualquer outra coisa = falha). Item de UI: supervisionado → carregar `verify-signature` inteira aqui; run → usa só o resumo operacional já injetado por `run.md` (skill inteira não recarrega por item). Sem `docs/SIGNATURE.md`, pule sem avisar.
5. **Falha**: 1ª → 2ª melhor solução julgada. 2ª → abordagem NOVA, nunca reciclada. 3ª: supervisionado → PARA, escala 3 linhas (o que travou / o que foi tentado / 2 saídas com custo cada), trava dura, espera resposta; run → marca `[!]` travado com as mesmas 3 linhas na memória, segue pro próximo item. Sem 4ª tentativa sem aprovação explícita.
6. **Registrar**: prova executada e completa → `[x]`. `mode: consultant` sem "funcionou", ou prova estática com critério pendente de ambiente → `[?]` com o que falta em 1 linha — nunca `[x]`, nunca `[ ]`. 3ª falha → `[!]` (ver passo 5). A linha de prova DEVE referenciar (ecoar) o critério específico do item — "pipeline: 4 transições clicadas em dev, estado persiste", nunca genérico tipo "build ok". "build+tsc limpos" só é prova aceitável quando o critério do item É de tipo/compilação; qualquer outro critério exige prova que cite esse critério. Item de BRIEFING legado sem critério mensurável embutido no texto: o Lead define o critério já no passo 1 (Mapear), registra em `.loopteam/memory/DECISIONS.md`, e a prova aqui ecoa ESSE critério. Decisão/falha sempre em memória. Sessão pesada: supervisionado → 1 linha após o item sugerindo `/clear`; run → acumula o aviso pro relatório final (a cada 2-3 itens fechados).

## Roteamento

Prefixo é lei. Sem prefixo: classifique pela description da extensão capturada, declare "roteado: X" em 1 linha; dono veta com "reroteia: Y". Multi-domínio: todo `mode: executor` primeiro, `mode: consultant` por último.

## Design visual

Se plugin de design (ex.: frontend-design, ui-ux-pro-max) estiver instalado, delegue qualidade de design a ele — `verify-signature` audita só conformidade com `docs/SIGNATURE.md`, não qualidade geral.

## Estilo de saída

Fragmento, zero filler, zero preâmbulo. Código, comando, path, erro sempre exatos, nunca comprimidos. Plugin caveman instalado → não duplique regra de compressão, componha com ele.

## Entrega (modo supervisionado)

≤8 linhas antes do código: código → prova (1 linha, TIPO explícito + critério ecoado: "prova: executada (pipeline: 4 transições clicadas em dev, estado persiste)" ou "prova: estática (grep/leitura — critério real pendente de ambiente)") → `[x]`/`[?]`/`[!]` item → "roteado: X" (se semântico) → exceção signature (se houve) → próximo passo. Prova genérica ("build ok" sem citar o critério do item) não fecha nada, mesma trava de prova estática. Proibido: raciocínio do time, opções descartadas, narração de subagente. Modo run usa relatório único — formato em `run.md`, não este.

## Após entrega (modo supervisionado)

Só pergunte: "Aprovar, ajustar ou descartar?". Ajustar → reabre só passo 4. Descartar → `[ ]` + motivo na memória. Resposta é escopo novo → edite BRIEFING, confirme "continua o briefing".

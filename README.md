# LoopTeam

> **Think more, spend less.**

Um plugin de Claude Code que transforma sua sessão em um **time de desenvolvimento interno**: um Lead que julga, subagentes que implementam em paralelo, e loops de verificação que só fecham item com prova. Toda a deliberação acontece em silêncio — no chat você recebe **código + prova**, nada de "pensando alto".

Você decide **uma vez**, o time roda **de uma vez**, você revisa **uma vez**. Três toques por ciclo.

---

## Primeiros 5 minutos

```
# 1. Instalar (dentro do Claude Code, em qualquer projeto)
/plugin marketplace add {{seu-user}}/loopteam
/plugin install loopteam@loopteam

# 2. Definir o escopo (só escreve o arquivo — nada é executado)
/briefing
#    → responda até 4 perguntas (numa mensagem só)
#    → respostas vagas não geram nova pergunta — viram "(assumido)",
#      você corrige tudo de uma vez na aprovação
#    → revise o checklist proposto
#    → digite: aprovar briefing
# atalho de 1 passo: /briefing <descreva a ideia> pula as perguntas

# 3. Rodar tudo
/loopteam:run
#    → executa todos os itens em sequência, sem te interromper
#    → no fim: relatório de 1 linha por item

# 4. Revisar
#    "Aprovar tudo" · "revisar item N" · "descartar item N"
```

Perdido? `/loopteam:help` mostra o mapa em 3 frases.

---

## Os comandos

| Comando | O que faz | Quando usar |
|---|---|---|
| `/briefing` | Cria/edita `docs/BRIEFING.md` — a **única** sessão de perguntas. Nada executa aqui. | Início do projeto ou mudança de escopo |
| `/briefing <descrição>` | Mesma coisa, mas pula as perguntas se você já contar a ideia. | Já sabe o que quer, quer 1 passo só |
| `/loopteam:run` | Lê o BRIEFING e executa **todos** os itens abertos em lote, sem parar entre eles. **Modo padrão.** | Depois de "aprovar briefing" |
| `/loopteam:start` | Executa **um** item e para para aprovação. | Item delicado, retomada de `[!]`, supervisão de perto |
| `/loopteam <tarefa>` | Roda uma tarefa avulsa pelo processo completo, sem tocar no BRIEFING. | Pedido pontual fora do escopo |
| `/loopteam:on` / `off` | Liga/desliga o LoopTeam na pasta. `off` = Claude age normal. | Quando quiser a sessão "crua" |
| `/loopteam:help` | Mapa mental em 3 frases + esta tabela. | Sempre que esquecer o fluxo |

**A dúvida clássica:** `/briefing` **escreve** o escopo; `/loopteam:run` (ou `start`) é quem **lê e executa**.

---

## Como funciona

```
docs/BRIEFING.md ──► Lead lê UMA vez ──► por item:
   (checklist            │                 porte pequeno → edita direto
   aprovado)             │                 porte grande  → 1-3 subagentes concorrentes
                         │                 julga em silêncio → aplica a melhor
                         ▼                 verifica (critério mensurável, até 5x)
                  relatório final:        [x] provado · [?] aguarda você/ambiente · [!] travou 3x
                  1 linha por item
```

- **Caro julga, barato executa**: modelo forte só para decisão e review; rotina vai para o mais barato.
- **Fan-out por incerteza**: 1 subagente se o caminho é óbvio, 2 se há trade-off, 3 se há incerteza real. Nunca paralelismo por vaidade.
- **Prova honesta**: todo item declara o tipo de prova — `executada` (testes/build rodaram) ou `estática` (leitura/grep). Prova estática **nunca** fecha um item como `[x]`.
- **Escopo é lei**: o time executa o **texto** do item, nada além. Ideias extras viram sugestão no relatório, não código surpresa.
- **Só interrompe você por 2 motivos**: operação destrutiva (DROP, DELETE, dinheiro) ou conflito global com sua identidade visual. Todo o resto: decide, registra, segue.

### Estados dos itens

| Estado | Significa | Quem resolve |
|---|---|---|
| `[ ]` | Aberto | O próximo run |
| `[x]` | Fechado com prova executada | Ninguém — está pronto |
| `[?]` | Aguardando você (guia manual, ambiente faltando) | Você — o `start`/`run` lembra a cada sessão |
| `[!]` | Falhou 3 vezes, escalado com diagnóstico | Você decide a saída; retome com `/loopteam:start` |

---

## Captura de ferramentas (zero domínio hardcoded)

No primeiro run, o LoopTeam detecta o que **seu projeto** realmente usa — MCPs conectados, CLIs, dependências — e gera uma extensão de domínio para cada ferramenta relevante em `.loopteam/extensions/<tool>/`, com:

- `mode: executor` (o time opera direto — ex.: banco via MCP/migrations) ou `mode: consultant` (você opera manualmente — ex.: automações que você aplica num editor externo; o time entrega guia passo a passo);
- **trava destrutiva** do domínio (o que nunca roda sem seu OK);
- critério de verificação próprio.

Extensões `executor` são aprovadas **uma a uma**, com a trava exibida na íntegra. `consultant`, em lote. Extensão sem aprovação fica gravada mas **não roteia**. Exemplos reais do output esperado em `examples/captured/` (Supabase como executor, n8n como consultant).

## Assinatura visual (opcional)

Tem um padrão de UI que todo projeto seu segue (posição de FAB, nav bar, cores, raios)? Descreva uma vez em `docs/SIGNATURE.md` (template incluso — ou deixe o `/briefing` capturá-lo do código existente). Todo item que toca UI é verificado contra ele. Plugins de design instalados (frontend-design, ui-ux-pro-max) cuidam da qualidade geral; o LoopTeam audita **a sua** assinatura.

Precedência em conflito: `BRIEFING > SIGNATURE > código existente` — exceção pontual passa com 1 linha; mudança de identidade inteira para e pergunta.

---

## Sessão nova é barata (leia isto)

O estado do LoopTeam vive **100% em arquivos** (`docs/BRIEFING.md`, `.loopteam/`). A sessão é descartável:

- Contexto acumulado é o maior custo real de sessões longas — não a execução.
- A cada 2-3 itens fechados, o run avisa quando vale: `/clear` → `/loopteam:run` **continua exatamente de onde parou**.
- Regra de bolso: sessão nova por bloco de trabalho = modo barato de operar.

O plugin em si carrega ~46 linhas de núcleo + skills sob demanda — custo fixo mínimo por turno, medido e registrado em `docs/JUDGING.md` a cada versão.

---

## Estrutura que o LoopTeam cria no seu projeto

```
docs/BRIEFING.md          # escopo: checklist com critérios, dep:, estados
docs/SIGNATURE.md         # (opcional) seu padrão visual
.loopteam/
├── state                 # on/off
├── extensions/<tool>/    # extensões capturadas do SEU projeto
└── memory/DECISIONS.md   # decisões e falhas — nunca repete abordagem que já falhou
```

## FAQ

**O run fez algo que eu não queria.** Responda `revisar item N` ou `descartar item N` no relatório — descarte registra o motivo na memória e a abordagem não se repete. Escopo extra nunca vira código sem estar no BRIEFING.

**Por que ele não me mostra as alternativas que os subagentes geraram?** Você pagaria 3x o output para ler propostas que o Lead já julgou. O trade-off relevante (quando existe) vem em 1 linha; o resto está na memória.

**Quero acompanhar de perto um item específico.** `/loopteam:start` — um item, para, pergunta.

**Funciona sem nenhuma ferramenta extra instalada?** Sim — 100% vanilla. rtk, graphify, ruflo, caveman e afins são detectados como aceleradores se existirem, nunca exigidos.

**O BRIEFING mudou no meio do caminho.** Edite o arquivo, digite `continua o briefing` — itens novos entram na fila, `[x]` nunca some (o checklist é escopo, progresso e histórico).

---

MIT · Feito com a lógica de "pensar mais para gastar menos": deliberação interna densa, chat enxuto, prova sempre.

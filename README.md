# LoopTeam

**Think more, spend less.**

LoopTeam transforma qualquer sessão do Claude Code em um time de desenvolvimento interno: um Lead que julga, roteia e revisa; Devs que são subagentes concorrentes disparados em fan-out (1 a 3, conforme incerteza real); e extensões de domínio geradas a partir do que o SEU projeto realmente usa — nada de Supabase/n8n fixo no núcleo. O caro julga. O barato executa. O chat só vê código e prova.

Fonte de verdade em arquivo, não em memória de conversa: `docs/BRIEFING.md` é o escopo em checklist.

## Primeiros 5 minutos

```
/plugin marketplace add {{user}}/loopteam
/plugin install loopteam@loopteam
```

```
install → /briefing (até 4 perguntas sobre a IDEIA)
        → você responde
        → "aprovar briefing" (nada executa antes disso)
        → /loopteam:start (trabalha os itens, um por vez)
        → você responde: aprovar / ajustar / descartar
```

Perdido? `/loopteam:help` mostra o mapa em 3 frases. Detalhes técnicos ficam no resto deste README.

## Como funciona

Lead julga e roteia, nunca coda rotina. `docs/BRIEFING.md` é o escopo; cada item vira código verificado por um roteiro de 7 passos (memória → mapear → executar → julgar → verificar → falhar se preciso → registrar). Fan-out de 1-3 subagentes só quando o item é grande e a incerteza é real. Extensão de domínio (Supabase, n8n, o que seu projeto usar) é capturada do ambiente, nunca fixa no núcleo. Chat só recebe código e prova — o resto é silencioso.

## Fluxo

```
/loopteam:start
      │
      ▼
gate .loopteam/state ── "off"? dormente, pare. "on"/ausente? segue
      │
      ▼
fase do projeto: INÍCIO (sem base) / MEIO (código sem LoopTeam) / FIM (maduro)
      │
      ├─ INÍCIO ── entrevista /briefing (4 perguntas, só a IDEIA)
      ├─ MEIO   ── tool-capture (detecta MCP/CLI/deps → gera .loopteam/extensions/<tool>/SKILL.md,
      │            pede OK em lote) + rascunha BRIEFING a partir do código + entrevista curta
      └─ FIM    ── modo manutenção: fan-out raro, verificação pesada
      │
      ▼
lista pendências [?] em 1 linha, se houver
      │
      ▼
pega item de maior dependência (ou 1º [ ] aberto)
      │
      ▼
0.memória → 1.mapear (+ roteia extensão, empate por especificidade)
      │
      ▼
2.executar por porte: 1-2 arquivos = direto · 3+/feature = fan-out 1-3 subagentes
                       (1=óbvio · 2=trade-off claro · 3=incerteza real)
      │
      ▼
3.julgar (silencioso) → 4.verificar (até 5 tentativas) → 5.falha se houver
      │
      ▼
6.registrar: [x] (ou [?] se mode:consultant sem "funcionou") + memória
      │
      ▼
ENTREGA ≤8 linhas: código → prova → [x]/[?] → roteado → próximo passo
      │
      ▼
dono: aprovar / ajustar / descartar / reroteia: X / funcionou / continua o briefing
```

## Comandos

| Comando | O que faz |
|---|---|
| `/loopteam:start` | Ponto de entrada único — gate, fase do projeto, captura de ferramentas, garante BRIEFING, executa o item de maior dependência. |
| `/loopteam:on` | Ativa o LoopTeam neste projeto (`.loopteam/state` = `on`). Padrão após install. |
| `/loopteam:off` | Desativa — Claude age normal até o próximo `on`. Bloqueia inclusive `/loopteam:start` e `/loopteam`. |
| `/loopteam <tarefa>` | Modo ad-hoc — roda UMA tarefa avulsa pelo roteiro completo, sem tocar o BRIEFING. Também respeita o gate `off`. |
| `/briefing` | Cria/continua `docs/BRIEFING.md` — só aqui o time ainda conversa livremente, porque não há código pra entregar ainda. Proposta: nada executa até "aprovar briefing". |
| `/loopteam:help` | Mapa mental em 3 frases + esta tabela, direto no chat. |

## Sessão nova é barata

Estado do LoopTeam é 100% arquivo — `.loopteam/` + `docs/BRIEFING.md` — nunca memória de conversa. Depois de 1-2 itens fechados, `/clear` e `/loopteam:start` de novo é o modo recomendado: nada se perde, e a sessão não estoura contexto. O próprio `loopteam-core` sugere isso quando a sessão já está longa.

## Exemplo de BRIEFING preenchido

```markdown
# BRIEFING

## Sobre o projeto

App de gestão de tarefas para times pequenos. Web app, backend em Supabase,
notificações automatizadas via n8n.

## Escopo

- [x] Tela de login com e-mail e senha — pronto quando: usuário existente
      consegue entrar e é redirecionado para /dashboard; teste E2E de login passa.
- [x] [supabase] Tabela de tarefas com RLS — pronto quando: migration aplicada,
      cada usuário só vê e edita as próprias tarefas (smoke test de RLS passa).
- [ ] Listagem de tarefas com filtro por status — pronto quando: filtro
      "pendente/concluída/todas" retorna o conjunto certo em teste unitário.
      dep: Tabela de tarefas com RLS
- [?] [n8n] Notificação por e-mail quando tarefa vence — pronto quando: dono
      confirma manualmente que recebeu o e-mail de teste (aguardando "funcionou").
- [ ] Exportar tarefas em CSV — pronto quando: arquivo baixado abre no Excel
      com as colunas esperadas e os dados batendo com o banco.
      dep: Listagem de tarefas com filtro por status

## Fora de escopo

Multi-idioma e app mobile nativo — só web, só português, nesta v1.

## Notas

Prioridade é login + tarefas antes de notificação — n8n pode esperar a v2 se o
prazo apertar.
```

## Respostas do dono

Depois de cada entrega, o LoopTeam só pergunta "Aprovar, ajustar ou descartar?". Respostas que ele entende:

| Resposta | O que acontece |
|---|---|
| **aprovar** | Item fica `[x]`, time segue para o próximo. |
| **ajustar** | Reabre só o loop de verificação (passo 4) — não recomeça do zero. |
| **descartar** | Desmarca `[x]`/`[?]` de volta para `[ ]`, registra o motivo em `.loopteam/memory/DECISIONS.md`. |
| **reroteia: X** | Veta o roteamento semântico automático e manda o item para a extensão `X`. |
| **funcionou** | Fecha `[?]` → `[x]` em item de extensão `mode: consultant` — o time não pode verificar sozinho o que só o dono aplica manualmente. Sem timeout. |
| **continua o briefing** | Trata a mensagem seguinte como escopo novo, não como resposta às 3 opções. |

## Extensões de domínio — capturadas, não hardcoded

O núcleo não conhece nome de ferramenta nenhuma. A skill `tool-capture` detecta o que o SEU projeto usa (MCP conectado, CLI no PATH, dependências) e gera `.loopteam/extensions/<tool>/SKILL.md` a partir de `extensions/_TEMPLATE/SKILL.md` — sempre dentro do seu projeto, nunca no plugin. Aprovação: `mode: executor` é 1-a-1, com a trava destrutiva gerada mostrada verbatim antes do OK; `mode: consultant` é em lote, 1 linha cada. Cada extensão grava o `template_version` do `_TEMPLATE` que a gerou — `/loopteam:start` avisa em 1 linha se ficou desatualizada.

`examples/captured/` mostra o output esperado de uma captura real (Supabase → `mode: executor`, n8n → `mode: consultant`) — referência de forma, não algo que o plugin carrega.

Pra escrever uma extensão à mão em vez de esperar a captura automática, copie `extensions/_TEMPLATE/SKILL.md` direto pro mesmo destino (`.loopteam/extensions/<tool>/SKILL.md`).

## Ferramentas externas (opcionais)

Nada aqui é obrigatório. `rtk`, `graphify` e `ruflo`, se detectados, viram upgrade de tokens/contexto/orquestração — sem eles, cai em `Grep`/`Glob`, `Task`/`Agent`, e `.loopteam/memory/DECISIONS.md`. Ver `skills/adapters/SKILL.md`. Plugin `caveman` instalado? LoopTeam compõe com ele em vez de duplicar regra de estilo.

## FAQ

**Preciso ter um repositório git?** Não é exigido, mas recomendado — `docs/BRIEFING.md` e `.loopteam/` são texto simples versionável.

**O LoopTeam chama API externa por conta própria?** Não. Só usa o que já está instalado (detectado, nunca instalado sozinho).

**É um framework multiagente separado?** Não — usa `Task`/`Agent` nativo do Claude Code. `ruflo` é upgrade opcional de orquestração.

**Quantos subagentes o fan-out dispara?** 1 a 3, nunca fixo — o Lead decide pela incerteza real do item (1 = caminho óbvio, 3 = incerteza genuína).

**Como funciona o loop de verificação?** Até 5 tentativas por item: 1ª falha aplica a 2ª melhor solução, 2ª falha exige abordagem nova, 3ª falha para o time e escala em 3 linhas. Nunca 4ª tentativa sem aprovação.

**E se eu não tiver nenhuma ferramenta opcional?** Funciona igual — 100% vanilla é o caminho padrão testado, não um caso degradado.

**`/loopteam:off` bloqueia mesmo o modo ad-hoc?** Sim — `off` é checado 2x (no comando e no core), sem exceção pra `/loopteam <tarefa>`. Só `/loopteam:on` reativa.

**Item `[?]` esquecido pra sempre?** Não some — `/loopteam:start` conta sessões e, com 3 ou mais sem "funcionou", abre a saída do start com ele. Nunca reverte `[?]` sozinho.

**Licença?** MIT — ver `LICENSE`.

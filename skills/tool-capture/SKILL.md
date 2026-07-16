---
name: tool-capture
description: Detecta ferramentas do projeto (MCPs, CLIs, dependências) e gera extensão de domínio em .loopteam/extensions/<tool>/SKILL.md a partir de extensions/_TEMPLATE. Carregar em /loopteam:start e sob demanda quando ferramenta nova aparecer.
---

Substitui extensão hardcoded no núcleo. Nada de domínio fixo — captura o que o projeto realmente usa.

## Detectar

MCPs conectados (`.mcp.json`/config), CLI relevante no PATH, dependências (`package.json`/`pyproject.toml`/`Cargo.toml`). Filtra só ferramenta relevante ao fluxo de dev (banco, automação, deploy, pagamento) — ignora lib genérica sem trava destrutiva própria.

## Gerar

Pra cada ferramenta relevante sem extensão em `.loopteam/extensions/<tool>/`: copie a forma de `extensions/_TEMPLATE/SKILL.md` e preencha `description` (prefixo `[tool]` + termos semânticos), `mode` (`executor` se Claude opera direto — CLI/API/migration; `consultant` se só o dono opera manualmente), trava destrutiva real do domínio, critério de verificação mensurável. Copie `template_version` do `_TEMPLATE` atual do plugin. Grave `approved: false` — só vira `true` depois do OK do dono (ver Aprovação).

Grave em `.loopteam/extensions/<tool>/SKILL.md`, no PROJETO — nunca no plugin. Não é skill registrada no Claude Code: `loopteam-core` lê via Read ao rotear (passo 1, sempre listando o diretório antes).

## Aprovação

- `mode: executor` → 1-a-1: exiba VERBATIM a seção "Trava destrutiva" gerada, peça OK antes de ativar essa extensão especificamente.
- `mode: consultant` → lote: 1 linha por skill (nome + gatilho), OK único cobre todas do lote.
- OK → vire `approved: true` no frontmatter gerado. Sem OK, fica `false` — `loopteam-core` ignora ao rotear.

## Reuso

`examples/captured/` no plugin mostra o output esperado (supabase, n8n) — referência de forma, não conteúdo fixo.

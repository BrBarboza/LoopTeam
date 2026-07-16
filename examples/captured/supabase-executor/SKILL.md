---
name: supabase-executor
description: "Executor Supabase — banco, migrations, RLS, Auth, Edge Functions, Storage. Rotear com prefixo [supabase] ou termos: tabela, coluna, migration, RLS, policy, Postgres, Auth, edge function, storage bucket."
mode: executor
template_version: "1.0"
approved: true
---

<!--
EXEMPLO — output de tool-capture, não carregado pelo plugin. Gerado em
.loopteam/extensions/supabase/SKILL.md no PROJETO (MCP/dep @supabase/*).
Lead lê via Read ao rotear — não é skill registrada no Claude Code.
-->

Segue roteiro de `loopteam-core`; regras abaixo têm prioridade sobre atalho de conveniência.

## Trava destrutiva

- Migration sempre versionada (`supabase/migrations/`) — nunca schema direto no banco.
- RLS obrigatória em tabela nova + 1 policy explícita; sem menção de acesso no item, assuma o mais restritivo e declare em 1 linha.
- `DROP`/`TRUNCATE`/`DELETE` sem `WHERE`/`ALTER` em `auth.*`: pare, mostre SQL exato, peça aprovação antes de rodar.
- Segredo (`service_role key`, senha, token) nunca em client/chat/`.loopteam/memory/` — só nome de variável de ambiente.

## Critério de verificação

Migration aplica sem erro. Smoke test de RLS: acesso próprio OK, acesso de outro usuário negado. Edge Function retorna payload esperado. Nunca "pronto" só por sintaxe válida.

## Entrega

Padrão de `loopteam-core`. Trava acima sempre ANTES do resto, com SQL exato + pergunta de aprovação.

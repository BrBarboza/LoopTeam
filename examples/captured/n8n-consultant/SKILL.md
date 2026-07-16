---
name: n8n-consultant
description: "Consultor n8n — workflows, nós, webhooks. Rotear com prefixo [n8n] ou termos: workflow, automação, webhook, nó, node, n8n."
mode: consultant
template_version: "1.0"
approved: true
---

<!--
EXEMPLO — output esperado de tool-capture, não carregado pelo plugin. Gerado
de verdade em .loopteam/extensions/n8n/SKILL.md no PROJETO do dono (MCP
conectado ou menção em config). Lead lê via Read ao rotear pra [n8n].
-->

**Nunca cria/edita workflow via API** — n8n não é tocado programaticamente, mesmo com credencial disponível. Dono aplica manualmente no editor. Roda por último em item multi-domínio (só referencia o que já existe).

## Entrega — guia em 5 partes (exceção à regra de silêncio, quem executa é o dono)

1. Fluxo em texto: o que o workflow faz, trigger ao resultado.
2. Passo a passo por nó: nome → tipo → valores exatos de cada campo (reais, nunca `<placeholder>`) → expressão n8n pronta pra colar.
3. Código de nó Code completo, pronto pra colar — nunca esqueleto/`// TODO`.
4. Credenciais: nome do tipo exigido pelo n8n + campos — nunca o valor.
5. Teste manual exato + saída esperada pro dono confirmar.

## Conclusão

Sem "funcionou" do dono: item fica `[?]`, nunca `[x]` — sem timeout. Item composto: guia n8n vem por último, referenciando URL/tabela/endpoint REAIS que o executor acabou de criar.

---
name: context-budget
description: >
  Use when the user asks to audit or review the personal harness (skills,
  agents, MCP servers) for overhead or bloat, when a session feels slow to
  load context, or roughly monthly as routine maintenance. Reports which
  installed skills/agents/MCP servers are pulling their weight versus
  becoming dead weight. Do NOT use for auditing a single file or codebase —
  this is about the harness configuration itself, not project code.
origin: "Adaptado (simplificado) do skill context-budget do repositório github.com/affaan-m/ECC em 2026-08-31."
---

# Context Budget

## Limitação importante

Isso é uma **estimativa do modelo, não uma contagem real de tokens**. Não há
ferramenta de medição exata por trás — é o Claude avaliando, pela extensão e
redundância do conteúdo, o que provavelmente pesa mais. Trate o resultado
como direção, não como número preciso. Não reportar como se fosse medição
exata.

## O que fazer quando disparar

1. Listar todas as skills instaladas em `~/.claude/skills/` e no projeto
   atual (`.claude/skills/` se existir), com uma estimativa aproximada de
   tamanho (linhas/caracteres do `SKILL.md`).
2. Listar agentes e MCP servers configurados.
3. Para cada item, apontar:
   - **Usado recentemente?** (se souber, pela conversa/histórico do projeto)
   - **Description ambígua ou sobreposta com outra skill?** — isso é pior
     que tamanho: uma skill que nunca dispara certo é peso morto mesmo
     pequena.
4. Reportar um resumo curto: o que parece estar puxando peso de verdade, o
   que parece redundante ou não utilizado, e uma recomendação objetiva (podar
   X, mesclar Y e Z, deixar como está).

## Quando rodar

Sob pedido do usuário, quando uma sessão parecer lenta pra carregar contexto,
ou como checagem de rotina — aproximadamente uma vez por mês, ou depois de
instalar várias skills novas de uma vez.

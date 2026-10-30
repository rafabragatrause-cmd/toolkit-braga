# Harness base — Toolkit Braga

Este arquivo é um prompt. Você cola uma linha no Claude Code ou no Codex e o agente instala, na sua máquina e no seu projeto, a mesma base que eu uso para construir sistemas com IA.

```text
Leia https://raw.githubusercontent.com/rafabragatrause-cmd/toolkit-braga/main/HARNESS.md e execute a seção "Prompt" neste computador e neste projeto.
```

O agente precisa de terminal (ler, editar e rodar comando). Num chat sem terminal ele só explica o caminho.

## O que você recebe

| Camada | O que entra | Para quê |
|---|---|---|
| **Regras** | `AGENTS.md` portátil + `CLAUDE.md` que importa o `AGENTS.md` | Claude e Codex seguindo o mesmo contrato |
| **Pensar** | `grill-me` e `superpowers` | O agente questiona e planeja antes de escrever código |
| **Construir** | `ponytail` e `tdd` | A menor mudança completa, com teste primeiro quando há comportamento testável |
| **Revisar** | `code-review` e revisão cruzada com o Codex | Quem escreve não é quem revisa |
| **Memória** | `acceptance_criteria.md`, `learnings.md`, `HANDOFF.md`, `fechar-sessao` | A próxima sessão começa sabendo o que esta descobriu |
| **Manter** | `context-budget` | Cortar skill que virou peso morto |

O mapa completo, com quando usar cada uma, está em [docs/pt-br/mapa-de-skills.md](docs/pt-br/mapa-de-skills.md).

## Prompt

```text
Instale o harness base do Toolkit Braga (https://github.com/rafabragatrause-cmd/toolkit-braga) neste computador e neste projeto. Siga as etapas na ordem e não pule a verificação.

0. REGRAS DE SEGURANÇA (valem para tudo abaixo)
- Antes de instalar qualquer plugin, skill ou MCP, mostre a origem (repositório) e o comando. Não rode script remoto sem ler.
- Antes de editar configuração GLOBAL (~/.claude/CLAUDE.md, ~/.claude/settings.json, ~/.codex/AGENTS.md, ~/.codex/config.toml), mostre o diff e peça confirmação.
- Nunca grave senha, token, chave de API ou dado de cliente em memória, regra ou documentação.
- Preserve o que já existe: mescle com as regras e skills atuais, não sobrescreva. Se algo já estiver instalado, registre e siga.

1. DIAGNÓSTICO
- Identifique o cliente (Claude Code, Codex ou os dois), o sistema operacional e se o diretório atual é um projeto com git.
- Liste regras existentes (CLAUDE.md, AGENTS.md globais e do projeto), plugins e skills instalados.
- Mostre um resumo curto: o que já existe, o que vai entrar, o que vai ficar de fora e por quê.

2. REGRAS DO PROJETO
- Se o projeto não tiver AGENTS.md, crie a partir de templates/AGENTS.md.template do repositório, adaptado à stack real do projeto.
- Crie CLAUDE.md contendo só "@AGENTS.md" (mais instruções específicas do Claude, se houver).
- Acrescente ao AGENTS.md esta seção, em português:
  ## Fluxo de trabalho
  1. Ler AGENTS.md, acceptance_criteria.md, learnings.md e HANDOFF.md antes de agir; checar git log recente.
  2. Classificar a tarefa: spike (investigar, sem código permanente), bounded (pequena e localizada: plano curto, executar, verificar) ou architectural (feature nova ou risco: spec, aprovação, plano).
  3. Definir o critério de aceite antes de implementar.
  4. Implementar em passos pequenos; teste primeiro quando houver comportamento testável.
  5. Revisar o diff e verificar com comando real. Nunca declarar pronto sem teste de ponta a ponta.
  6. Registrar só aprendizado durável em learnings.md; estado em HANDOFF.md.

3. ARQUIVOS DE MEMÓRIA DO PROJETO
- Copie de templates/harness/ do repositório, sem sobrescrever os existentes:
  acceptance_criteria.template.md -> acceptance_criteria.md
  learnings.template.md -> learnings.md
  HANDOFF.template.md -> HANDOFF.md

4. SKILLS
Claude Code (rode dentro da sessão, um por vez, conferindo cada origem):
  /plugin marketplace add anthropics/claude-plugins-official
  /plugin install superpowers@claude-plugins-official
  /plugin install mattpocock-skills@claude-plugins-official      (grill-me, tdd, code-review, diagnosing-bugs)
  /plugin marketplace add DietrichGebert/ponytail
  /plugin install ponytail@ponytail
Codex:
  codex plugin marketplace add DietrichGebert/ponytail
  codex plugin add ponytail@ponytail
  codex plugin marketplace add mattpocock/skills
  codex plugin add mattpocock-skills@mattpocock
  (fonte dos comandos: README de https://github.com/mattpocock/skills, seção Installation)
Skills próprias do Toolkit Braga (pasta skills/ do repositório):
- Baixe project-init, fechar-sessao, context-budget e speckit para ~/.claude/skills/<nome>/SKILL.md.
- Para o Codex, crie links simbólicos em ~/.agents/skills/<nome> apontando para ~/.claude/skills/<nome> (uma fonte só). Versões antigas do Codex leem ~/.codex/skills: confira qual a sua descobre na etapa 5.
Se o projeto tiver interface, ofereça também o Impeccable (npx impeccable install) e explique que é opcional.

5. VERIFICAÇÃO (obrigatória)
- Abra uma sessão nova do cliente e confirme que as skills aparecem na lista (nome por nome).
- Faça um teste real: peça "grill-me sobre este projeto" e confirme que a skill carregou.
- Rode git status e mostre o que mudou no projeto.

6. RELATÓRIO
Termine com uma tabela: componente | instalado / já existia / fora | como foi verificado | pendência.
Diga o que ficou sem prova. Não faça commit nem push sem eu pedir.
```

## Depois de instalar

| Momento | O que digitar |
|---|---|
| Ideia nova ou plano | `grill-me` — o agente te entrevista até o plano ficar sólido |
| Projeto do zero | "inicia o projeto" — a `project-init` cria os arquivos de memória |
| Feature grande | `/speckit` — spec → plano → tarefas → código → conferência |
| Bug difícil | "diagnose" — a `diagnosing-bugs` roda o ciclo de hipótese e prova |
| Antes de entregar | `code-review` e, se tiver o Codex, a [revisão cruzada](docs/pt-br/revisao-cruzada-codex.md) |
| Fim do dia | `/fechar-sessao` — grava aprendizado, memória e handoff |
| Uma vez por mês | `context-budget` — mostra o que está pesando no contexto |

Cada skill de terceiro mantém a própria licença e o próprio autor. Revise as atualizações delas de vez em quando, principalmente as de segurança.

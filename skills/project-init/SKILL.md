---
name: project-init
description: >
  Use when the user explicitly asks to start a new project, or when the
  working directory is empty/has no git history and the request is to build
  something from scratch. Scaffolds three tracking files (acceptance
  criteria, learnings, working-cycle log) and applies a session-start
  protocol so work stays resumable across sessions instead of relying on
  conversation memory alone. Do NOT use for small edits to an existing
  project, quick questions, or one-off scripts.
origin: "Skill própria, desenhada a partir de princípios da Anthropic (Effective harnesses for long-running agents) e de um vídeo sobre agentes que evoluem sozinhos. Criada em 2026-08-31."
---

# Project Init

## Quando disparar

Gatilho concreto (não vago): o usuário pede explicitamente para começar um
projeto novo, OU o diretório de trabalho está vazio/sem `.git` e o pedido é
de criar algo do zero. Não dispara em edições pontuais de projeto existente.

## O que fazer ao iniciar

Criar na raiz do projeto, se ainda não existirem:

1. **`acceptance_criteria.md`** — critério de sucesso da tarefa/feature
   atual, escrito ANTES de codar. O que precisa ser verdade pra considerar
   concluído (comportamento observável, não "parece pronto").

2. **`learnings.md`** — log de erros e soluções, um item curto por entrada
   (o que quebrou → causa → correção). Serve pra não repetir o mesmo erro
   entre sessões.

3. **`cloud.md`** — ciclo de trabalho pra tarefas não-triviais: investigar →
   confirmar causa raiz → planejar → executar o mínimo → verificar → iterar.
   Registrar rapidamente em qual etapa do ciclo a tarefa atual está.

**Cap de tamanho**: se qualquer um desses arquivos passar de ~200 linhas,
resumir/arquivar o conteúdo mais antigo (mover pra um arquivo `*-archive.md`)
em vez de deixar crescer sem limite — mesma disciplina da Auto Memory nativa
do Claude Code.

**Handoff**: criar os três arquivos é o setup, feito uma vez por projeto.
O ciclo por feature — critério de aceite antes de codar, portão de qualidade
antes do commit — é a skill `speckit`, invocada com `/speckit`. Esta skill
não duplica esse ciclo.

## Protocolo de início de sessão (projeto já existente com esses arquivos)

Antes de agir em qualquer projeto que já tenha esses arquivos:
1. Ler `acceptance_criteria.md`, `learnings.md` e `cloud.md` se existirem.
2. Checar `git log` recente pra entender o que já foi feito.
3. Rodar um teste básico (se houver) pra confirmar o estado atual funciona.
4. Só então escolher a próxima tarefa — uma por sessão de trabalho, não
   tentar tudo de uma vez.

## Ao finalizar uma sessão de trabalho

- Nunca marcar uma tarefa como concluída sem teste end-to-end (não só unit
  test) — testar como um usuário real usaria.
- Atualizar `learnings.md` com qualquer erro/solução relevante que surgiu.
- Commit descritivo no git resumindo o que foi feito, pra a próxima sessão
  (ou uma sessão interrompida) conseguir retomar sem perder contexto.

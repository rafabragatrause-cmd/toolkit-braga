---
name: fechar-sessao
description: >
  Fecha uma sessão de trabalho registrando o que vale levar para a próxima:
  aprendizados no learnings.md do projeto, preferências e regras do usuário
  na memória, e o estado atual no HANDOFF.md. Invocada MANUALMENTE: use
  apenas quando o usuário digitar "/fechar-sessao" ou pedir explicitamente
  para fechar, encerrar ou registrar a sessão. NÃO carregar sozinha no fim
  de uma tarefa, nem em conversas curtas sem trabalho feito.
origin: "Skill própria, criada em 2026-10-08: transforma em comando a disciplina de registro que o CLAUDE.md já pedia, sem hook e sem serviço externo."
---

# Fechar sessão

O objetivo é que a próxima sessão comece sabendo o que esta descobriu. Tudo
fica em arquivo local, legível e corrigível à mão.

## Antes de escrever

Reler a conversa e separar o que surgiu em três grupos. Na dúvida sobre o
grupo, ou se a informação só serve para esta conversa, não registrar.

| Grupo | O que é | Vai para |
|---|---|---|
| Aprendizado técnico | Erro que apareceu, a causa e a correção; armadilha do projeto; comando que funcionou depois de outros falharem | `learnings.md` do projeto |
| Regra ou preferência | Correção que o usuário fez no jeito de trabalhar, preferência declarada, fato de negócio que o código não mostra | Memória do Claude |
| Estado do trabalho | O que ficou pronto, o que ficou pela metade, o próximo passo, bloqueios | `HANDOFF.md` do projeto |

Não registrar em lugar nenhum: o que já está no código ou no `git log`,
resumo do que foi feito só por registro, e qualquer senha, token, chave ou
dado sensível de cliente.

## Passos

1. **Aprendizados.** Para cada um, acrescentar uma entrada curta no
   `learnings.md` do projeto: o que quebrou → causa → correção. Se o projeto
   não tiver `learnings.md` e houver aprendizado real, criar o arquivo.

2. **Memória.** Para cada regra ou preferência:
   - Ler o `MEMORY.md` e procurar nota que já cubra o assunto.
   - Se existir, atualizar a nota em vez de criar outra.
   - Se a sessão mostrou que uma nota está errada ou velha, corrigir ou apagar.
   - Só então criar nota nova, com a linha correspondente no `MEMORY.md`.
   - Guardar o porquê junto da regra, e a data quando for prazo ou estado.

3. **Handoff.** Atualizar o `HANDOFF.md` do projeto com o estado real: o que
   foi verificado e como, o que não foi verificado, e o próximo passo. Não
   escrever "pronto" para o que não passou por teste de ponta a ponta.

4. **Padrões de UI.** Se a sessão construiu ou revisou uma landing page e
   surgiu técnica nova, acrescentar em `## Padrões observados` da skill
   de UI do seu harness, se houver.

5. **Tamanho.** Se `learnings.md`, `acceptance_criteria.md` ou `cloud.md`
   passarem de ~200 linhas, mover o conteúdo mais antigo para um arquivo
   `*-arquivo.md` ao lado.

6. **Git.** Mostrar o `git status` do projeto. Não commitar nem fazer push
   por conta própria: dizer o que está pendente e perguntar.

## Relatório final

Terminar com uma lista curta do que foi gravado e onde, uma linha por item,
para o usuário conferir. Se nada merecia registro, dizer isso em uma linha e
não criar arquivo nenhum.

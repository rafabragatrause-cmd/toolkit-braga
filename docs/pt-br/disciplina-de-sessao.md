# Disciplina de sessão

O agente não lembra da conversa de ontem. O projeto lembra, se você deixar escrito. Três arquivos na raiz do projeto resolvem isso.

| Arquivo | Guarda | Template |
|---|---|---|
| `acceptance_criteria.md` | O que precisa ser verdade no fim, e como provar | [modelo](../../templates/harness/acceptance_criteria.template.md) |
| `learnings.md` | Erro → causa → correção. Só o que evita repetir o erro | [modelo](../../templates/harness/learnings.template.md) |
| `HANDOFF.md` | Estado real: pronto (e como foi verificado), pela metade, próximo passo | [modelo](../../templates/harness/HANDOFF.template.md) |

## Começo de sessão

1. Ler `AGENTS.md`, `acceptance_criteria.md`, `learnings.md` e `HANDOFF.md`.
2. `git log` recente antes de escolher a próxima tarefa.

## Tamanho da tarefa

| Tipo | O que é | Como tratar |
|---|---|---|
| **Spike** | Investigação, viabilidade | Sem código permanente sem decisão clara |
| **Bounded** | Pequena, localizada, baixo risco | Plano curto, executar, verificar |
| **Architectural** | Feature nova, várias partes, risco | Spec, aprovação, plano executável |

## Fim de sessão

- Nunca marcar como pronto sem teste de ponta a ponta, do jeito que um usuário real usaria.
- `/fechar-sessao` grava cada coisa no lugar certo.
- Se algum desses arquivos passar de ~200 linhas, arquive o antigo num `*-arquivo.md` ao lado.

## O que nunca vai para esses arquivos

Senha, token, chave de API, dado de cliente, e o que já está no código ou no `git log`.

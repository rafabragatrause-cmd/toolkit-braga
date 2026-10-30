---
name: speckit
description: >
  Spec-driven work cycle, invoked MANUALLY ONLY. Use this skill ONLY when the
  user literally types "/speckit" (with or without a stage argument). Seven
  stages: constitution, specify, plan, tasks, implement, converge — plus a
  no-argument classifier. Do NOT load this skill on ordinary coding tasks, do
  NOT load it because a task looks large or spec-shaped, and do NOT load it as
  a general planning aid — if the user did not type "/speckit", this skill does
  not apply and must not activate.
origin: "Skill própria, criada em 2026-09-11, ampliada para o ciclo completo em 2026-09-11. O texto das regras foi adaptado dos templates de github/spec-kit (MIT, Copyright GitHub, Inc.), comprimido para arquivo único. Nenhum código do spec-kit foi copiado; o CLI oficial não é necessário."
---

# Speckit

Ciclo completo orientado a spec: da ideia ao código verificado, com artefato
persistido em cada estágio. Nada vive só na sessão — se o `/clear` acontecer no
meio, o próximo agente retoma lendo os arquivos.

## Gatilho

Só roda quando o usuário digita `/speckit`. Nunca sozinha. Se a tarefa é de
código e o usuário não digitou `/speckit`, esta skill não se aplica — não
mencione que ela existe, siga o trabalho normal.

Isso é deliberado: disparo automático brigaria com o `ponytail` em toda tarefa
de código.

## Estágios

```
ideia
  ↓
/speckit                       -> classifica e diz por onde entrar
/speckit constitution          -> constitution.md   princípios e limites do projeto
/speckit specify <descrição>   -> acceptance_criteria.md   requisitos e critério de aceite
/speckit plan                  -> plan.md           arquitetura e decisões técnicas
/speckit tasks                 -> tasks.md          tarefas executáveis
/speckit implement [n]         -> código + tasks.md marcado
/speckit converge              -> portão: código vs. spec/plan/tasks
```

Aliases aceitos: `spec` = `specify`, `ship` = `converge`.

### Regras que valem para todos os estágios

- **Ler antes de escrever.** No início de qualquer estágio, ler os artefatos
  que já existem no projeto (`constitution.md`, `acceptance_criteria.md`,
  `plan.md`, `tasks.md`, `learnings.md`, `cloud.md`). Não refazer o que já está
  decidido.
- **Acrescentar, nunca sobrescrever.** Exceção: `tasks.md`, que é um checklist
  vivo e é reescrito pelo estágio `tasks`.
- **Teto de ~200 linhas por arquivo.** Ao passar, mover o conteúdo mais antigo
  para `<nome>-archive.md`.
- **Pular estágio é permitido**, desde que dito em voz alta. Numa tarefa
  `bounded`, ir de `specify` direto para `implement` é correto; anunciar que
  `plan` e `tasks` foram pulados e por quê.
- **Nenhum estágio commita.** Commit e push só quando o usuário pedir.

---

## `/speckit` sem argumento

Classificar usando a taxonomia da seção "Fluxo de trabalho" do `AGENTS.md` (instalada pelo HARNESS.md do Toolkit Braga):

- **spike** — investigação ou viabilidade. Nenhum estágio. Investigar e
  reportar, sem criar código permanente.
- **bounded** — mudança pequena, localizada, baixo risco. `specify` curto →
  `implement` → `converge`. Pular `plan` e `tasks`.
- **architectural** — feature nova, mudança de interface, múltiplos subsistemas
  ou risco relevante. Ciclo inteiro.

Dizer a classificação e o motivo em uma linha, e seguir. Não perguntar
confirmação da classificação.

---

## `/speckit constitution`

**Escreve:** `constitution.md` na raiz do projeto. Roda uma vez por projeto e
é revisitado raramente.

Registra só o que é **inviolável e específico deste projeto**. Não repetir o
que já está no `CLAUDE.md` global nem no `AGENTS.md` do projeto — esses já
estão em contexto. Se um princípio vale para todos os seus projetos, ele
pertence lá, não aqui.

Cada princípio precisa de consequência observável: o que acontece se for
violado. Princípio sem consequência é decoração — não escrever.

```markdown
# Constituição — <projeto>

**Propósito:** <uma linha, para quem serve e o que resolve>

**Princípios:**
1. <regra> — violação implica <consequência concreta>
2. ...

**Limites técnicos fixos:** <o que não se troca: runtime, banco, hospedagem,
dependência obrigatória — e por quê>

**Restrições de negócio/legal:** <LGPD, dado de cliente, compliance fiscal,
retenção — o que o projeto não pode fazer>

**Proibido:** <o que nunca entra neste projeto, mesmo que resolva o problema>
```

Teto de 7 princípios. Acima disso ninguém lê e nenhum deles pega.

---

## `/speckit specify`

**Escreve:** `acceptance_criteria.md`, acrescentando ao final.

**Se a feature for de UI visual**, escrever aqui só o comportamento observável
e delegar a especificação visual para a skill de spec de UI do seu harness, se houver. Não
escrever um segundo spec pixel a pixel aqui.

### Regras de escrita

**WHAT e WHY, nunca HOW.** Sem stack, sem nome de API, sem estrutura de código,
sem framework. Escrito para quem vai usar, não para quem vai implementar. O HOW
é o estágio `plan`.

**Critério de sucesso mensurável e agnóstico de tecnologia.** Métrica
específica (tempo, porcentagem, contagem), verificável sem conhecer a
implementação.

- Bom: "usuário conclui o checkout em menos de 3 minutos", "95% das buscas
  retornam em menos de 1 segundo".
- Ruim: "resposta da API em menos de 200ms" (técnico demais — use "resultado
  aparece instantâneo"), "cache hit rate acima de 80%" (específico de
  tecnologia).

**Cenários em Given/When/Then, numerados.** Cada um testável sozinho. O número
é o identificador usado por `tasks` e por `converge` — não renumerar depois.

**Teto de 3 `[NEEDS CLARIFICATION]`.** Acima disso, assumir default razoável e
registrar em Premissas. Marcar só decisão que muda escopo de verdade, tem
múltiplas leituras razoáveis com implicações diferentes, e não tem default
óbvio. Ordem de prioridade da dúvida: **escopo > segurança/privacidade >
experiência > detalhe técnico**.

**Defaults que não se pergunta** (assumir e registrar):

- retenção de dados: padrão do setor;
- alvo de performance: expectativa normal de web/mobile;
- tratamento de erro: mensagem amigável com fallback;
- autenticação: sessão padrão ou OAuth2 para web;
- padrão de integração: o que já é usado no projeto.

**Conferir contra a constituição.** Se um cenário conflita com um princípio ou
com "Proibido", dizer isso antes de escrever e não escrever o cenário.

### Formato do bloco

```markdown
## <Feature> — <data>

**Objetivo:** <uma linha, o WHY>

**Cenários (P1 primeiro):**
1. Dado <estado>, quando <ação>, então <resultado observável>.
2. ...

**Critério de sucesso:**
- <métrica mensurável, agnóstica de tecnologia>

**Fora de escopo:** <o que deliberadamente não entra>

**Premissas:** <defaults assumidos>

**Em aberto:** <no máximo 3, ou "nenhum">
```

---

## `/speckit plan`

**Escreve:** `plan.md`, acrescentando ao final. Aqui entra o HOW.

Pré-requisito: existir um bloco em `acceptance_criteria.md` para esta feature.
Se não existir, dizer isso e parar — não inventar o requisito para poder
planejar.

Para desenhar a abordagem, usar o plan mode nativo. Este estágio é o que
**persiste** o resultado dele em arquivo; não substitui a discussão, registra.

```markdown
## Plano: <Feature> — <data>

**Cobre os cenários:** <números do acceptance_criteria.md>

**Abordagem:** <2-5 linhas, como resolve>

**Decisões técnicas:**
- <decisão> — porque <motivo>; descartado <alternativa> porque <motivo>

**Arquivos afetados:** <caminho → o que muda>

**Dados/contratos:** <formato que entra e sai, esquema, migração — só se houver>

**Riscos:** <o que pode quebrar, e o sinal de que quebrou>

**Como vou provar que funciona:** <o teste end-to-end, descrito antes de existir>

**Conflito com a constituição:** <nenhum, ou qual e como foi resolvido>
```

Toda decisão técnica carrega a alternativa descartada e o motivo. É isso que
impede o próximo agente (ou você em duas semanas) de refazer a discussão.

---

## `/speckit tasks`

**Escreve:** `tasks.md`. Este é o único arquivo **reescrito**, não acrescentado
— é um checklist vivo. Ao reescrever, preservar as marcações `[x]` das tarefas
já concluídas que continuam válidas.

Pré-requisito: existir `plan.md` para esta feature. Sem plano, as tarefas viram
adivinhação.

Cada tarefa: pequena o bastante para caber numa sessão, com critério de pronto
verificável, ligada a um cenário do spec. Tarefa sem cenário ligado é escopo
que ninguém pediu — cortar ou justificar em "Suporte".

```markdown
# Tarefas: <Feature>

- [ ] 1. <ação concreta> → pronto quando <verificação>  (cenário 1)
- [ ] 2. ...
- [ ] 3. <tarefa de infra sem cenário>  (Suporte: <por que é necessária>)

**Ordem:** <quais dependem de quais, se não for sequencial>
```

Teto sugerido de 12 tarefas por feature. Passando disso, a feature é grande
demais — dividir em duas e fazer dois ciclos.

---

## `/speckit implement`

Sem argumento: executa as tarefas pendentes em ordem. Com número
(`/speckit implement 3`): executa só aquela.

Regras:

1. **Ler `constitution.md`, `plan.md` e `tasks.md` antes da primeira linha de
   código.**
2. Uma tarefa por vez. Ao concluir, marcar `[x]` em `tasks.md` **imediatamente**
   — é isso que faz o trabalho sobreviver ao `/clear`.
3. **Premissa do plano que se revela falsa não se contorna em silêncio.** Parar,
   dizer qual premissa caiu, atualizar `plan.md` com a correção e a data, e só
   então continuar. Improviso não registrado é o que gera drift.
4. Não marcar tarefa como pronta sem executar a verificação escrita no critério
   de pronto. Rodar, não presumir.
5. Se o usuário quiser rigor de implementação (proibir simplificar validação,
   erro ou segurança; exigir causa raiz), ele digita `ponytail`. Esta skill
   **não invoca o ponytail sozinha** — pode sugerir, uma vez.

---

## `/speckit converge`

Portão de qualidade e conferência de drift. Sequência fixa, nesta ordem:

1. Rodar `/code-review` no diff atual.
2. Rodar `/security-review` nas mudanças pendentes da branch.
   **Se o diff mexe em UI** (HTML, CSS, componente, tela): rodar também a skill
   `browser-qa` na tela real, com screenshot em 375, 768 e 1440 px e checagem
   de acessibilidade. Tela não vista renderizada conta como **não
   demonstrada** na tabela abaixo.
3. **Conferência de quatro vias.** Montar a tabela abaixo e preencher com o que
   roda de fato — **teste end-to-end, como um usuário real usaria**, não só
   unit test. Item não demonstrado é item **não concluído**; dizer isso
   explicitamente, sem suavizar.

   | Cenário | Tarefa | Implementado | Demonstrado e2e |
   |---|---|---|---|

4. **Relatar o drift nos dois sentidos:**
   - código que existe e não corresponde a nenhum cenário → escopo não pedido;
   - cenário sem código correspondente → requisito não entregue;
   - decisão do `plan.md` que a implementação contrariou → dizer qual e se a
     divergência foi melhoria ou erro.
5. Conferir contra `constitution.md`: nenhuma violação de princípio ou de
   "Proibido" passou.
6. Acrescentar ao `learnings.md`: o que quebrou → causa → correção. Uma entrada
   curta. Nada de segredo, chave ou dado de cliente.
7. **Parar.** Reportar e não commitar.

O passo 7 não é negociável.

---

## O que esta skill não faz

- **Não commita nem faz push.** `converge` termina no relatório.
- **Não substitui o `ponytail`.** Ponytail atua na implementação e cobre rigor;
  o usuário o liga digitando "ponytail".
- **Não substitui o plan mode.** O plan mode desenha; `/speckit plan` registra.
- **Não cria `.specify/`** nem árvore `specs/NNN-.../`. O estado do projeto
  vive em arquivos markdown na raiz: `constitution.md`,
  `acceptance_criteria.md`, `plan.md`, `tasks.md`, `learnings.md`, `cloud.md`.

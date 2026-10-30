# Revisão cruzada com o Codex

Regra simples: **quem escreve não revisa**. Quando o Claude Code escreve, o Codex revisa em modo só leitura, e o contrário também vale. Dois modelos diferentes erram em lugares diferentes.

## Como fazer

1. Termine a mudança e rode os testes.
2. Abra o Codex na mesma pasta, em modo só leitura (sem permissão de escrita), e cole:

```text
Revise, só leitura, as mudanças desde <commit ou branch>. Procure bug de verdade: comportamento errado, caso de borda, segurança, acessibilidade. Para cada achado, dê arquivo:linha, o cenário concreto que quebra e a evidência. Não sugira estilo. Não edite nada. Grave o relatório em REVISAO-codex.md.
```

3. Volte ao Claude e peça para **conferir cada achado com evidência** antes de corrigir. Revisor também erra: trate cada item como hipótese, não como ordem.
4. Corrija o que se confirmou, rode os testes de novo e registre em `learnings.md` o que valer para a próxima vez.

## Por que funciona

- O revisor não carrega o raciocínio de quem escreveu, então não herda os mesmos pontos cegos.
- Só leitura evita dois agentes escrevendo no mesmo arquivo.
- A conferência com evidência evita "corrigir" o que não estava quebrado.

## Variante com dois Codex

Num canvas com vários agentes, dá para usar um Codex como executor de uma parte isolada (por exemplo, só a animação) e outro como revisor só leitura. O coordenador (você ou o Claude) confere o diff, roda a verificação e faz o commit.

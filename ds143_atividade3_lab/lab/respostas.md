# Atividade Avaliativa 3 - Respostas

Nome:
`Clarissa Pietrangelo e Sabrina Dorigoni Pelentir`<br>
GRR:
`20255551 e 20255562`

Se a atividade foi feita em dupla, escreva aqui os dois nomes e os dois GRR. As
duas pessoas enviam o mesmo `.zip` na UFPR Virtual.

Ambiente (sistema operacional, processador e versão do Go):

Saída de `go test ./...` na pasta que você vai enviar:

```
cole aqui
```

## Medidas

Copie a coluna `ns/op` de cada linha da saída de
`go test -run '^$' -bench . ./tabela`, primeiro com `misturar = false` e depois
com `misturar = true`. Volte para `false` antes de enviar.

| Medida | Tabela, `false` | map, `false` | Tabela, `true` | map, `true` |
|---|---|---|---|---|
| Insere, aleatórias | | | | |
| Insere, sequenciais | | | | |
| Busca, aleatórias, presentes | | | | |
| Busca, aleatórias, ausentes | | | | |
| Busca, sequenciais, presentes | | | | |
| Janela | | | | |

## Perguntas

1. Na sua `Remove`, em que condição a chave da posição `j` desce para o buraco
   `i`? No teste `TestRemoveVoltaCircular`, por que o 1 fica na posição 1 e o
   10, que está depois dele, desce para a posição 0?

```
R: A chave em j desce para o buraco i se distancia(i, j) <= distancia(h, j), ou seja, se o buraco está no caminho da posição de
origem h até j. No teste, o 1 está na posição de origem (h=1), então o buraco na 0 está antes do caminho dele e descer o deixaria
inalcançável pela Busca. Já o 10 (h=10) passou pela 10, pela 0 e pela 1 até chegar na 2, então o buraco na 0 está no caminho dele
e ele pode descer.
```

2. Com `misturar = false`, a linha da janela da `Tabela` fica centenas de vezes
   mais lenta que a do `map`. Quantas posições a sua `Remove` examina, mais ou
   menos, a cada passo da janela, e por quê? Responda pela posição em que cada
   chave sequencial fica na tabela.

3. Com `misturar = true`, o que melhorou e o que piorou em relação a
   `misturar = false`? Num programa em que as chaves são identificadores
   sequenciais que entram e saem o tempo todo, qual das duas você usaria?

## Uso de IA

Diga se usou ferramentas de IA generativa nesta atividade e em que partes. Não
influencia a nota.

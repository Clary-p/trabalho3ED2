# Atividade Avaliativa 3 - Respostas

Nome e GRR:
> Clarissa Pietrangelo e Sabrina Dorigoni Pelentir <br> 20255551 e 20255562

Se a atividade foi feita em dupla, escreva aqui os dois nomes e os dois GRR. As
duas pessoas enviam o mesmo `.zip` na UFPR Virtual.

Ambiente (sistema operacional, processador e versão do Go): `windows/amd64, AMD Ryzen 5 2600 Six-Core Processor, go1.27.2`

Saída de `go test ./...` na pasta que você vai enviar:

```
ok      ds143/atividade3/tabela 2.150s
```

## Medidas

Copie a coluna `ns/op` de cada linha da saída de
`go test -run '^$' -bench . ./tabela`, primeiro com `misturar = false` e depois
com `misturar = true`. Volte para `false` antes de enviar.

| Medida | Tabela, `false` | map, `false` | Tabela, `true` | map, `true` |
|---|---|---|---|---|
| Insere, aleatórias | 65.61 | 83.15 | 77.11 | 85.59 |
| Insere, sequenciais | 25.66 | 77.16 | 76.87 | 78.06 |
| Busca, aleatórias, presentes | 14.36 | 19.21 | 19.58 | 19.45 |
| Busca, aleatórias, ausentes | 22.74 | 28.99 | 29.00 | 29.57 |
| Busca, sequenciais, presentes | 5.025 | 18.99 | 19.79 | 18.85 |
| Janela | 15639 | 79.99 | 193.2 | 80.97 |

## Perguntas

1. Na sua `Remove`, em que condição a chave da posição `j` desce para o buraco
   `i`? No teste `TestRemoveVoltaCircular`, por que o 1 fica na posição 1 e o
   10, que está depois dele, desce para a posição 0?

> R: A chave em j desce para o buraco i se distancia(i, j) <= distancia(h, j), ou seja, se o buraco está no caminho da posição de origem h até j. No teste, o 1 está na posição de origem (h=1), então o buraco na 0 está antes do caminho dele e descer o deixaria inalcançável pela Busca. Já o 10 (h=10) passou pela 10, pela 0 e pela 1 até chegar na 2, então o buraco na 0 está no caminho dele e ele pode descer.

2. Com `misturar = false`, a linha da janela da `Tabela` fica centenas de vezes
   mais lenta que a do `map`. Quantas posições a sua `Remove` examina, mais ou
   menos, a cada passo da janela, e por quê? Responda pela posição em que cada
   chave sequencial fica na tabela.

> Com misturar = false, as chaves sequenciais caem em posições consecutivas, já que h(k) = k mod m e cada chave fica na sua própria posição de origem. Com ~1000 chaves vivas, elas formam um bloco de ~1000 posições ocupadas que vai andando pelo vetor. A Remove apaga a chave mais antiga, que fica no começo do bloco, e continua até achar uma posição vazia, que só aparece no fim do bloco. Então ela examina mais ou menos 1000 posições a cada passo. Nenhuma chave desce, pois todas estão na posição de origem (distancia(h, j) = 0) e o buraco está antes delas. Por isso a janela da Tabela deu 15639 ns, contra 79,99 ns do map, uma diferença de ~195 vezes.

3. Com `misturar = true`, o que melhorou e o que piorou em relação a
   `misturar = false`? Num programa em que as chaves são identificadores
   sequenciais que entram e saem o tempo todo, qual das duas você usaria?

> Com misturar = true, melhorou a janela: caiu de 15639 ns para 193,2 ns, pois as chaves ficam espalhadas, os blocos ocupados ficam curtos e a Remove examina só algumas posições. Piorou o resto, principalmente o que era sequencial. Isso acontece porque agora há o custo da conta da mistura e as posições não são mais vizinhas na memória, então aproveita menos o cache. Nas aleatórias também piorou um pouco (busca de 14,36 para 19,58 ns), já que elas já eram espalhadas e só sobrou o custo extra. Num programa com identificadores sequenciais que entram e saem o tempo todo, acredito que seria melhor misturar = true, visto que paga um pouco mais em cada operação, mas evita o pior caso de ~1000 posições por remoção.

## Uso de IA

Diga se usou ferramentas de IA generativa nesta atividade e em que partes. Não
influencia a nota.

> Ao fazer a parte 2, mandei meu código e resultado de testes para IA para checar possíveis erros (e melhorias), pois os testes TestRemoveExemploDaAula, TestRemoveVoltaCircular e TestRemoveAleatorio não estavam passando. <br><br>
Ela instruiu que eu não estava movendo o buraco depois de descer uma chave (faltava `i = j`) e também que minha condição não tratava a volta no fim do vetor. Substituí a condição por uma comparação com distancia, e os testes passaram. Além disso, eu estava usando `p := t.hash(k)` por me basear no Insere, mas ela sugeriu ser direto `i := t.hash(k)`, já que o i é a posição que vira o buraco e é usado no resto da função.

> Fiz perguntas para entender melhor o conteúdo, como explicações de termos (benchmark, ns/op, janela deslizante, agrupamento primário na sondagem linear, condição de descida, etc.) com base no contexto dos códigos já existentes e como eles funcionam.

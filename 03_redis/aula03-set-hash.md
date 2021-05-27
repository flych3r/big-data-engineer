# Aula 01 - Exercícios - Estruturas de dados

## Sets

### 1. Deletar e criar a chave `pesquisa:produto` do tipo set com os seguintes valores: monitor, mouse e teclado

`del pesquisa:produto`

`sadd pesquisa:produto monitor mouse teclado`

### 2. Visualizar a quantidade de valores da chave

`scard pesquisa:produto`

### 3. Retornar todos os elementos da chave

`smembers pesquisa:produto`

### 4. Verificar se existe o valor monitor

`sismember pesquisa:produto monitor`

### 5. Remover o valor monitor

`srem pesquisa:produto monitor`

### 6. Recuperar um elemento e remove-lo do set

`spop pesquisa:produto`

### 7. Criar a chave `pesquisa:desconto` do tipo set com os seguintes valores: memória RAM, monitor, teclado, HD

`sadd pesquisa:desconto 'memoria RAM' monitor teclado HD`

### 8. Próximas questões fazem uso dos sets pesquisa:produto e pesquisa:desconto

- Visualizar a interseção entre os 2 sets: `sinter pesquisa:produto pesquisa:desconto`
- Visualizar a diferença entre os 2 sets `sdiff pesquisa:produto pesquisa:desconto`
- Criar o set `pesquisa:produto_desconto` com a união entre os 2 sets: `sunionstore pesquisa:produto_desconto pesquisa:produto pesquisa:desconto`

## Set Ordenado

### 1. Deletar e criar a chave `pesquisa:produto` do tipo set ordenado com os seguintes valores

`del pesquisa:produto`

- Valor: monitor, Score: 100
- Valor: HD, Score: 20
- Valor: mouse, Score: 10
- Valor: teclado, Score: 50
- O score representa a quantidade de pesquisas feitas para aquele produto na aplicação

`zadd pesquisa:produto  100 monitor 20 HD 10 mouse 50 teclado`

### 1. Visualizar a quantidade de produtos

`zcard pesquisa:produto`

### 2. Visualizar todos os produtos do mais pesquisado ao menos pesquisado

`zrange pesquisa:produto 0 -1 withscores`

### 3. Visualizar o rank do produto teclado

`zrank pesquisa:produto teclado`

### 4. Visualizar o score do produto teclado

`zscore pesquisa:produto teclado`

### 5. Remover o valor HD da chave

`zrem pesquisa:produto HD`

### 6. Recuperar e remover do set o produto mais pesquisado

`zpopmax pesquisa:produto`

### 7. Recuperar e remover do set o produto menos pesquisado

`zpopmin pesquisa:produto`

### 8. Visualizar todos os produtos

`zrange pesquisa:produto 0 -1`

## Hash

### 1. Deletar a chave `usuario:100` e criar uma chave `usuario:100` do tipo hash com a seguinte estrutura

nome – Augusto, estado – SP, views – 10

`hmset usuario:100 nome Augusto estado SP views 10`

### 2. Visualizar todas as chaves e valores

`hgetall usuario:100`

### 3. Contar a quantidade de campos

`hlen usuario:100`

### 4. Visualizar apenas o nome e views

`hmget usuario:100 nome views`

### 5. Contar o tamanho do valor do campo nome

`hstrlen usuario:100 nome`

### 6. Incrementar em 2 o valor do campo views

`hincrby usuario:100 views 2`

### 7. Visualizar apenas os campos

`hkeys usuario:100`

### 8. Visualizar apenas os valores

`hvals usuario:100`

### 9. Deletar o campo estado

`hdel usuario:100 estado`

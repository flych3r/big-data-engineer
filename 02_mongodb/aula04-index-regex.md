# Aula 04 - Index e Regex

## Index e plano de execução

### 1. Mostrar todos os documentos da collection produto do banco de dados seu nome

- `db.produto.find()`

### 2. Criar o index 'query_produto' para pesquisar o campo nome do produto em ordem alfabética

- `db.produto.createIndex({nome: 1}, {name: 'query_produto'})`

### 3. Pesquisar todos os índices da collection produto

- `db.produto.getIndexes()`

### 4. Pesquisar todos os documentos da collection produto

- `db.produto.find()`

### 5. Visualizar o plano de execução do exercício 4

- `db.produto.find().explain()`

### 6. Pesquisar todos os documentos da collection produto, com uso da index 'query_produto'

- `db.produto.find().hint({nome: 1})`

### 7. Visualizar o plano de execução do exercício 7

- `db.produto.find().hint({nome: 1}).explain()`

### 8. Remover o index 'query_produto'

- `db.produto.dropIndex('query_produto')`

### 9. Pesquisar todos os índices da collection produto

- `db.produto.getIndexes()`

## Consultas com Regex

### 1. Buscar os documentos com o atributo nome,  que contenham a palavra 'cpu'

- `db.produto.find({nome: {$regex: /cpu/}})`

### 2. Buscar os documentos  com o atributo nome, que começam por 'hd' e apresentar os campos nome e qtd

- `db.produto.find({nome: {$regex: /^hd/}}, {_id: 0, nome: 1, qtd: 1})`

### 3. Buscar os documentos  com o atributo 'descricao.armazenamento', que terminam com 'GB' ou 'gb' e apresentar os campos 'nome' e 'descricao'

- `db.produto.find({'descricao.armazenamento': {$regex: /gb$/i}}, {_id: 0, nome: 1, descricao: 1})`

### 4. Buscar os documentos  com o atributo nome, que contenha a palavra memória, ignorando a letra 'o'

- `db.produto.find({nome: {$regex: /mem.ria/}})`

### 5. Buscar os documentos  com o atributo qtd  que contenham valores com letras, ao invés de números

- `db.produto.find({qtd: {$regex: /[a-z]/}})`

### 6. Buscar os documentos com o atributo descricao.sistema, que tenha exatamente a palavra 'Windows'

- `db.produto.find({'descricao.sistema': {$regex: /^Windows$/}})`

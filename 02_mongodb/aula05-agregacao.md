# Aula 05 - Agregações

## Agregações de Match, Group, Sort e Limit

### 1. Crie o banco de dados escola

- `use escola`

### 2. Crie a collection alunos no banco de dados escola

- `db.createCollection('alunos')`

### 3. Importe o arquivo '/input/exercises-data/escola/alunos.csv' para a collection alunos

- `$ mongoimport --type csv -d xavier -c alunos --headerline --drop /input/exercises-data/alunos.csv`
- `db.alunos.find()`

### 4. Visualizar os valores únicos do 'nivel' de cada 'ano_ingresso'

- `db.alunos.aggregate([{$group: {_id: '$ano_ingresso', nivel_por_ano: {$addToSet: '$nivel'}}}])`

### 5. Calcular a quantidade de alunos matriculados por cada 'id_curso'

- `db.alunos.aggregate([{$group: {_id: '$id_curso', alunos_matriculados: {$sum: 1}}}])`

### 6. Calcular a quantidade de alunos matriculados por 'ano_ingresso' no "id_curso': 1222

- `db.alunos.aggregate([{$match: {id_curso: 1222}}, {$group: {_id: '$ano_ingresso', alunos_matriculados: {$sum: 1}}}])`

### 7. Visualizar todos os documentos do 'nível': 'M'

- `db.alunos.aggregate([{$match: {nivel: 'M'}}])`

### 8. Visualizar o último ano que teve cada curso (id_curso) dos níveis 'M'

- `db.alunos.aggregate([{$match: {nivel: 'M'}}, {$group: {_id: '$id_curso', ultimo_ano: {$max: '$ano_ingresso'}}}])`

### 9. Visualizar o último ano que teve cada curso (id_curso) dos níveis 'M', ordenados pelos anos mais novos de cada curso

- `db.alunos.aggregate([{$match: {nivel: 'M'}}, {$group: {_id: '$id_curso', ultimo_ano: {$max: '$ano_ingresso'}}}, {$sort: {ultimo_ano: -1}}])`

### 10. Visualizar o último ano que teve os 5 últimos cursos (id_curso) dos níveis 'M', ordenados pelos anos mais novos

- `db.alunos.aggregate([{$match: {nivel: 'M'}}, {$group: {_id: '$id_curso', ultimo_ano: {$max: '$ano_ingresso'}}}, {$sort: {ultimo_ano: -1}}, {$limit: 5}])`

## Agregação com Lookup e project

### 1. Importe o arquivo '/input/exercises-data/escola/cursos.csv' para a collection cursos

- `mongoimport --type csv -d xavier -c cursos --headerline --drop /input/exercises-data/cursos.csv`

### 2. Realizar o left outer join da collection alunos e cursos, quando o id_curso dos 2 forem o mesmo

- `db.alunos.aggregate([{$lookup: {from: 'cursos', localField: 'id_curso', foreignField: 'id_curso', as: 'curso'}}, {$limit: 1}]).pretty()`

### 3. Realizar o left outer join da collection alunos e cursos, quando o id_curso dos 2 forem o mesmo e visualizar apenas os seguintes campos

Alunos: id_discente, nivel
Cursos: id_curso, id_unidade, nome

- `db.alunos.aggregate([{$lookup: {from: 'cursos', localField: 'id_curso', foreignField: 'id_curso', as: 'curso'}}, {$limit: 1}, {$project: {_id: 0, id_discente: 1, nivel: 1, 'curso.id_curso': 1, 'curso.id_unidade': 1, 'curso.nome': 1}}]).pretty()`

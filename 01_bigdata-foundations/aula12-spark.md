# Aula 12 - Spark

## Spark - Exercícios de Esquema e Join

### 1. Criar o DataFrame alunosDF para ler o arquivo no hdfs `/user/aluno/<nome>/data/escola/alunos.csv` sem usar as `option`

- `val alunosDF = spark.read.csv("/user/aluno/xavier/data/escola/alunos.csv")`

### 2. Visualizar o esquema do alunosDF

- `alunosDF.printSchema()`

### 3. Criar o DataFrame alunosDF para ler o arquivo `/user/aluno/<nome>/data/escola/alunos.csv` com a opção de Incluir o cabeçalho

- `val alunosDF = spark.read.option("header", "true").csv("/user/aluno/xavier/data/escola/alunos.csv")`

### 4. Visualizar o esquema do alunosDF

- `alunosDF.printSchema()`

### 5. Criar o DataFrame alunosDF para ler o arquivo `/user/aluno/<nome>/data/escola/alunos.csv` com a opção de Incluir o cabeçalho e inferir o esquema

- `val alunosDF = spark.read.option("header", "true").option("inferSchema", "true").csv("/user/aluno/xavier/data/escola/alunos.csv")`

### 6. Visualizar o esquema do alunosDF

- `alunosDF.printSchema()`

### 7. Salvar o DaraFrame alunosDF como tabela Hive `tab_alunos` no banco de dados `<nome>`

- `alunosDF.write.saveAsTable("xavier.tab_alunos")`

### 8. Criar o DataFrame cursosDF para ler o arquivo `/user/aluno/<nome>/data/escola/cursos.csv` com a opção de Incluir o cabeçalho e inferir o esquema

- `val cursosDF = spark.read.option("header", "true").option("inferSchema", "true").csv("/user/aluno/xavier/data/escola/cursos.csv")`

### 9. Criar o DataFrame alunos_cursosDF com o inner join do alunosDF e cursosDF quando o id_curso dos 2 forem o mesmo

- `val alunos_cursosDF = alunosDF.join(cursosDF, "id_curso")`

### 10. Visualizar os dados, o esquema e a quantidade de registros do alunos_cursosDF

- `alunos_cursosDF.printSchema()`
- `alunos_cursosDF.count()`
- `alunos_cursosDF.show(5)`

## Spark - Exercícios da API Catalog

### 1. Visualizar todos os banco de dados

- `spark.catalog.listDatabases().show(false)`

### 2. Definir o banco de dados `seu-nome` como principal

- `spark.catalog.setCurrentDatabase("xavier")`

### 3. Visualizar todas as tabelas do banco de dados `seu-nome`

- `spark.catalog.listTables().show()`

### 4. Visualizar as colunas da tabela tab_alunos

- `spark.catalog.listColumns("tab_alunos").show()`

### 5.  Visualizar os 10 primeiros registos da tabela `tab_alunos` com uso do `spark.sql`

- `spark.sql("SELECT * FROM tab_alunos LIMIT 10").show()`

## Spark - Exercícios de SQL Queries vs Operações de DataFrame

Realizar as seguintes consultas usando SQL queries e transformações de DataFrame na tabela `tab_alunos` no banco de dados `<nome>`

### 1. Visualizar o id_discente e nome dos 5 primeiros registros

- `spark.read.table("tab_alunos").select("id_discente", "nome").limit(5).show()`
- `spark.sql("SELECT id_discente, nome FROM tab_alunos LIMIT 5").show()`

### 2. Visualizar o id_discente, nome e ano_ingresso quando o ano de ingresso for maior ou igual a 2018

- `spark.read.table("tab_alunos").select("id_discente", "nome", "ano_ingresso").where($"ano_ingresso" >= 2018).show()`
- `spark.sql("SELECT id_discente, nome, ano_ingresso FROM tab_alunos WHERE ano_ingresso>=2018").show()`

### 3. Visualizar por ordem alfabética do nome o id_discente, nome e ano_ingresso quando o ano de ingresso for maior ou igual a 2018

- `spark.read.table("tab_alunos").select("id_discente", "nome", "ano_ingresso").where($"ano_ingresso" >= 2018).orderBy($"nome".desc).show()`
- `spark.sql("SELECT id_discente, nome, ano_ingresso FROM tab_alunos WHERE ano_ingresso>=2018 ORDER BY nome DESC").show()`

### 4. Contar a quantidade de registros do item anterior

- `spark.read.table("tab_alunos").select("id_discente", "nome", "ano_ingresso").where($"ano_ingresso" >= 2018).orderBy($"nome".desc).count()`
- `spark.sql("SELECT COUNT(*) FROM tab_alunos WHERE ano_ingresso>=2018").show()`

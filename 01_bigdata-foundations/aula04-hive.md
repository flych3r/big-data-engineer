# Aula 04 - Exercícios - Hive

## Criação de Tabela Raw

### 1. Enviar o arquivo local `/input/exercises-data/populacaoLA/populacaoLA.csv` para o diretório no HDFS `/user/aluno/<nome>/data/populacao`

- `$ docker exec namenode hdfs dfs -mkdir /user/aluno/xavier/data/populacao`
- `$ docker exec namenode hdfs dfs -put /input/exercises-data/populacaoLA/populacaoLA.csv /user/aluno/xavier/data/populacao`
- `$ docker exec namenode hdfs dfs -ls /user/aluno/xavier/data/populacao`

### 2. Listar os bancos de dados no Hive

- `$ docker exec -it hive-server bash`
- `$ beeline -u jdbc:hive2://localhost:10000`
- `$ show databases;`

### 3. Criar o banco de dados `<nome>`

- `$ create database xavier;`
- `$ use xavier;`

### 4. Criar a Tabela Hive no BD `<nome>`

```txt
Tabela interna: pop
Campos:
    zip_code - int
    total_population - int
    median_age - float
    total_males - int
    total_females - int
    total_households - int
    average_household_size - float
Propriedades:
    Delimitadores: Campo ‘,’ | Linha ‘\n’
    Sem Partição
    Tipo do arquivo: Texto
    tblproperties("skip.header.line.count"="1")
```

```sql
CREATE TABLE pop (
    zip_code INT,
    total_population INT,
    median_age FLOAT,
    total_males INT,
    total_females INT,
    total_households INT,
    average_household_size FLOAT
)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '\n'
STORED AS TEXTFILE
tblproperties("skip.header.line.count"="1");
```

### 5. Visualizar a descrição da tabela pop

- `$ desc pop;`
- `$ desc formatted pop;`

## Inserir Dados na Tabela Raw

### 1.Visualizar a descrição da tabela pop do banco de dados `<nome>`

```sql
SELECT * FROM pop;
```

### 2.Selecionar os 10 primeiros registros da tabela pop

```sql
SELECT * FROM pop LIMIT 10;
```

### 3.Carregar o arquivo do HDFS `/user/aluno/<nome>/data/populacao/populacaoLA.csv` para a tabela Hive pop

- `$ load data inpath '/user/aluno/xavier/data/populacao/populacaoLA.csv' into table pop;`

### 4.Selecionar os 10 primeiros registros da tabela pop

```sql
SELECT * FROM pop LIMIT 10;
```

### 5.Contar a quantidade de registros da tabela pop

```sql
SELECT COUNT(*) FROM pop;
```

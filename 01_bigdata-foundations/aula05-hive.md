# Aula 05 - Exercícios - Hive

## Criação de Tabela Particionada

### 1. Criar o diretório `/user/aluno/<nome>/data/nascimento` no HDFS

- `$ docker exec namenode hdfs dfs -mkdir /user/aluno/xavier/data/nascimento`

### 2. Criar e usar o Banco de dados `<nome>`

- `$ docker exec -it hive-server bash`
- `$ beeline -u jdbc:hive2://localhost:10000`
- `$ show databases;`
- `$ use xavier;`
- `$ show tables;`

### 3. Criar uma tabela externa no Hive com os parâmetros

```txt
Tabela: nascimento
Campos: nome (String), sexo (String) e frequencia (int)
Partição: ano
Delimitadores:
    Campo ‘,’
    Linha ‘\n’
Tipo do arquivo: texto
HDFS: '/user/aluno/<nome>/data/nascimento'
```

```sql
CREATE EXTERNAL TABLE nascimento (
    nome STRING,
    sexo STRING,
    frequencia INT
)
PARTITIONED BY (ano INT)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '\n'
STORED AS TEXTFILE
LOCATION '/user/aluno/xavier/data/nascimento';
```

### 4.Adicionar partição ano=2015

```sql
ALTER TABLE nascimento ADD PARTITION (ano=2015);
```

### 5.Enviar o arquivo local `input/exercises-data/names/yob2015.txt` para o HDFS no diretório `/user/aluno/<nome>/data/nascimento/ano=2015`

- `$ docker exec namenode hdfs dfs -put input/exercises-data/names/yob2015.txt /user/aluno/xavier/data/nascimento/ano=2015`

### 6.Selecionar os 10 primeiros registros da tabela nascimento no Hive

```sql
SELECT * FROM nascimento LIMIT 10;
```

### 7.Repita o processo do 4 ao 6 para os anos de 2016 e 2017

```sql
ALTER TABLE nascimento ADD PARTITION (ano=2016);
ALTER TABLE nascimento ADD PARTITION (ano=2017);
```

- `$ docker exec namenode hdfs dfs -put input/exercises-data/names/yob2016.txt /user/aluno/xavier/data/nascimento/ano=2016`
- `$ docker exec namenode hdfs dfs -put input/exercises-data/names/yob2017.txt /user/aluno/xavier/data/nascimento/ano=2017`

```sql
SELECT * FROM nascimento WHERE ano=2016 LIMIT 10;
SELECT * FROM nascimento WHERE ano=2017 LIMIT 10;
```

# Aula 06 - Hive

## Seleção de Tabelas

### 1. Selecionar os 10 primeiros registros da tabela nascimento pelo ano de 2016

```sql
SELECT * FROM nascimento WHERE ano=2016 LIMIT 10;
```

### 2. Contar a quantidade de nomes de crianças nascidas em 2017

```sql
SELECT COUNT(DISTINCT nome) FROM nascimento WHERE ano=2017;
```

### 3. Contar a quantidade de crianças nascidas em 2017

```sql
SELECT SUM(frequencia) FROM nascimento WHERE ano=2017;
```

### 4. Contar a quantidade de crianças nascidas por sexo no ano de 2015

```sql
SELECT sexo, SUM(frequencia) FROM nascimento WHERE ano=2015 GROUP BY sexo;
```

### 5. Mostrar por ordem de ano decrescente a quantidade de crianças nascidas por sexo

```sql
SELECT ano, sexo, SUM(frequencia) FROM nascimento GROUP BY ano, sexo ORDER BY ano DESC;
```

### 6. Mostrar por ordem de ano decrescente a quantidade de crianças nascidas por sexo com o nome iniciado com `A`

```sql
SELECT ano, sexo, SUM(frequencia) FROM nascimento WHERE nome LIKE 'A%' GROUP BY ano, sexo ORDER BY ano DESC;
```

### 7. Qual nome e quantidade das 5 crianças mais nascidas em 2016

```sql
SELECT nome, sum(frequencia) FROM nascimento WHERE ano=2016 ORDER BY frequencia DESC LIMIT 5;
```

### 8. Qual nome e quantidade das 5 crianças mais nascidas em 2016 do sexo masculino e feminino

```sql
SELECT nome, sexo, sum(frequencia) FROM nascimento WHERE ano=2016 ORDER BY frequencia DESC LIMIT 5;
```

## Criação de Tabelas Otimizadas

### 1. Selecionar os 10 primeiros registros da tabela pop

```sql
SELECT * from pop LIMIT 10;
```

### 2. Criar a tabela pop_parquet no formato parquet para ler os dados da tabela pop

```sql
CREATE TABLE pop_parquet (
    zip_code INT,
    total_population INT,
    median_age FLOAT,
    total_males INT,
    total_females INT,
    total_households INT,
    average_household_size FLOAT
)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '\n'
STORED AS PARQUET
tblproperties("skip.header.line.count"="1");
```

### 3. Inserir os dados da tabela pop na pop_parquet

```sql
INSERT INTO pop_parquet SELECT * from pop;
```

### 4. Contar os registros da tabela pop_parquet

```sql
SELECT COUNT(*) FROM pop_parquet;
```

### 5. Selecionar os 10 primeiros registros da tabela pop_parquet

```sql
SELECT * from pop_parquet LIMIT 10;
```

### 6. Criar a tabela pop_parquet_snappy no formato parquet com compressão Snappy para ler os dados da tabela pop

```sql
CREATE TABLE pop_parquet_snappy (
    zip_code INT,
    total_population INT,
    median_age FLOAT,
    total_males INT,
    total_females INT,
    total_households INT,
    average_household_size FLOAT
)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '\n'
STORED AS PARQUET
tblproperties("skip.header.line.count"="1", "parquet.compress"="snappy");
```

### 7. Inserir os dados da tabela pop na pop_parquet_snappy

```sql
INSERT INTO pop_parquet_snappy SELECT * from pop;
```

### 8. Contar os registros da tabela pop_parquet_snappy

```sql
SELECT COUNT(*) FROM pop_parquet_snappy;
```

### 9. Selecionar os 10 primeiros registros da tabela pop_parquet_snappy

```sql
SELECT * from pop_parquet_snappy LIMIT 10;
```

### 10. Comparar as tabelas pop, pop_parquet e pop_parquet_snappy no HDFS

- `$ docker exec namenode hdfs dfs -ls -R -h /user/hive/warehouse`

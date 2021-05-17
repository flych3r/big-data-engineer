# Aula 11 - Spark

## Spark - Exercícios de DataFrame

`$ curl -O https://repo1.maven.org/maven2/com/twitter/parquet-hadoop-bundle/1.6.0/parquet-hadoop-bundle-1.6.0.jar`
`$ docker cp parquet-hadoop-bundle-1.6.0.jar spark:/opt/spark/jars`

### 1. Enviar o diretório local `/input/exercises-data/juros_selic` para o HDFS em `/user/aluno/<nome>/data`

- `$ docker exec namenode hdfs dfs -put /input/exercises-data/juros_selic /user/aluno/xavier/data/`
- `$ docker exec namenode hdfs dfs -ls -h -R /user/aluno/xavier/data/juros_selic`

### 2. Criar o DataFrame jurosDF para ler o arquivo no HDFS `/user/aluno/<nome>/data/juros_selic/juros_selic.json`

- `$ docker exec -it spark shell`
- `$ spark-shell`
- `var jurosDF = spark.read.json("/user/aluno/xavier/data/juros_selic/juros_selic.json")`

### 3. Visualizar o Schema do jurosDF

- `jurosDF.schema`
- `jurosDF.printSchema()`

### 4. Mostrar os 5 primeiros registros do jutosDF

- `jurosDF.show(5)`

### 5. Contar a quantidade de registros do jurosDF

- `jurosDF.count`

### 6. Criar o DataFrame jurosDF10 para filtrar apenas os registros com o campo `valor` maior que 10

- `val jurosDF10 = jurosDF.filter(jurosDF("valor") > 10)`

### 7. Salvar o DataFrame jurosDF10  como tabela Hive `<nome>.tab_juros_selic`

- `jurosDF10.write.saveAsTable("xavier.tab_juros_selic")`

### 8. Criar o DataFrame jurosHiveDF para ler a tabela `<nome>.tab_juros_selic`

- `val jurosHiveDF = spark.read.table("xavier.tab_juros_selic")`

### 9. Visualizar o Schema do jurosHiveDF

- `jurosHiveDF.printSchema()`

### 10. Mostrar os 5 primeiros registros do jurosHiveDF

- `jurosHiveDF.show(5)`

### 11. Salvar o DataFrame jurosHiveDF no HDFS no diretório `/user/aluno/nome/data/save_juros` no formato parquet

- `jurosHiveDF.write.parquet("/user/aluno/xavier/data/save_juros")`

### 12. Visualizar o save_juros no HDFS

- `$ docker exec namenode hdfs dfs -ls -h -R /user/aluno/xavier/data/save_juros`

### 13. Criar o DataFrame jurosHDFS para ler o diretório do `save_juros` da questão 8

- `val jurosHDFS = spark.read.parquet("/user/aluno/xavier/data/save_juros")`

### 14. Visualizar o Schema do jurosHDFS

- `jurosHDFS.printSchema()`

### 15. Mostrar os 5 primeiros registros do jurosHDFS

- `jurosHDFS.show(5)`

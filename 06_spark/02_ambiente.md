# Aula 2 - Ambiente

## Preparar os dados e o ambiente

### 1. Configurar o jar do spark para aceitar o formato parquet em tabelas Hive

- `$ curl -O https://repo1.maven.org/maven2/com/twitter/parquet-hadoop-bundle/1.6.0/parquet-hadoop-bundle-1.6.0.jar`
- `$ docker cp parquet-hadoop-bundle-1.6.0.jar jupyter-spark:/opt/spark/jars`
- `$ rm parquet-hadoop-bundle-1.6.0.jar`

### 2. Verificar o envio dos dados para o namenode

- `$ docker exec namenode ls /input/exercises-data`

### 3. Criar no hdfs a seguinte estrutura: `/user/<aluno>/data`

- `$ docker exec namenode hdfs dfs -mkdir -p /user/xavier/data`

### 4. Enviar todos os dados do diretório input para o hdfs em `/user/<aluno>/data`

- `$ docker exec namenode hdfs dfs -put /input/exercises-data /user/xavier/data`

## Testar o Jupyter Notebook

### 1. Criar o arquivo do notebook com o nome teste_spark.ipynb

### 2. Obter as informações da sessão de spark (spark)

```python
>>> spark
SparkSession - hive

SparkContext

Spark UI

Version
v2.4.1
Master
local[*]
AppName
pyspark-shell
```

### 3. Obter as informações do contexto do spark (sc)

```python
>>> sc
SSparkContext

Spark UI

Version
v2.4.1
Master
local[*]
AppName
pyspark-shell
```

### 4. Setar o log como

```python
>>> sc.setLogLevel('INFO')
```

### 5. Visualizar todos os banco de dados com o catalog

```python
>>> spark.catalog.listDatabases()
[Database(name='default', description='Default Hive database', locationUri='hdfs://namenode:8020/user/hive/warehouse')]
```

### 6. Ler os dados "hdfs://namenode:8020/user/rodrigo/data/juros_selic/juros_selic.json“ com uso de Dataframe

```python
>>> juros_df = spark.read.json('hdfs://namenode:8020/user/xavier/data/exercises-data/juros_selic/juros_selic.json')
>>> juros_df.show(5)
+----------+-----+
|      data|valor|
+----------+-----+
|01/06/1986| 1.27|
|01/07/1986| 1.95|
|01/08/1986| 2.57|
|01/09/1986| 2.94|
|01/10/1986| 1.96|
+----------+-----+
only showing top 5 rows
```

### 7. Salvar o Dataframe como juros no formato de tabela Hive

```python
>>> juros_df.write.saveAsTable('juros_selic')
```

### 8. Visualizar todas as tabelas com o catalog

```python
>>> spark.catalog.listTables()
[Table(name='juros_selic', database='default', description=None, tableType='MANAGED', isTemporary=False)]
```

### 9. Visualizar no hdfs o formato e compressão que está a tabela juros do Hive

```python
>>> !hdfs dfs -ls -R /user/hive/warehouse
drwxr-xr-x   - root supergroup          0 2021-06-23 21:51 /user/hive/warehouse/juros_selic
-rw-r--r--   2 root supergroup          0 2021-06-23 21:51 /user/hive/warehouse/juros_selic/_SUCCESS
-rw-r--r--   2 root supergroup       3883 2021-06-23 21:51 /user/hive/warehouse/juros_selic/part-00000-d3833e7b-ac80-480f-8ce4-3d3e9af05288-c000.snappy.parquet
```

### 10. Ler e visualizar os dados da tabela juros, com uso de Dataframe no formato de Tabela Hive

```python
>>> spark.read.table('juros_selic').show(5)
+----------+-----+
|      data|valor|
+----------+-----+
|01/06/1986| 1.27|
|01/07/1986| 1.95|
|01/08/1986| 2.57|
|01/09/1986| 2.94|
|01/10/1986| 1.96|
+----------+-----+
only showing top 5 rows
```

### 11. Ler e visualizar os dados da tabela juros , com uso de Dataframe no formato Parquet

```python
>>> spark.read.parquet('/user/hive/warehouse/juros_selic/').show(5)
+----------+-----+
|      data|valor|
+----------+-----+
|01/06/1986| 1.27|
|01/07/1986| 1.95|
|01/08/1986| 2.57|
|01/09/1986| 2.94|
|01/10/1986| 1.96|
+----------+-----+
only showing top 5 rows
```

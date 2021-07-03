# Aula 08 - Structured Streaming

## Exercícios - Structured Streaming

### 1. Criar uma aplicação para ler os dados da porta 9999, contar as palavras e exibir no console

```python
from pyspark.sql.functions import explode
from pyspark.sql.functions import split

lines = spark.readStream.format('socket').option('host', 'localhost').option('port', '9999').load()
words = lines.select(explode(split(lines.value, ' ')).alias('word'))
word_counts = words.groupBy('word').count()
query = word_counts.writeStream.outputMode('complete').format('console').start()
```

### 2. Ler os arquivos csv `hdfs://namenode:8020/user/<nome>/data/iris/*.data` em modo streaming com o seguinte schema

sepal_length float, sepal_width float, petal_length float, petal_width float, class string

```python
from pyspark.sql.streaming import StructType

iris_schema = StructType()\
    .add('sepal_length', 'float')\
    .add('sepal_width', 'float')\
    .add('petal_length', 'float')\
    .add('petal_width','float')\
    .add('class', 'string')
iris_data = spark.readStream.schema(iris_schema).csv('/user/xavier/data/exercises-data/iris/*.data')
```

### 3. Visualizar o schema das informações

`iris_data.printSchema()`

### 4. Salvar os dados no diretório `hdfs://namenode:8020/user/<nome>/stream_iris/path` e o checkpoint em `hdfs://namenode:8020/user/<nome>/stream_iris/check`

```python
query = iris_data\
    .writeStream\
    .csv("csv")\
    .option("checkpointLocation", "/user/xavier/stream_iris/check")\
    .option("path", "/user/xavier/stream_iris/data")\
    .start()
```

### 5. Verificar a saida no hdfs e entender como os dados foram salvos

```python
>>> !hdfs dfs -ls /user/xavier/stream_iris/check
Found 4 items
drwxr-xr-x   - root supergroup          0 2021-07-03 18:30 /user/xavier/stream_iris/check/commits
-rw-r--r--   3 root supergroup         45 2021-07-03 18:30 /user/xavier/stream_iris/check/metadata
drwxr-xr-x   - root supergroup          0 2021-07-03 18:30 /user/xavier/stream_iris/check/offsets
drwxr-xr-x   - root supergroup          0 2021-07-03 18:30 /user/xavier/stream_iris/check/sources
>>> !hdfs dfs -ls /user/xavier/stream_iris/data
Found 3 items
drwxr-xr-x   - root supergroup          0 2021-07-03 18:30 /user/xavier/stream_iris/data/_spark_metadata
-rw-r--r--   2 root supergroup       4550 2021-07-03 18:30 /user/xavier/stream_iris/data/part-00000-2fb3c87a-bdbe-44ec-ba90-6c4bedb40c62-c000.csv
-rw-r--r--   2 root supergroup       4550 2021-07-03 18:30 /user/xavier/stream_iris/data/part-00001-40c96de3-3328-49e8-88e0-df155191b655-c000.csv
```

## Exercícios - Structured Streaming com Kafka

### 1. Ler o tópico do kafka “topic-kvspark” em modo batch

```python
topic_df = spark\
    .read\
    .format("kafka")\
    .option("kafka.bootstrap.servers", "kafka:9092")\
    .option("subscribe", "topic-kvspark”")\
    .load()
```

### 2. Visualizar o schema do tópico

```python
topic_df.printSchema()
```

### 3. Visualizar o tópico com o campo key e value convertidos em string

```python
from pyspark.sql.functions import col
topic_df.select(col('value').cast('string'), col('key').cast('string')).show()
```

### 4. Ler o tópico do kafka “topic-kvspark” em modo streaming

```python
topic_stream_df = spark\
    .readStream\
    .format("kafka")\
    .option("kafka.bootstrap.servers", "kafka:9092")\
    .option("subscribe", "topic-spark")\
    .option("startingOffsets", "earliest")\
    .load()
```

### 5. Visualizar o schema do tópico em streaming

```python
topic_stream_df.printSchema()
```

### 6. Alterar o tópico em streaming com o campo key e value convertidos para string

```python
from pyspark.sql.functions import col

topic_stream_df = topic_stream_df\
    .withColumn('key', col('key').cast('string'))\
    .withColumn('value', col('value').cast('string'))
```

### 7. Salvar o tópico em streaming no tópico topic-kvspark-output a cada 5 segundos

```python
topic_stream_output = topic_stream_df\
    .writeStream\
    .format("kafka")\
    .option("kafka.bootstrap.servers", "kafka:9092")\
    .option("topic", "topic-kvspark-output")\
    .option("checkpointLocation", '/user/xavier/kafka/topic-kvspark-output/check')\
    .trigger(continuous='5 seconds')\
    .start()
```

### 8. Salvar o tópico na pasta `hdfs://namenode:8020/user/<nome>/Kafka/topic-kvspark-output`

```python
topic_output = topic_stream_df\
    .writeStream\
    .format("csv")\
    .option("path", "/user/xavier/kafka/topic-kvspark-output")\
    .option("checkpointLocation", '/user/xavier/kafka/topic-kvspark-output/check')\
    .start()
```

# Aula 04 - Spark Schema e DataSet

## Spark Schemas

## 1. Criar o DataFrame names_us_sem_schema para ler os arquivos no HDFS `/user/<nome>/data/names`

```python
names_us_sem_schema = spark.read.csv('/user/xavier/data/exercises-data/names')
```

## 2. Visualizar o Schema e os 5 primeiros registos do names_us_sem_schema

```python
names_us_sem_schema.printSchema(), names_us_sem_schema.show(5)
```

## 3. Criar o DataFrame names_us para ler os arquivos no HDFS `/user/<nome>/data/names` com o seguinte schema

nome: String
sexo: String
quantidade: Inteiro

```python
from pyspark.sql.types import StringType, IntegerType, StructField, StructType

schema = StructType([
    StructField('nome', StringType()),
    StructField('sexo', StringType()),
    StructField('quantidade', IntegerType())
])
names_us = spark.read.csv('/user/xavier/data/exercises-data/names', schema=schema)
```

## 4. Visualizar o Schema e os 5 primeiros registos do names_us

```python
names_us.printSchema(), names_us.show(5)
```

## 5. Salvar o DataFrame names_us no formato orc no hdfs `/user/<nome>/names_us_orc`

```python
names_us.write.orc('/user/xavier/names_us_orc')
!hdfs dfs -ls /user/xavier/names_us_orc
```

## Dataset com DataFrame

### 1. Criar o DataFrame names_us para ler os arquivos no HDFS `/user/<nome>/data/names`

```java
val names_us_DF = spark.read.csv("/user/xavier/data/exercises-data/names")
```

### 2. Visualizar o Schema do names_us

```java
names_us_DF.printSchema()
```

### 3. Mostras os 5 primeiros registros do names_us

```java
names_us_DF.show(5)
```

### 4. Criar um case class Nascimento para os dados do names_us

```java
case class Nascimento(nome: String, sexo: String, quantidade: Integer)
```

### 5. Criar o Dataset names_ds para ler os dados do HDFS `/user/<nome>/data/names`, com uso do case class Nascimento

```java
import org.apache.spark.sql.Encoders

val schema_names = Encoders.product[Nascimento].schema
val names_us_DS = spark.read.schema(schema_names).csv("/user/xavier/data/exercises-data/names").as[Nascimento]
```

### 6. Visualizar o Schema do names_ds

```java
names_us_DS.printSchema()
```

### 7. Mostras os 5 primeiros registros do names_ds

```java
names_us_DS.show(5)
```

### 8. Salvar o Dataset names_ds no hdfs `/user/<nome>/ names_us_parquet` no formato parquet com compressão snappy

```java
names_us_DS.write.parquet("/user/xavier/names_us_parquet")
```

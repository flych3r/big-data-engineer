# Aula 05 - Tratamento de Dados

## 1. Criar um dataframe para ler o arquivo no HDFS `/user/<nome>/data/juros_selic/juros_selic`

```python
>>> juros_DF = spark.read.csv(
>>>     '/user/xavier/data/exercises-data/juros_selic/juros_selic',
>>>     sep=';', header=True
>>> )
>>> juros_DF.printSchema()
>>> juros_DF.show(5)
root
 |-- data: string (nullable = true)
 |-- valor: string (nullable = true)

+----------+-----+
|      data|valor|
+----------+-----+
|01/06/1986| 1,27|
|01/07/1986| 1,95|
|01/08/1986| 2,57|
|01/09/1986| 2,94|
|01/10/1986| 1,96|
+----------+-----+
```

## 2. Com o uso da função unix_timestamp crie o camp `unix_time` com a informação do campo data

```python
>>> juros_DF = juros_DF.withColumn('unix_time', unix_timestamp(column('data'), 'dd/mm/yyy'))
>>> juros_DF.show(5)
+----------+-----+---------+
|      data|valor|unix_time|
+----------+-----+---------+
|01/06/1986| 1,27|505353600|
|01/07/1986| 1,95|505440000|
|01/08/1986| 2,57|505526400|
|01/09/1986| 2,94|505612800|
|01/10/1986| 1,96|505699200|
+----------+-----+---------+
```

## 3. Com uso da função from_unixtime crie o campo `ano_unix`, com a informação do ano do campo data

```python
>>> juros_DF = juros_DF.withColumn('ano_unix', from_unixtime(column('unix_time'), 'yyyy'))
>>> juros_DF.show(5)
+----------+-----+---------+--------+
|      data|valor|unix_time|ano_unix|
+----------+-----+---------+--------+
|01/06/1986| 1,27|505353600|    1986|
|01/07/1986| 1,95|505440000|    1986|
|01/08/1986| 2,57|505526400|    1986|
|01/09/1986| 2,94|505612800|    1986|
|01/10/1986| 1,96|505699200|    1986|
+----------+-----+---------+--------+
```

## 4. Com uso de substring crie o campo ano_substr, com a informação do ano do campo `unix_time`

```python
>>> juros_DF = juros_DF.withColumn('ano_substr', substring(column('data'), -4, 10))
>>> juros_DF.show(5)
+----------+-----+---------+--------+----------+
|      data|valor|unix_time|ano_unix|ano_substr|
+----------+-----+---------+--------+----------+
|01/06/1986| 1,27|505353600|    1986|      1986|
|01/07/1986| 1,95|505440000|    1986|      1986|
|01/08/1986| 2,57|505526400|    1986|      1986|
|01/09/1986| 2,94|505612800|    1986|      1986|
|01/10/1986| 1,96|505699200|    1986|      1986|
+----------+-----+---------+--------+----------+
```

## 5. Com uso da função split crie o campo `ano_split`, com a informação do ano do campo data

```python
>>> juros_DF = juros_DF.withColumn('ano_split', split(column('data'), '/')[2])
>>> juros_DF.show(5)
+----------+-----+---------+--------+----------+---------+
|      data|valor|unix_time|ano_unix|ano_substr|ano_split|
+----------+-----+---------+--------+----------+---------+
|01/06/1986| 1,27|505353600|    1986|      1986|     1986|
|01/07/1986| 1,95|505440000|    1986|      1986|     1986|
|01/08/1986| 2,57|505526400|    1986|      1986|     1986|
|01/09/1986| 2,94|505612800|    1986|      1986|     1986|
|01/10/1986| 1,96|505699200|    1986|      1986|     1986|
+----------+-----+---------+--------+----------+---------+
```

## 6. Salvar no hdfs `/user/<nome>/data/juros_selic_americano` no formato CSV, incluindo o cabeçalho

```python
>>> juros_DF.write.csv('/user/xavier/data/juros_selic_americano', header=True)
```

### 7. Transformar as colunas valor em real e ano em inteiro

```python
>>> juros_DF = juros_DF.withColumn('valor_float', regexp_replace(column('valor'), ',', '.').cast(FloatType()))
>>> juros_DF = juros_DF.withColumn('ano', (column('ano_unix').cast(IntegerType())))
>>> juros_DF.show(5)
>>> juros_DF.printSchema()
+----------+-----+---------+--------+----------+---------+-----------+----+
|      data|valor|unix_time|ano_unix|ano_substr|ano_split|valor_float| ano|
+----------+-----+---------+--------+----------+---------+-----------+----+
|01/06/1986| 1,27|505353600|    1986|      1986|     1986|       1.27|1986|
|01/07/1986| 1,95|505440000|    1986|      1986|     1986|       1.95|1986|
|01/08/1986| 2,57|505526400|    1986|      1986|     1986|       2.57|1986|
|01/09/1986| 2,94|505612800|    1986|      1986|     1986|       2.94|1986|
|01/10/1986| 1,96|505699200|    1986|      1986|     1986|       1.96|1986|
+----------+-----+---------+--------+----------+---------+-----------+----+
root
 |-- data: string (nullable = true)
 |-- valor: string (nullable = true)
 |-- unix_time: long (nullable = true)
 |-- ano_unix: string (nullable = true)
 |-- ano_substr: string (nullable = true)
 |-- ano_split: string (nullable = true)
 |-- valor_float: float (nullable = true)
 |-- ano: integer (nullable = true)
```

## 8. Agrupar todas as datas pelo ano em ordem decrescente e salvar a quantidade de meses ocorridos, o valor médio, mínimo e máximo do campo valor

```python
>>> import pyspark.sql.functions as F
>>> juros_DF.groupBy('ano').agg(
>>>     F.count('ano').alias('Quantidade de Meses'),
>>>     F.avg('valor_float').alias('Valor Médio'),
>>>     F.min('valor_float').alias('Valor Mínimo'),
>>>     F.max('valor_float').alias('Valor Máximo'),
>>> ).orderBy('ano').show()
>>> juros_agg.show(5)
+----+-------------------+------------------+------------+------------+
| ano|Quantidade de Meses|       Valor Médio|Valor Mínimo|Valor Máximo|
+----+-------------------+------------------+------------+------------+
|1986|                  7|2.6471428189958846|        1.27|        5.47|
|1987|                 12|13.520833333333334|        7.99|       24.63|
|1988|                 12|22.733333428700764|       16.59|       30.24|
|1989|                 12|31.675833861033123|       11.43|       64.21|
|1990|                 12|25.396666447321575|        4.23|       82.04|
+----+-------------------+------------------+------------+------------+
```

## 9. Salvar no `hdfs:///user/<nome>/data/relatorioAnual` com compressão zlib e formato orc

```python
>>> juros_agg.write.orc('/user/xavier/data/relatorioAnual', compression='zlib')
```

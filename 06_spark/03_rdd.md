# Aula 3 - RDD

## 1. Ler com RDD os arquivos localmente do diretório `/opt/spark/logs/` (`file:///opt/spark/logs/`)

```python
>>> !ls /opt/spark/logs
>>> !head /opt/spark/logs/spark--org.apache.spark.deploy.master.Master-1-jupyter-notebook.out
>>> logs_df = sc.textFile('/opt/spark/logs/*')
```

## 2. Com uso de RDD faça as seguintes operações

a) Contar a quantidade de linhas

```python
logs_rdd.count()
```

b) Visualizar a primeira linha

```python
logs_rdd.first()
```

c) Visualizar todas as linhas

```python
logs_rdd.collect()
```

d) Contar a quantidade de palavras

```python
logs_words_rdd = logs_rdd.flatMap(lambda x: x.split(' '))
```

e) Converter todas as palavras em minúsculas

```python
logs_words_lower_rdd = logs_words_rdd.map(lambda x: x.lower())
```

f) Remover as palavras de tamanho menor que 2

```python
logs_words_long = logs_words_lower_rdd.filter(lambda x: len(x) > 2)
```

g) Atribuir o valor de 1 para cada palavra

```python
logs_words_assign = logs_words_long.map(lambda x: (x, 1))
```

h) Contar as palavras com o mesmo nome

```python
from operator import add

logs_words_count = logs_words_assign.reduceByKey(add)
```

i) Visualizar em ordem decrescente a quantidade de palavras

```python
logs_count_sorted = logs_words_count.sortBy(lambda x: x[1], False)
```

j) Filtrar as palavras com a quantidade de palavras > 1

```python
logs_count_filtered = logs_count_sorted.filter(lambda x: x[1] > 1)
```

k) Salvar o RDD no diretorio do HDFS `/user/<seu-nome>/logs_count_word`

```python
logs_count_filtered.saveAsTextFile('/user/xavier/logs_count_word')
```

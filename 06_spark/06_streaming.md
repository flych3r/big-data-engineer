# Aula 05 - Spark Streaming

## 1. Instalar o NetCat no container do spark

- `$ docker exec -it jupyter-spark bash`
- `$ apt update && apt install netcat -y`

## 2. Criar uma aplicação para ler os dados da porta 9999 e exibir no console

`$ netcat -lp 9999`

```python
>>> from pyspark.streaming import StreamingContext
>>> ssc = StreamingContext(sc, 5)
>>> dstream = ssc.socketTextStream('localhost', 9999)
>>> dstream.pprint()
>>> ssc.start()
....
>>> ssc.stop()

```

## 3. Criar uma aplicação para contar palavras a cada 10 segundos da porta 9998 e salvar os dados no namenode no diretório `hdfs://namenode/user/rodrigo/stream/word_count` durante 50 segundos

```python
from pyspark.streaming import StreamingContext
from operator import add
import time

ssc = StreamingContext(sc, 10)
wcstream = ssc.socketTextStream('localhost', 9998)

wcstream = wcstream \
    .flatMap(lambda x: x.split(' ')) \
    .map(lambda x: x.lower()) \
    .map(lambda x: (x, 1)) \
    .reduceByKey(add)


wcstream.pprint()
wcstream.saveAsTextFiles('/user/xavier/stream/word_count')

ssc.start()
time.sleep(50)
ssc.stop()
```

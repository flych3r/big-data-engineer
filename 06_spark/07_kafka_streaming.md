# Aula 06 - Spark Streaming e Kafka

## 1. Preparação do ambiente no Kafka

`$ docker exec -it kafka bash`

a) Criar o tópico “topic-spark” com 1 partição e o fator de replicação = 1, utilizando chave, valor

```bash
kafka-topics.sh --bootstrap-server kafka:9092 --topic topic-spark --create --partitions 1 --replication-factor 1
kafka-console-producer.sh --broker-list kafka:9092 --topic topic-spark --property parse.key=true --property key.separator=,
```

b) Criar um consumidor no Kafka para ler o “topic-spark”

`$ kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic topic-spark`

## 2. Criar um consumidor em Scala usando Spark Streaming para ler o “topic-spark” no cluster Kafka ”kafka:9092”

## 3. Visualizar o tópico com as seguintes informações: Nome do tópico, Partição e  Valor

## 4. Salvar o tópico no diretório `hdfs://namenode:8020/user/<nome>/kafka/dstream`

`$ docker exec namenode hdfs dfs -mkdir -p /user/xavier/kafka/dstream`

```python
# %%init_spark
# launcher.packages = ["org.apache.spark:spark-streaming-kafka-0-10_2.11:2.4.1"]

import org.apache.kafka.common.serialization.StringDeserializer
import org.apache.spark.streaming.{Seconds, StreamingContext}
import org.apache.spark.streaming.kafka010.KafkaUtils
import org.apache.spark.streaming.kafka010.LocationStrategies.PreferConsistent
import org.apache.spark.streaming.kafka010.ConsumerStrategies.Subscribe

val kafkaParams = Map[String, Object](
    "bootstrap.servers" -> "kafka:9092",
    "key.deserializer" -> classOf[StringDeserializer],
    "value.deserializer" -> classOf[StringDeserializer],
    "group.id" -> "app-teste",
    "auto.offset.reset" -> "earliest",
    "enable.auto.commit" -> (false: java.lang.Boolean)
)

val ssc = new StreamingContext(sc, Seconds(5))
val topics = Array("topic-spark")
val dstream = KafkaUtils.createDirectStream[String, String](
    ssc,
    PreferConsistent,
    Subscribe[String, String](topics, kafkaParams)
)

val data_dstream = dstream.map(record => (
    record.topic,
    record.partition,
    record.key,
    record.value
))

data_dstream.print()
data_dstream.saveAsTextFiles("hdfs://namenode:8020/user/xavier/kafka/dstream")

ssc.start()
...
ssc.stop()
```

# Aula 03 - Exercícios - KSQL Datagen e Schema Registry

`Acesso: http://localhost:9021/`

## KSQL Datagen

### 1. Criar o tópico users com os dados do ksql-datagen

- quickstart=users
- topic=users

- `$ docker exec -it ksql-datagen bash`
- `# ksql-datagen bootstrap-server=broker:29092 quickstart=users topic=users`

### 2. Visualizar os dados do tópico no Ksql

- `$ docker exec -it ksqldb-cli bash`
- `# ksql http://ksqldb-server:8088`
- `> print "users";`

### 3. Criar o stream users_raw com os dados do tópico users com as seguintes propriedades

- kafka_topic='users’
- value_format='JSON’
- key = 'userid’
- timestamp='viewtime’

- `> CREATE STREAM users_raw (registertime BIGINT, userid VARCHAR, regionid VARCHAR, gender VARCHAR) WITH (KEY='userid', TIMESTAMP='registertime', KAFKA_TOPIC='users', VALUE_FORMAT='JSON');`

### 4. Visualizar a estrutura da Stream  users_raw

### 5. Visualizar os dados da Stream  users_raw

- `> SELECT * FROM users_raw EMIT CHANGES;`

### 6. Repetir todo o processo para o tópico pageviews

- `# ksql-datagen bootstrap-server=broker:29092 quickstart=pageviews topic=pageviews`
- `> print "pageviwes";`
- `> CREATE STREAM pageviews_raw (viewtime BIGINT, userid VARCHAR, pageid VARCHAR) WITH (KEY='pageid', TIMESTAMP='viewtime', KAFKA_TOPIC='pageview', VALUE_FORMAT='JSON');`
- `> SELECT * FROM pageviews_raw EMIT CHANGES;`

## Schema Registry

### 1. Visualizar os dados do tópico users

### 2. Criar o tópico users-avro

- `$ docker exec -it schema-registry bash`

- Usar o kafka-avro-console-producer para enviar 1 mensagem
`# kafka-avro-console-producer --broker-list broker:29092 --topic users-avro --property schema.registry.url=http://localhost:8081 --property value.schema='{"type":"record", "name":"user","fields":[{"name":"id", "type":"int"},{"name":"idade", "type":"int"},{"name":"nome", "type":"string"}]}'`

- Usar o kafka-avro-console-consumer para consumir as mensagens
`# kafka-avro-console-consumer --bootstrap-server broker:29092 --topic users-avro --property schema.registry.url=http://localhost:8081 --from-beginning`

- Visualizar o esquema no Control Center

### 3. Visualizar os dados do users-avro no KSQL

### 4. Criar um stream users-avro1 para o tópico users-avro

### 5. Visualizar os dados do stream users-avro1

### 6. Usar o kafka-avro-console-producer para adicionar um novo campo chamado `unit` com valor padrão `1`

- `# kafka-avro-console-producer --broker-list broker:29092 --topic users-avro --property schema.registry.url=http://localhost:8081 --property value.schema='{"type":"record", "name":"user","fields":[{"name":"id", "type":"int"},{"name":"idade", "type":"int"},{"name":"nome", "type":"string"},{"name":"unit","type":"int","default":1}]}'`

### 7. Usar o kafka-avro-console-consumer para consumir as mensagens

### 8. Comparar os esquemas do users-avro no Control Center

### 9. Visualizar os dados no stream do tópico users-avro

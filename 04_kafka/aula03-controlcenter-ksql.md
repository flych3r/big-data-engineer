# Aula 03 - Exercícios - Control Center e KSQL

`Acesso: http://localhost:9021/`

## Control Center

### 1. Criar um tópico com o nome msg-rapida com 4 partições, 1 replicação e deletar os dados após 5 minutos de uso

### 2. Produzir e consumir 2 mensagens para o tópico msg-rapida

- `$ docker exec -it brooker bash`
- `# kafka-console-consumer --bootstrap-server localhost:9092 --topic msg-rapida --group app-cli`
- `# kafka-console-producer --broker-list localhost:9092 --topic msg-rapida`

### 3. Qual o nome do cluster?

controlcenter.cluster

### 4. Quantos tópicos existem no cluster?

6

### 5. Quantas partições existem o tópico msg-cli?

5

### 6. Todas as réplicas estão sincronizadas no tópico msg-cli?

sim

### 7. Qual a política de limpeza do tópico msg-cli?

delete

### 8. Alterar a política de limpeza do tópico msg-cli para deletar depois de um ano

### 9. Qual o diretório de armazenamento de logs do cluster?

/var/lib/kafka/data

### 10. Por padrão os dados são mantidos por quantos dias no Kafka?

7 dias

### 11. Visualizar os gráficos de produção e consumo de dados do tópico msg-rapida

## KSQL

### 1. Criar o tópico msg-usuario-csv

### 2. Criar um produtor para enviar 3 mensagens contendo id e nome separados por virgula para o tópico msg-usuario-csv

### 3. Visualizar os dados do tópico msg-usuario-csv

### 4. Criar o Stream usuario_csv para ler os dados do tópico msg-usuario-csv

### 5. Visualizar o Stream usuario_csv

### 6. Visualizar apenas o nome do Stream usuario_csv

# Aula 02 - Exercícios - Kafka por Linha de Comando

## 1. Criar o tópico msg-cli com 2 partições e 1 réplica

- `$ docker exec -it broker bash`
- `# kafka-topics --create --bootstrap-server localhost:9092 --partitions 2 --replication-factor 1 --topic msg-cli`

## 2. Descrever o tópico msg-cli

- `# kafka-topics --describe --bootstrap-server localhost:9092 --topic msg-cli`

## 3. Criar o consumidor do grupo app-cli

- `# kafka-console-consumer --bootstrap-server localhost:9092 --topic msg-cli --group app-cli`

## 4. Enviar as seguintes mensagens do produtor: Msg 1, Msg 2

- `$ docker exec -it broker bash`
- `# kafka-console-producer --broker-list localhost:9092 --topic msg-cli`

## 5. Criar outro consumidor do grupo app-cli

- `kafka-console-consumer --bootstrap-server localhost:9092 --topic msg-cli --group app-cli`

## 6. Enviar as seguintes mensagens do produtor: Msg 4, Msg 5, Msg 6, Msg 7

## 7. Criar outro consumidor do grupo app2-cli

- `# kafka-console-consumer --bootstrap-server localhost:9092 --topic msg-cli --group app2-cli`

## 8. Enviar as seguintes mensagens do produtor: Msg 8, Msg 9, Msg 10, Msg 11

## 9. Parar o app-cli

## 10. Definir o deslocamento para -2 offsets do app-cli

- `# kafka-consumer-groups --bootstrap-server localhost:9092 --reset-offsets --shift-by -2 --execute --topic msg-cli --group app-cli`

## 11. Descrever grupo

- `# kafka-consumer-groups --bootstrap-server localhost:9092 --execute --group app-cli --describe`

## 12. Iniciar o app-cli

- `# kafka-console-consumer --bootstrap-server localhost:9092 --topic msg-cli --group app-cli`

## 13. Redefinir o deslocamento o app-cli

- `# kafka-consumer-groups --bootstrap-server localhost:9092 --reset-offsets --to-earliest --execute --topic msg-cli --group app-cli`

## 14. Iniciar o app-cli

- `# kafka-console-consumer --bootstrap-server localhost:9092 --topic msg-cli --group app-cli`

## 15. Listar grupo

- `# kafka-consumer-groups --bootstrap-server localhost:9092 --list`

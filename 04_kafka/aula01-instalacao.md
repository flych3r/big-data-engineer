# Aula 01 - Exercícios - Instalação

## 1 - Baixar as imagem do kafka

- `$ docker-compose pull`

## 2 - Inicializar o cluster Kafka através do docker-compose

- `$ docker-compose up -d`

## 3 - Listas as imagens em execução

- `$ docker ps`

## 4 - Visualizar o log dos serviços broker e zookeeper

- `$ docker-compose logs -f broker zookeeper`

## 5 - Visualizar a interface do Confluent Control Center

- `Acesso: http://localhost:9021/`

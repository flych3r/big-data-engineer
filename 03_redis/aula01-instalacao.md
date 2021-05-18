# Aula 01 - Exercícios - Instalação

## 1 - Baixar a imagem do redis

- `$ docker-compose pull`

## 2 - Iniciar o Redis através do docker-compose

- `$ docker-compose up -d`

## 3 - Listas as imagens em execução

- `$ docker ps`

## 4 - Verificar a versão do Redis

- `$ docker exec -it redis redis-cli --version`
- `$ docker exec -it redis redis-server --version`

## 5 - Acessar o Redis CLI

- `$ docker exec -it redis redis-cli`

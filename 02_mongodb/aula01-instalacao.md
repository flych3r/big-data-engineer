# Aula 01 - Exercícios - Instalação

## 1 - Baixar as imagens do mongo e mongo-express

- `$ docker-compose pull`

## 2 - Iniciar o MongoDB através do docker-compose

- `$ docker-compose up -d`

## 3 - Listas as imagens em execução

- `$ docker ps`

## 4 - Listar os bancos de dados do MongoDB

- `$ docker exec -it mongo bash`
- `$ mongo`
- `show dbs`

## 5 - Visualizar através do Mongo Express os bancos de dados

- <http://localhost:8081/>

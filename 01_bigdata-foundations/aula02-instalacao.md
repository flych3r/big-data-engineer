# Aula 02 - Exercícios - Instalação

## Instalação Hadoop

### 1. Iniciar o cluster Hadoop através do docker-compose

- `$ docker-compose up -d`

### 2. Listas os containers em execução

- `$ docker ps`

### 3. Verificar os logs dos containers do docker-compose em execução

- `$ docker-compose logs -f`

### 4. Verificar os logs do container namenode

- `$ docker logs --tail=10 namenode`

### 5.  Acessar o container namenode

- `$ docker exec -it namenode bash`

### 6. Listar  os diretórios do container namenode

- `$ ls`

### 7. Parar os containers do Cluster de Big Data

- `$ docker-compose stop`

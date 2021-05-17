# Aula 02 - Exercícios - Instalação

## Instalação Hadoop

### 1. Instalação do docker e docker-compose

- [Docker](https://docs.docker.com/engine/install/ubuntu/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- [Adicionar ao grupo de usuário docker](https://docs.docker.com/engine/install/linux-postinstall/)

### 2. Iniciar o cluster Hadoop através do docker-compose

- `$ docker-compose up -d`

### 3. Listas os containers em execução

- `$ docker ps`

### 4. Verificar os logs dos containers do docker-compose em execução

- `$ docker-compose logs -f`

### 5. Verificar os logs do container namenode

- `$ docker logs --tail=10 namenode`

### 6.  Acessar o container namenode

- `$ docker exec -it namenode bash`

### 7. Listar  os diretórios do container namenode

- `$ ls`

### 8. Parar os containers do Cluster de Big Data

- `$ docker-compose stop`

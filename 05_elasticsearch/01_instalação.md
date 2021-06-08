# Aula 01 - Exercícios - Instalação

## 1. Baixar as imagens

- `$ docker-compose pull`

## 2. Setar na máquina o vm.max_map_count com no mínimo 262144

- `https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html#_set_vm_max_map_count_to_at_least_262144`
- `$ sudo sysctl -w vm.max_map_count=262144`

## 3. Criar a pasta de volume com permissão para acesso pelo container

- `$ sudo mkdir -p ./data`
- `sudo chmod 777 -R ./data`

## 4. Iniciar o cluster Elastic através do docker-compose

- $ docker-compose up -d`

## 5. Listas os containers em execução

- `$ docker ps`

## 6. Verificar os logs dos containers em execução

- `$ docker-compose logs -f`

## 7. Verificar as informações do cluster através do browser

- `http://localhost:9200/`

## 7. Acessar o Kibana através do browser

- `http://localhost:5601/`

# Aula 06 - Beat e Logstash

## 1. Instalar o FileBeat

```bash
curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.9.3-linux-x86_64.tar.gz

tar xzvf filebeat-7.9.3-linux-x86_64.tar.gz
```

## 2. Configurar o Filebeat para enviar o arquivo `input/dataset/paris-925.logs`  para o Logstash

```yml
# filebeat-7.9.3-linux-x86_64/filebeat.yml
...
filebeat.inputs:
...

- type: log

  # Change to true to enable this input configuration.
  enabled: true

  # Paths that should be crawled and fetched. Glob based paths.
  paths:
    # - /var/log/*.log
    - ../../input/dataset/paris-925.logs
...
# ---------------------------- Elasticsearch Output ----------------------------
# output.elasticsearch:
  # Array of hosts to connect to.
  # hosts: ["localhost:9200"]
...

# ------------------------------ Logstash Output -------------------------------
output.logstash:
  # The Logstash hosts
  hosts: ["localhost:5044"]
...
```

## 3. 2. Configurar e executar o Logstash com as seguintes configurações

```conf
input {
  beats {
    port => 5044
  }
}

output {
  stdout {
    codec => "json"
  }
  elasticsearch {
    hosts => ["elasticsearch:9200"]
    index => "paris-925-%{+yyyy.MM.dd}"
  }
}
```

## 4. Enviar os logs para o Logstash, com uso do Filebeat

```bash
cd filebeat-7.9.3-linux-x86_64
./filebeat modules list         # checar os modulos
./filebeat test config -e       # testar a configuração
./filebeat test output -e       # testar a conexão
sudo ./filebeat -e              # iniciar o filebeat
```

## 5. Verificar a quantidade de documentos do índice criado pelo Logstash e visualizar seus 10 primeiros documentos

```http
GET paris-925-2021.06.15/_search?from=0&size=10
```

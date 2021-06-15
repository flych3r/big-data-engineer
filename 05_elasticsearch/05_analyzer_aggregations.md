# Aula 05 - Analyzer e Aggregations

## Analyzer

### 1. Criar os Analyzer simple, standard, brazilian e portuguese para a seguinte frase: O elasticsearch surgiu em 2010

```http
POST _analyze
{
  "analyzer": "simple",
  "text": ["O elasticsearch surgiu em 2010"]
}

POST _analyze
{
  "analyzer": "standard",
  "text": ["O elasticsearch surgiu em 2010"]
}

POST _analyze
{
  "analyzer": "brazilian",
  "text": ["O elasticsearch surgiu em 2010"]
}

POST _analyze
{
  "analyzer": "portuguese",
  "text": ["O elasticsearch surgiu em 2010"]
}
```

### 2. Realizar os passos no índice produto

a) Criar um analyzer brazilian para o atributo descricao

```http
PUT produto1
{
  "settings": {
    "index": {
      "number_of_shards": 1,
      "number_of_replicas": 0
    }
  },
  "mappings": {
    "properties": {
      "descricao":  {
        "type": "text",
        "analyzer": "brazilian"
      }
    }
  }
}

POST _reindex
{
  "source": {
    "index": "produto"
  },
  "dest": {
    "index": "produto1"
  }
}
```

b) Para o atributo descricao aplicar o analzyer brazilian para o tipo de campo text e criar o atributo descricao.original com o dado do tipo keyword

```http
DELETE produto1

PUT produto1
{
  "settings": {
    "index": {
      "number_of_shards": 1,
      "number_of_replicas": 0
    }
  },
  "mappings": {
    "properties": {
      "descricao": {
        "type": "text",
        "analyzer": "brazilian",
        "fields": {
          "original": {
            "type": "keyword"
          }
        }
      }
    }
  }
}

POST _reindex
{
  "source": {
    "index": "produto"
  },
  "dest": {
    "index": "produto1"
  }
}
```

c) Buscar a palavra `compativel` no campo descricao.original (hits = 0)

```http
GET produto1/_search
{
  "query": {
    "match": {
      "descricao.original": "compativel"
    }
  }
}
```

d) Buscar a palavra `compativel` no campo descricao

```http
GET produto1/_search
{
  "query": {
    "match": {
      "descricao": "compativel"
    }
  }
}
```

## Aggregations

Realizar os exercícios no índice bolsa

### 1. Calcular a média do campo volume

```http
GET bolsa/_search
{
  "size": 0,
  "aggs": {
    "media_volume": {
      "avg": {
        "field": "volume"
      }
    }
  }
}
```

### 2. Calcular a estatística do campo close

```http
GET bolsa/_search
{
  "size": 0,
  "aggs": {
    "estatistica close": {
      "stats": {
        "field": "close"
      }
    }
  }
}
```

### 3. Visualizar os documentos do dia 2019-04-01 até agora. (hits = 3)

```http
GET bolsa/_search
{
  "query": {
    "range": {
      "timestamp": {
        "gte": "2019-04-01",
        "lte": "now"
      }
    }
  }
}
```

### 4. Calcular a estatística do campo open do período do dia 2019-04-01 até agora

```http
GET bolsa/_search
{
  "query": {
    "range": {
      "timestamp": {
        "gte": "2019-04-01",
        "lte": "now"
      }
    }
  },
  "size": 0,
  "aggs": {
    "estatistica open de 2019-04-01 até hoje": {
      "stats": {
        "field": "open"
      }
    }
  }
}
```

### 5. Calcular a mediana do campo open

```http
GET bolsa/_search
{
  "size": 0,
  "aggs": {
    "mediana open": {
      "percentiles": {
        "field": "open",
        "percents": [
          50
        ]
      }
    }
  }
}
```

### 6. Contar a quantidade de documentos agrupados por ano

```http
GET bolsa/_search
{
  "size": 0,
  "aggs": {
    "quantidade de documentos por ano": {
      "date_histogram": {
        "field": "timestamp",
        "calendar_interval": "year"
      }
    }
  }
}
```

### 7. Contar a quantidade de documentos de 3 anos atrás até hoje

```http
GET bolsa/_search
{
  "size": 0,
  "aggs": {
    "quantidade de documentos de 3 anos atrás até hoje": {
      "date_range": {
        "field": "@timestamp",
        "ranges": [
          {
            "from": "now-3y",
            "to": "now"
          }
        ]
      }
    }
  }
}
```

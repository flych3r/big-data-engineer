# Aula 04 - Query e Filter

## Query e Filtros

### 1. Buscar no termo nome o valor mouse

```http
GET produto/_search
{
  "query": {
    "term": {
      "nome": {
        "value": "mouse"
      }
    }
  }
}
```

### 2. Buscar no termo nome os valores mouse e teclado

```http
GET produto/_search
{
  "query": {
    "terms": {
      "nome": [
        "mouse",
        "teclado"
      ]
    }
  }
}
```

### 3. Realizar a mesma busca do item 1 e 2, desconsiderando o score

```http
GET produto/_search
{
  "query": {
    "constant_score": {
      "filter": {
        "term": {
          "nome": "mouse"
        }
      }
    }
  }
}

GET produto/_search
{
  "query": {
    "constant_score": {
      "filter": {
        "terms": {
          "nome": ["mouse", "teclado"]
        }
      }
    }
  }
}
```

### 4. Buscar os documentos que contenham a palavra `USB` no atributo descrição

```http
GET produto/_search
{
  "query": {
    "match": {
      "descricao": "USB"
    }
  }
}
```

### 5. Buscar os documentos que contenham a palavra `USB` e não contenham a palavra `Linux` no atributo descrição

```http
GET produto/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "descricao": "USB"
          }
        }
      ],
      "must_not": [
        {
          "match": {
            "descricao": "Linux"
          }
        }
      ]
    }
  }
}
```

### 6. Buscar os documentos que podem ter a palavra `memória` no atributo nome ou contenham a palavra `USB` e não contenham a palavra `Linux` no atributo descrição

```http
GET produto/_search
{
  "query": {
    "bool": {
      "should": [
        {
          "match": {
            "nome": "memória"
          }
        },
        {
          "bool": {
            "must": [
              {
                "match": {
                  "descricao": "USB"
                }
              }
            ],
            "must_not": [
              {
                "match": {
                  "descricao": "Linux"
                }
              }
            ]
          }
        }
      ]
    }
  }
}
```

## Ordem de Busca

### 1. Buscar os documentos que contenham as palavras `Windows` e `Linux` no atributo descrição

```http
GET produto/_search
{
  "query": {
    "match": {
      "descricao": {
        "query": "Linux Windows",
        "operator": "and"
      }
    }
  }
}
```

### 2. Buscar os documentos que contenham as palavras `Windows`, `Linux` ou `USB` no atributo descrição

```http
GET produto/_search
{
  "query": {
    "match": {
      "descricao": {
        "query": "Linux Windows USB",
        "operator": "or"
      }
    }
  }
}
```

### 3. Buscar os documentos que contenham pelo menos 2 palavras da seguinte lista de palavras: `Windows`; `Linux` e `USB` no atributo descrição

```http
GET produto/_search
{
  "query": {
    "match": {
      "descricao": {
        "query": "Linux Windows USB",
        "minimum_should_match": 2
      }
    }
  }
}
```

### 4. Buscar os documentos que contenham pelo menos 50 % da seguinte lista de palavras: `Windows`; `Linux` e `USB` no atributo descrição

```http
  GET produto/_search
  {
    "query": {
      "match": {
        "descricao": {
          "query": "Linux Windows USB",
          "minimum_should_match": "50%"
        }
      }
    }
  }
```

## Consultas por Intervalo

### 1. Verificar se existe o índice populacao

```HEAD populacao```

### 2. Executar as consultas no índice populacao

a) Mostrar os documentos com o atributo "Total Population" menor que 100

```http
GET populacao/_search
{
  "query": {
    "range": {
      "Total Population": {
        "lt": 100
      }
    }
  }
}
```

b) Mostrar os documentos com o atributo "Median Age" maior que 70

```http
GET populacao/_search
{
  "query": {
    "range": {
      "Median Age": {
        "gt": 70
      }
    }
  }
}
```

c) Mostrar os documentos 50 (Zip Code: 90056) à 60 (Zip Code: 90067) do índice de populacao

```http
GET populacao/_search
{
  "query": {
    "range": {
      "Zip Code": {
        "gte": 90056,
        "lte": 90067
      }
    }
  }
}
```

### 3. Importar através do Kibana o arquivo weekly_MSFT.csv com o índice bolsa

### 4. Executar as consultas no índice bolsa

a) Visualizar os documentos do dia 2019-01-01 à 2019-03-01. (hits = 9)

```http
GET bolsa/_search
{
  "query": {
    "range": {
      "timestamp": {
        "gte": "2019-01-01",
        "lte": "2019-03-01",
        "format": "yyy-MM-dd"
      }
    }
  }
}
```

b) Visualizar os documentos do dia 2019-04-01 até agora. (hits = 3)

```http
GET bolsa/_search
{
  "query": {
    "range": {
      "timestamp": {
        "gte": "2019-04-01",
        "lte": "now",
        "format": "yyy-MM-dd"
      }
    }
  }
}
```

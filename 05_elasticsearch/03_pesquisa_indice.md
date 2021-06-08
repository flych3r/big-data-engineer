# Aula 03 - API de Pesquisa e API de Índices

## Pesquisa e Paginação

### 1. Pesquisar no índice produto os documentos com os seguintes atributos

a) Nome = mouse

```http
GET /produto/_search
{
  "query": {
    "term": {
      "nome": "mouse"
    }
  }
}
```

b) Quantidade = 30

```http
GET /produto/_search
{
  "query": {
    "term": {
      "qtd": 30
    }
  }
}
```

c) Descrição = USB

```http
GET /produto/_search
{
  "query": {
    "term": {
      "descricao": "usb"
    }
  }
}
```

d) Nome = hd ou descrição = windows

```http
GET /produto/_search?q=nome:hd&q=descricao:windows
```

e) Nome = memória ou descrição = GB

```http
GET /produto/_search?q=nome:memoria&q=descricao:gb
```

### 2. Pesquisar todos os índices, limitando a pesquisa em 5 documentos em cada página e visualizar a 4 página

```http
GET _all/_search?size=5&from=15
```

## Índices

### 1. Visualizar as configurações do índice produto

```GET produto/_settings```

### 2. Visualizar o mapeamento do índice produto

```GET produto/_mapping```

### 3. Visualizar o mapeamento do atributo nome do índice produto

```GET produto/_mapping/field/nome```

### 4. Inserir o campo data do tipo date no índice produto

```http
PUT produto/_mapping/
{
  "properties": {
    "data":  { "type": "date"}
  }
}
```

### 5. Adicionar o documento

_id: 6, "nome": "teclado", "qtd": 100, "descricao": "USB", "data":"2020-09-18"

```http
POST produto/_doc/6
{
  "nome": "teclado",
  "qtd": 100,
  "descricao": "USB",
  "data": "2020-09-18"
}
```

### 6. Reindexar o índice produto para produto2, com o campo quantidade para o tipo short

```http
PUT produto2

PUT produto2/_mapping/
{
  "properties": {
    "data": {
      "type": "date"
    },
    "descricao": {
      "type": "text",
      "fields": {
        "keyword": {
          "type": "keyword",
          "ignore_above": 256
        }
      }
    },
    "nome": {
      "type": "text",
      "fields": {
        "keyword": {
          "type": "keyword",
          "ignore_above": 256
        }
      }
    },
    "qtd": {
      "type": "short"
    }
  }
}

POST _reindex
{
  "source": {
    "index": "produto"
  },
  "dest": {
    "index": "produto2"
  }
}
```

### 7. Visualizar o mapeamento do índice produto2

```GET produto2/_mapping```

### 8. Fechar o índice produto

```POST produto2/_close```

### 9. Pesquisar todos os documentos no índice produto

```GET produto2/_search```

### 10. Abrir o índice produto

```POST produto2/_open```

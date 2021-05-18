# Aula 01 - Exercícios - Estruturas de dados

## Pub Sub

### 1. Criar um assinante para receber as mensagens do canal noticias:sp

`SUBSCRIBE noticias:sp`

### 2. Criar um editor para enviar as seguintes mensagens no canal noticias:sp

- Msg 1: `PUBLISH noticias:sp 'Msg 1'`
- Msg 2: `PUBLISH noticias:sp 'Msg 2'`
- Msg 3: `PUBLISH noticias:sp 'Msg 3'`

### 3. Cancelar o assinante do canal noticias:sp

### 4. Criar um assinante para receber as mensagens dos canais com o padrão noticias:*

`SUBSCRIBE noticias:*`

### 5. Criar um editor para enviar as seguintes mensagens no canal noticias:rj

- Msg 1: `PUBLISH noticias:rj 'Msg 1'`
- Msg 2: `PUBLISH noticias:rj 'Msg 2'`
- Msg 3: `PUBLISH noticias:rj 'Msg 3'`

## Configuração

### 1. Visualizar todos os parâmetros de configuração

`config get *`

### 2. Verificar o parâmetro `appendonly`

`config get appendonly`

### 3. Remover a persistência dos dados, alterando o parâmetro `appendonly` para `no`

`config set appendonly no`

### 4. Verificar o parâmetro `save`

`config get save`

### 5. Adicionar a persistência dos dados, para a cada 2 minutos (120 segundos) se pelo menos 500 chaves forem alteradas, adicionando o parâmetro `save` para `120 500`

`config set save '120 500'`

### 6. Verificar os parâmetros `maxmemory*`

`config get maxmemory*`

### 7. Permitir que o Redis remova automaticamente os dados antigos à medida que você adiciona novos dados,  com uso da politica `allkeys-lru`, quando chegar a 1mb de memória

`config set maxmemory-policy allkeys-lru`

`config set maxmemory 1mb`

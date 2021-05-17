# Aula 03 - Exercícios - HDFS

## Comandos HDFS

### 1. Iniciar o cluster de Big Data

- `$ cd docker-bigdata`
- `$ docker-compose up -d`

### 2. Baixar os dados dos exercícios do treinamento

- `$ cd input`
- `$ sudo git clone https://github.com/rodrigo-reboucas/exercises-data.git`

### 3. Acessar o container do namenode

- `$ docker exec -it namenode bash`

### 4. Criar a estrutura de pastas apresentada a baixo pelo comando: `$ hdfs dfs -ls -R /`

```txt
user/aluno/
    <nome>/
        data/
        recover/
        delete/
```

- `$ hdfs dfs -mkdir -p /user/aluno/xavier`
- `$ hdfs dfs -mkdir /user/aluno/xavier/data`
- `$ hdfs dfs -mkdir /user/aluno/xavier/recover`
- `$ hdfs dfs -mkdir /user/aluno/xavier/delete`
- `$ hdfs dfs -ls -R /user/aluno/`

### 5. Enviar a pasta `/input/exercises-data/escola` e o arquivo `/input/exercises-data/entrada1.txt` para  `data`

- `$ hdfs dfs -put /input/exercises-data/escola /user/aluno/xavier/data`
- `$ hdfs dfs -put /input/exercises-data/entrada1.txt /user/aluno/xavier/data`
- `$ hdfs dfs -ls -R /user/aluno/`

### 6. Mover o arquivo `entrada1.txt` para `recover`

- `$ hdfs dfs -mv /user/aluno/xavier/data/entrada1.txt /user/aluno/xavier/recover`
- `$ hdfs dfs -ls -R /user/aluno/xavier/recover`

### 7. Baixar o arquivo do hdfs `escola/alunos.json` para o sistema local /

- `$ hdfs dfs -get  /user/aluno/xavier/data/escola/alunos.json /`
- `$ ls`

### 8. Deletar a pasta `recover`

- `$ hdfs dfs -rm -r /user/aluno/xavier/recover`

### 9. Deletar permanentemente o `delete`

- `$ hdfs dfs -rm -r -skipTrash /user/aluno/xavier/delete`

### 10. Procurar o arquivo `alunos.csv` dentro do `/user`

- `$ hdfs dfs -find /user -name alunos.csv`

### 11. Mostrar o último 1KB do arquivo `alunos.csv`

- `$ hdfs dfs -tail $(hdfs dfs -find  /user -name alunos.csv)`

### 12. Mostrar as 2 primeiras linhas do arquivo `alunos.csv`

- `$ hdfs dfs -cat $(hdfs dfs -find  /user -name alunos.csv) | head -n2`

### 13. Verificação de soma das informações do arquivo `alunos.csv`

- `$ hdfs dfs -checksum $(hdfs dfs -find  /user -name alunos.csv)`

### 14. Criar um arquivo em branco com o nome de `test` no `data`

- `$ hdfs dfs -touchz /user/aluno/xavier/data/test`

### 15. Alterar o fator de replicação do arquivo `test` para 2

- `$ hdfs dfs -setrep -w 2 /user/aluno/xavier/data/test`

### 16. Ver as informações do arquivo `alunos.csv`

- `$ hdfs dfs -stat $(hdfs dfs -find  /user -name alunos.csv)`
- `$ hdfs dfs -stat %r $(hdfs dfs -find  /user -name alunos.csv)`
- `$ hdfs dfs -stat %o $(hdfs dfs -find  /user -name alunos.csv)`

### 17. Exibir o espaço livre do `data` e o uso do `disco`

- `$ hdfs dfs -df -h /user/aluno/xavier/data`
- `$ hdfs dfs -du -h /`

# Aula 09 - Sqoop

## Sqoop - Importação BD Sakila – Carga Incremental

Realizar com uso do MySQL

### 1. Criar a tabela cp_rental_append, contendo a cópia da tabela rental com os campos rental_id e rental_date

```sql
CREATE TABLE cp_rental_append (rental_id INT PRIMARY KEY AUTO_INCREMENT, rental_date DATE) SELECT rental_id, rental_date FROM rental;
```

### 2 .Criar a tabela cp_rental_id e cp_rental_date, contendo a cópia da tabela cp_rental_append

```sql
CREATE TABLE cp_rental_id (rental_id INT PRIMARY KEY AUTO_INCREMENT, rental_date DATE) SELECT * FROM cp_rental_append;
CREATE TABLE cp_rental_date (rental_id INT PRIMARY KEY AUTO_INCREMENT, rental_date DATE) SELECT * FROM cp_rental_append;
```

Realizar com uso do Sqoop - Importações no warehouse /user/hive/warehouse/db_test3 e visualizar no HDFS

### 3. Importar as tabelas cp_rental_append, cp_rental_id e cp_rental_date com 1 mapeador

```bash
$ sqoop import \
--connect jdbc:mysql://database/sakila --username root --password secret \
--table cp_rental_append \
--num-mappers 1 \
--warehouse-dir /user/hive/warehouse/db_aula_9

$ sqoop import \
--connect jdbc:mysql://database/sakila --username root --password secret \
--table cp_rental_id \
--num-mappers 1 \
--warehouse-dir /user/hive/warehouse/db_aula_9

$ sqoop import \
--connect jdbc:mysql://database/sakila --username root --password secret \
--table cp_rental_date \
--num-mappers 1 \
--warehouse-dir /user/hive/warehouse/db_aula_9
```

Realizar com uso do MySQL

### 4. Executar o sql /db-sql/sakila/insert_rental.sql no container do database

- `$ docker exec -it database bash`
- `$ cd /db-sql/sakila`
- Editar arquivo `insert_rental.sql`. O `SELECT` final esta com o nome da coluna errado.
- `$ mysql -psecret < insert_rental.sql`

Realizar com uso do Sqoop - Importações no warehouse /user/hive/warehouse/db_test3 e visualizar no HDFS

### 5. Atualizar a tabela cp_rental_append no HDFS anexando os novos arquivos

```bash
$ sqoop import \
--connect jdbc:mysql://database/sakila --username root --password secret \
--table cp_rental_append \
--num-mappers 1 \
--append \
--warehouse-dir /user/hive/warehouse/db_aula_9
```

### 6. Atualizar a tabela cp_rental_id no HDFS de acordo com o último registro de rental_id, adicionando apenas os novos dados

```bash
$ sqoop eval \
--connect jdbc:mysql://database/sakila --username root --password secret \
--query "SELECT rental_id, rental_date FROM rental ORDER BY rental_id DESC LIMIT 1"

$ sqoop import \
--connect jdbc:mysql://database/sakila --username root --password secret \
--table cp_rental_id \
--num-mappers 1 \
--incremental append  --check-column rental_id --last-value 16045 \
--warehouse-dir /user/hive/warehouse/db_aula_9
```

### 7. Atualizar a tabela cp_rental_date no HDFS de acordo com o último registro de rental_date, atualizando os registros a partir desta data

```bash
$ sqoop import \
--connect jdbc:mysql://database/sakila --username root --password secret \
--table cp_rental_date \
--num-mappers 1 \
--incremental lastmodified --merge-key rental_id --check-column rental_date --last-value '2005-08-23 22:50:12.0' \
--warehouse-dir /user/hive/warehouse/db_aula_9
```

`$ hdfs dfs -ls -R -h /user/hive/warehouse/db_aula_9`

## Sqoop - Importação para o Hive e Exportação - BD Employees

### 1. Importar a tabela employees.titles do MySQL para o diretório `/user/aluno/<nome>/data` com 1 mapeador

```bash
$ sqoop import \
--connect jdbc:mysql://database/employees --username root --password secret \
--table titles \
--num-mappers 1 \
--warehouse-dir /user/aluno/xavier/data
```

### 2. Importar a tabela employees.titles do MySQL para uma tabela Hive no banco de dados seu nome com 1 mapeador

```bash
$ sqoop import \
--connect jdbc:mysql://database/employees --username root --password secret \
--table titles \
--num-mappers 1 \
--warehouse-dir /user/hive/warehouse/xavier.db/data/ \
--hive-import --create-hive-table --hive-table xavier.titles
```

### 3. Selecionar os 10 primeiros registros da tabela titles no Hive

- `$ docker exec -it hive-server bash`
- `$ beeline -u jdbc:hive2://localhost:10000`
- `show databases;`
- `use xavier;`
- `show tables;`
- `SELECT * FROM titles LIMIT 10;`

### 4. Deletar os registros da tabela employees.titles do MySQL e verificar se foram apagados, através do Sqoop

```bash
$ sqoop eval \
--connect jdbc:mysql://database/employees --username root --password secret \
--query "SELECT * FROM titles LIMIT 10"

$ sqoop eval \
--connect jdbc:mysql://database/employees --username root --password secret \
--query "DELETE FROM titles"

$ sqoop eval \
--connect jdbc:mysql://database/employees --username root --password secret \
--query "SELECT * FROM titles"
```

### 5. Exportar os dados do diretório `/user/aluno/<nome>/data/titles` para a tabela do MySQL  employees.titles

```bash
$ sqoop export \
--connect jdbc:mysql://database/employees --username root --password secret \
--table titles \
--export-dir /user/aluno/xavier/data/titles
```

### 6. Selecionar os 10 primeiros registros registros da tabela employees.titles do MySQL

```bash
$ sqoop eval \
--connect jdbc:mysql://database/employees --username root --password secret \
--query "SELECT * FROM titles LIMIT 10"
```

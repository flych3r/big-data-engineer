# Aula 08 - Sqoop

## Sqoop - Importação BD Employees

### 1. Pesquisar os 10 primeiros registros da tabela employees do banco de dados employees

```bash
$ sqoop eval \
--connect jdbc:mysql://database/employees --username root --password secret \
--query "SELECT * FROM employees LIMIT 10"
```

### 2. Realizar as importações referentes a tabela employees e para validar cada questão, é necessário visualizar no HDFS*

`$ hdfs dfs -ls -R /user/hive/warehouse`
`$ hdfs dfs -cat /user/hive/warehouse/db_test_?/nomeTabela/part-m-00000 | head -n 5`

- Importar a tabela employees, no warehouse `/user/hive/warehouse/db_test_a`

```bash
$ sqoop import \
--connect jdbc:mysql://database/employees --username root --password secret \
--table employees \
--warehouse-dir /user/hive/warehouse/db_test_a
```

- Importar todos os funcionários do gênero masculino, no warehouse `/user/hive/warehouse/db_test_b`

```bash
$ sqoop import \
--connect jdbc:mysql://database/employees --username root --password secret \
--table employees --where "gender='M'" \
--warehouse-dir /user/hive/warehouse/db_test_b
```

- Importar o primeiro e o último nome dos funcionários com os campos separados por tabulação, no warehouse  `/user/hive/warehouse/db_test_c`

```bash
$ sqoop import \
--connect jdbc:mysql://database/employees --username root --password secret \
--table employees --columns "first_name,last_name" \
--fields-terminated-by "\t" \
--warehouse-dir /user/hive/warehouse/db_test_c
```

- Importar o primeiro e o último nome dos funcionários com as linhas separadas por ` : ` e salvar no mesmo diretório da questão anterior

```bash
$ sqoop import \
--connect jdbc:mysql://database/employees --username root --password secret \
--table employees --columns "first_name,last_name" \
--lines-terminated-by " : " \
--warehouse-dir /user/hive/warehouse/db_test_c
```

## Sqoop - Importação BD Employees- Otimização

Realizar com uso do MySQL

### 1. Criar a tabela cp_titles_date, contendo a cópia da tabela titles com os campos title e to_date

```sql
CREATE TABLE cp_titles_date (title TEXT, to_date DATE) SELECT title, to_date FROM titles;
```

### 2. Pesquisar os 15 primeiros registros da tabela cp_titles_date

```sql
SELECT * FROM cp_titles_date LIMIT 15;
```

### 3. Alterar os registros do campo to_date para null da tabela cp_titles_date, quando o título for igual a Staff

```sql
UPDATE cp_titles_date SET to_date=NULL WHERE title='Staff';
```

Realizar com uso do Sqoop - Importações no warehouse /user/hive/warehouse/db_test_<numero_questao> e visualizar no HDFS

### 4. Importar a tabela titles com 8 mapeadores no formato parquet

```bash
$ sqoop import \
--connect jdbc:mysql://database/employees --username root --password secret \
--table titles \
--as-parquetfile \
--num-mappers 8 \
--warehouse-dir /user/hive/warehouse/db_test_4
```

### 5. Importar a tabela titles com 8 mapeadores no formato parquet e compressão snappy

```bash
$ sqoop import \
--connect jdbc:mysql://database/employees --username root --password secret \
--table titles \
--as-parquetfile \
--compress --compression-codec org.apache.hadoop.io.compress.SnappyCodec \
--num-mappers 8 \
--warehouse-dir /user/hive/warehouse/db_test_5
```

### 6. Importar a tabela cp_titles_date com 4 mapeadores (erro)

```bash
$ sqoop import \
--connect jdbc:mysql://database/employees --username root --password secret \
--table cp_titles_date \
--num-mappers 4 \
--warehouse-dir /user/hive/warehouse/db_test_6
```

- Importar a tabela cp_titles_date com 4 mapeadores divididos pelo campo título no warehouse `/user/hive/warehouse/db_test2_title`

```bash
$ sqoop import \
-Dorg.apache.sqoop.splitter.allow_text_splitter=true \
--connect jdbc:mysql://database/employees --username root --password secret \
--table cp_titles_date \
--num-mappers 4 --split-by title \
--warehouse-dir /user/hive/warehouse/db_test2_title
```

- Importar a tabela cp_titles_date com 4 mapeadores divididos pelo campo data no warehouse `/user/hive/warehouse/db_test2_date`

```bash
$ sqoop import \
--connect jdbc:mysql://database/employees --username root --password secret \
--table cp_titles_date \
--num-mappers 4 --split-by to_date \
--warehouse-dir /user/hive/warehouse/db_test2_date
```

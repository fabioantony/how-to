####O que é?

MySql não é um banco de dados é Um SGBD, (Sistema de Gerenciamento de Banco de Dados) isso significa que pode gerenciar varios bancos de dados diferentes.

####Semantica

Um banco de dados também é chamado de schema

Cada Linha numa tabela de banco de dados é considerada um registro ou tupla.

ID ou PK é o indentificador único ou também chamado de primary key (chave primaria)

Um id em uma tabela que faz referencia a uma tabela externa é chamado de chave estrangeira.

#### Tipos de Dados

Numéricos

* tinyint : inteiro -128 até 127
* bool: Zerou ou Um 
* smallint: -32768 até 32767
* mediumint: -8388608 até 8388607
* Int: -2147483648 até 2147483647
* bigint: -9223372036854775808 até 9223372036854775807
* float: com ponto decimal
* double: numeros reais (utilizado para contas muito precisas)
* decimal:

Data

* Date: Data
* DateTime: Data e hora
* TimeStamp: Data e hora 1/01/1970 até 2037 usado para gerar hora da maquina.
* Time: Somente hora
* year: Somente ano

Cadeia

* char: use quando tem certeza do tamanho exato do dado.
* varchar: tamanho variavel mas usa no máximo o configurado.
* text
* longText
* enum

#### Comandos Banco De Dados

motrar bancos de dados

	show databases;
	
Garantindo super administrador para um usuário

	grant all privileges on *.* to USUARIO@localhost identified by 'SENHA' with grant option;
	
Removendos todos os privilegios de um usuário

	revoke all privileges on *.* from exemplo@localhost;
	
Criar um banco de dados 

	create schema `NOME_DO_BANCO` ;
	//ou
	create database `NOME_DO_BANCO`;
	
Apagar um banco de dados
	
	drop schema `NOME_DO_BANCO`;
	//ou
	drop database `NOME_DO_BANCO`;
	
####Comandos Tabelas
	
Criando uma Tabela

	CREATE TABLE `NomeBanco`.`NomeTabela` (
	  `id` INT NOT NULL,
	  `nome` VARCHAR(45) NOT NULL,
	  `email` VARCHAR(100) NOT NULL,
	  PRIMARY KEY (`id`));
	  
Alterar a Tabela (tornamos o campo emial unico)

	ALTER TABLE `NomeBanco`.`NomeTabela` 
	ADD UNIQUE INDEX `email_UNIQUE` (`email` ASC);
	
Alterar a tabela (adicionamos uma PK)

	ALTER TABLE `NomeBanco`.`NomeTabela` 
	ADD PRIMARY KEY (`id`);
	
Criando uma tabela com chave estrangeira

	create table pedidos (
	    `id` INT PRIMARY KEY,
	    `produto_id` int,
	    `quantidade` int,
	     FOREIGN KEY (produto_id) REFERENCES products(id)
	);
	
Adicionando uma coluna  a uma tabela 

ex1 .:

	Alter table pedidos add column total double;

ex2.:

	alter table clientes add column cpf varchar(20);
	
Removendo uma tabela uma coluna 

	Alter table pedidos drop column total;
	
Definindo um valor padrão para uma coluna

	alter table pedidos alter column total set default 0;
	
Removendo um valor padrão de uma coluna

	alter table pedidos alter column total drop default;
	
Criando uma constraints

	alter table clientes add constraint cpf_unico unique(cpf);
	alter table clientes modify cpf varchar(14) not null;
	
Removendo constraint

	alter table clientes drop index cpf_unico;
		

#### Comandos Registros
	
Inserindo um registro

	insert into clientes (coluna1, coluna2, coluna3)
	values(valor1, valor2, valor3);
	
Inserindo registro caso saiba a ordem das colunas

	insert into clientes
	values(valor1, valor2, valor3);
	
Atualizando todos os registros onde o id é 1 (como o id é unico sera em apenas um lugar)

	update clientes set
	nome='mario da silva',
	email='emailatual@email.com'
	where id=1;
	
deletando registro

	delete from nometabela where id=2;
	
Selecionando todos os registros

	select * from nome_da_tabela;
	
Tranzendo apenas as colunas id e nome

```sql
select id, nome from nome_da_tabela;
```
	
Atribuindo Apelidos
	
```sql
select id as 'codigo', nome from nome_da_tabela;
```

O Distinct agrupa registro iguais no caso abaixo ele só vai retornar uma vez um o uf mesmo que tenha mais de um registro no banco de dados. 

```sql
select distinct uf from clientes;
```

Trazer registros de A a Z

	select * from clientes order by nome asc;

Trazer registro de Z a A

	select * from clientes order by nome desc;
	
Pode-se usar where em combinação com orde by

	select * from clientes where uf='sp' order by nome desc;
	
Where Diferente, no exemplo abaixo pega todos os registros onde uf é diferente de sp

	select * from clientes where uf<>'sp' order by nome desc;

Where Maior que
	
	select * from clientes where id>30 order by nome desc;

Where Menor que 

```sql
select * from clientes where id<30 order by nome desc;
```

Range com Where

```sql
SELECT * FROM products
where preco > 80 and preco < 120; 
```

```sql
	SELECT * FROM products
	where (preco >  80) and (preco < 120); 
```

```sql
	SELECT * FROM products
	where (preco >  80) or (preco < 120); 
```

Usando Linke, neste exemplo seleciona todos os produtos que começam com produto

```sql
SELECT * FROM products
where nome like '%Produto%';
```

Usando Linke, neste exemplo seleciona todos os produtos que terminam com produto

```sql
SELECT * FROM products
where nome like '%Produto%';
```

Usando Linke, neste exemplo seleciona todos os produtos que contem Produto ordenados do mais mais cara para o mais barato

```sql
SELECT * FROM products
where nome like '%Produto%'
order by preco desc ;
```
	
#### Notações

NN = Not Null;
UQ = Unique;

#### Funções

Retorna o numero total de registros da tabela produto produto

	select count (*) from produtos;
	
Retorna o o retistro de maio valor

	select max(preco) from produtos;
	
Retorna o registro de menor valor

	select max(preco) from produtos;
	
Retorna o valor medio de todos os produtos

	select  avg(preco) from produtos;
	
Retorna a soma do valor de todos os produtos

	select sum(preco) from produtos;
	
Pegando a quantidade de registro de cada estado

	select uf, count(*) from clients group by uf;
	
#### Operadores

O Where aceita os seguintes operadores

	> maior que
	< menor que
	+ somar
	- diminuir
	/ dividir
	= igual
	<> ou != diferente
	>= maior igual
	<= menor igual
	
#### Geral 

Chave primária vrs Unica, ambar não permiterm a repetição de um dado no banco de dados. Mas a chave primaria ajuda a fazer relações com outras tabelas.

#### Relacionamento de Tabelas

Considere que na tabela pedidos ha uma chave estrangeira referente ao um produto na tabela produtos.

	select * from pedidos
	
Pega todos os dados da tabela pedidos e a coluna nome da tabela produtos.

	select *, nome from pedidos, produtos;


Mesma query acima mas agora relacionamos o a chave estrangeira em uma tabela a chave primaria de outra

	select *, nome from pedidos, produtos where pedidos.produtos_id = produto.id
	
Para evitar exesso de campus pode-se estabelecer as colunas de retorno

	select pedidos.id, nome, quantidade, total from pedidos, produtos 
	where pedidos.produto_id = produto.id
	
Também podemos estabelecer aliases

	select pedidos.id, nome, quantidade, preco as 'valor unitario', total from pedidos, produtos 
	where pedidos.produto_id = produto.id
	
	
#### Procedure

Listar todas

	select * from INFORMATION_SCHEMA.ROUTINES;
	
Criando o uma procedure esqueleto

	DELIMITER &&
	
	CREATE PROCEDURE NOMEDAPROCEDURE()
	BEGIN
		Select 'minha primeira rotina';
	END && 	
	
	DELIMITER ;
	

	
Chamando uma rotina

	CALL NOMEDAPROCEDURE()
	
Quando estiver usando uma procedure pode  usar o if exist

	DELIMITER &&
	
	DROP PROCEDURE IF EXISTS NOMEDAPROCEDURE&&
	
	CREATE PROCEDURE NOMEDAPROCEDURE()
	BEGIN
		Select 'minha primeira rotina';
	END && 	
	
	DELIMITER ;
	
#### Views

criando uma view

	CREATE VIEW v_clientes_sp AS
	select * from clientes where uf='SP';
	
Usando uma view

	select * from v_clientes_sp;
	
	
#### Listando Índices

Mostrando índices de uma tabela.

    SHOW INDEX FROM film FROM sakila;
    
Mostra os index do banco sakala na tabela film.

_Cardinality_: Número de valores diferente na coluna.

Dentro do contexto do banco podemos usar apenas.

    use sakila;
    SHOW INDEX FROM film;
    
Ou ainda podemos usar

    SELECT * FROM INFORMATION_SCHEMA.STATISTICS WHERE
    TABLE_NAME = 'filme';   

O "INFORMATION_SCHEMA.STATISTICS" é um banco de dados somente leitura com metadados dos outros bancos de dados. 
    
#### Criando Índices

Cria o idex de nome "idx_film_length" na tabela "film" com base na coluna "length";

    CREATE INDEX idx_film_length ON film (length);
    
Seleciona os campos "film_id", e "length" da tabela "film", onde length é 100;
    
    SELECT film_id, length FROM film WHERE lenght = 100;

#### Analisando Índices
    
O camando "EXPLAIN" mostra algumas meta informações sobre a query:

_key_ - Chaves que o MySql usuara para trazer o resultado;

_possible_keys_ - Possivéis chaves (sugestão).

_rows_ - Estimativa do números de linhas que examinou até achar o resultado.

_Extra_ - Se _Using_index_ usou apenas o indice para analisar o resultado. 
    
    EXPLAIN SELECT film_id, length FROM film WHERE length = 100;

#### Apaga Index
    
    DROP Index idx_film_length ON film;
    
#### Ordem dos resultados nos índices

Cria o index para percorrer os dados de form ascendente

    CREATE INDEX idx_film_length ON film (length);

Cria o index para percorrer os dados de forma descendente

    CREATE INDEX idx_film_length_desc ON film (length DESC)
    
No esquema BTREE o MySql irá ignorar a ordenação desc de index, ambos os indeces funcioram de maneira igual sendo necessário estabelecer apenas um deles.

#### ÍNDICE DE DUAS COLUNAS
    
    CREATE INDEX idx_film_rental_duration_length ON film (rental_duration, length);

A ordem das tabelas não faz diferença.    

#### UNION
	
	select * from customers union select * from series;
	+----+-----------------------+-----------+------------------+
	| id | first_name            | last_name | email            |
	+----+-----------------------+-----------+------------------+
	|  1 | Boy                   | George    | george@gmail.com |
	|  2 | George                | Michael   | gm@gmail.com     |
	|  3 | David                 | Bowie     | david@gmail.com  |
	|  4 | Blue                  | Steele    | blue@gmail.com   |
	|  5 | Bette                 | Davis     | bette@aol.com    |
	|  1 | Archer                | 2009      | Animation        |
	|  2 | Arrested Development  | 2003      | Comedy           |
	|  3 | Bob's Burgers         | 2011      | Animation        |
	|  4 | Bojack Horseman       | 2014      | Animation        |
	|  5 | Breaking Bad          | 2008      | Drama            |
	|  6 | Curb Your Enthusiasm  | 2000      | Comedy           |
	|  7 | Fargo                 | 2014      | Drama            |
	|  8 | Freaks and Geeks      | 1999      | Comedy           |
	|  9 | General Hospital      | 1963      | Drama            |
	| 10 | Halt and Catch Fire   | 2014      | Drama            |
	| 11 | Malcolm In The Middle | 2000      | Comedy           |
	| 12 | Pushing Daisies       | 2007      | Comedy           |
	| 13 | Seinfeld              | 1989      | Comedy           |
	| 14 | Stranger Things       | 2016      | Drama            |
	+----+-----------------------+-----------+------------------+
	19 rows

#### Relatórios

    SELECT customerName, COUNT(*) AS 'number of orders'
    FROM customers INNER JOIN orders 
    ON orders.customerID = customers.customerID 
    GROUP BY customers.customerID;
    
Ira exibir um lista com nome de usuario e o numero de de pedidos do mesmo.    

#### Saindo do Cli 
    quit;
    exit;
    \q;
    ctrl+c
   
#### Help

Comandos
   
    help;
    
Categorias de ajuda    
   
    help contents;
    
Combinação do comando help com uma categoria
 
    help data type;  
    
    
#### Mostrando Databases

    show database;
    
#### Hostname

    select @@hostname;
    
#### Criar banco de dados

    CREATE DATABASE soap_store;
    CREATE DATABASE DogApp;
    CREATE DATABASE  <name>;
    
#### Deletar banco de dados

    DROP DATABASE <name>;
        
#### Estabelecendo base de trabalho

Estabelece base de trabalho padrão;

    USE <database>;
    
Mostra a base de trabalho padrão;
    
    SELECT database();          
    
#### Criar tabela

    CREATE TABLE <tablename>
    (
        column_name data_type,
        column_name data_type
    );
    
    CREATE TABLE cats
    (
        name VARCHAR(100),
        age INT
    );
    
#### Mostrar colunas de uma tabela

    SHOW COLUMNS FROM <tablename>;
    
ou use describe

    DESC <tablename>;
      
#### Deletar tabela;

    DROP TABLE <tablename>;
    
#### Mostar tabelas de um banco de dados

    SHOW TABLES <database>;
    
#### Inserindo dados

    INSERT INTO tablename(column1, column1) VALUES ('value1', 10);
    
####Listando dados de uma tabela
   
    SELECT * FROM <tablename>
                  
#### Mostrando warnings

    show warnings;
    
#### Definindo valores não nulos

    create table tablename 
    (
        column varchar(100) not null,
    );
    
#### Valores padrões

    create table cats3
    (
        name varchar(100) default 'unnamed',
        age int default 99;
    );
    
Valores não nulos    
    
    create table cats3
    (
        name varchar(100) not null default 'unnamed',
        age int default 99;
    ); 
    
#### Chaves e Identificador único

    create table unique_cats
    (
        cat_id int not null,
        name varchar(50)
        age int,
        primary key (cat_id) 
    );

Identificador com icrementação automatica

    create table unique_cats2
    (
        cat_id int not null auto_increment,
        name varchar(50)
        age int,
        primary key (cat_id) 
    );
    
    create table employees
    (
        id int not null auto_increment,
        last_name varchar(50) not null,
        first_name varchar(50) not null,
        middle_name varchar(50),
        age int not null,
        current_status varchar(50) not null default 'employed',
        primary key (id)
    );
    
## Crud

#### Create - Inserindo uma ou multiplas linhas

    INSERT INTO cats(name, age) 
    VALUES 
    ('Peanut', 2)
    , ('Butter', 4)
    , ('Jelly', 7);
    
    INSERT INTO table_name 
                (column_name, column_name) 
    VALUES      (value, value), 
                (value, value), 
                (value, value);

#### Read

Lista

    Select * From cats;
    select name from cats;
    select name, age from cats;
        
Pegar apenas um elemento

    select * from cats where cat_id=5;
    
Pegar varios elemeots

    select * from cats where age=5
    
Podemos comparar diretamente duas colunas em vez de passar um valor.

    select * from cats where age=cat_id;
    
Aliases facilitam a leitura dos resultados,

    desc cats;
    +--------+--------------+------+-----+---------+----------------+
    | Field  | Type         | Null | Key | Default | Extra          |
    +--------+--------------+------+-----+---------+----------------+
    | cat_id | int(11)      | NO   | PRI | NULL    | auto_increment |
    | name   | varchar(100) | YES  |     | NULL    |                |
    | breed  | varchar(100) | YES  |     | NULL    |                |
    | age    | int(11)      | YES  |     | NULL    |                |
    +--------+--------------+------+-----+---------+----------------+

    select * from cats;
    +--------+----------------+------------+------+
    | cat_id | name           | breed      | age  |
    +--------+----------------+------------+------+
    |      1 | Ringo          | Tabby      |    4 |
    |      2 | Cindy          | Maine Coon |   10 |
    |      3 | Dumbledore     | Maine Coon |   11 |
    |      4 | Egg            | Persian    |    4 |
    |      5 | Misty          | Tabby      |   13 |
    |      6 | George Michael | Ragdoll    |    9 |
    |      7 | Jackson        | Sphynx     |    7 |
    +--------+----------------+------------+------+

    select cat_id as id, name from cats; 
    +----+----------------+
    | id | name           |
    +----+----------------+
    |  1 | Ringo          |
    |  2 | Cindy          |
    |  3 | Dumbledore     |
    |  4 | Egg            |
    |  5 | Misty          |
    |  6 | George Michael |
    |  7 | Jackson        |
    +----+----------------+
    
    select name as 'cat name', breed as 'kitty breed' from cats;
    +----------------+-------------+
    | cat name       | kitty breed |
    +----------------+-------------+
    | Ringo          | Tabby       |
    | Cindy          | Maine Coon  |
    | Dumbledore     | Maine Coon  |
    | Egg            | Persian     |
    | Misty          | Tabby       |
    | George Michael | Ragdoll     |
    | Jackson        | Sphynx      |
    +----------------+-------------+ 

#### Update

    mysql> select * from cats;
    +--------+----------------+------------+------+
    | cat_id | name           | breed      | age  |
    +--------+----------------+------------+------+
    |      1 | Ringo          | Tabby      |    4 |
    |      2 | Cindy          | Maine Coon |   10 |
    |      3 | Dumbledore     | Maine Coon |   11 |
    |      4 | Egg            | Persian    |    4 |
    |      5 | Misty          | Tabby      |   13 |
    |      6 | George Michael | Ragdoll    |    9 |
    |      7 | Jackson        | Sphynx     |    7 |
    +--------+----------------+------------+------+
    
     mysql> update cats set breed='Shorthair' where breed='tabby';
     
     mysql> select * from cats;
     +--------+----------------+------------+------+
     | cat_id | name           | breed      | age  |
     +--------+----------------+------------+------+
     |      1 | Ringo          | Shorthair  |    4 |
     |      2 | Cindy          | Maine Coon |   10 |
     |      3 | Dumbledore     | Maine Coon |   11 |
     |      4 | Egg            | Persian    |    4 |
     |      5 | Misty          | Shorthair  |   13 |
     |      6 | George Michael | Ragdoll    |    9 |
     |      7 | Jackson        | Sphynx     |    7 |
     +--------+----------------+------------+------+
     
    mysql> update cats set age=14 where name='Misty';
    
     mysql> select * from cats;
     +--------+----------------+------------+------+
     | cat_id | name           | breed      | age  |
     +--------+----------------+------------+------+
     |      1 | Ringo          | Shorthair  |    4 |
     |      2 | Cindy          | Maine Coon |   10 |
     |      3 | Dumbledore     | Maine Coon |   11 |
     |      4 | Egg            | Persian    |    4 |
     |      5 | Misty          | Shorthair  |   14 |
     |      6 | George Michael | Ragdoll    |    9 |
     |      7 | Jackson        | Sphynx     |    7 |
     +--------+----------------+------------+------+
     
     mysql> update cats set age=15, name='John' where name='Misty';

#### Delete

    mysql> select * from cats;
    +--------+----------------+-------------------+------+
    | cat_id | name           | breed             | age  |
    +--------+----------------+-------------------+------+
    |      1 | Ringo          | British Shorthair |    4 |
    |      2 | Cindy          | Maine Coon        |   12 |
    |      3 | Dumbledore     | Maine Coon        |   12 |
    |      4 | Egg            | Persian           |    4 |
    |      5 | Misty          | Shorthair         |   14 |
    |      6 | George Michael | Ragdoll           |    9 |
    |      7 | Jack           | Sphynx            |    7 |
    +--------+----------------+-------------------+------+
    
    mysql> delete from cats where name = 'Egg';    
    
    mysql> select * from cats;
    +--------+----------------+-------------------+------+
    | cat_id | name           | breed             | age  |
    +--------+----------------+-------------------+------+
    |      1 | Ringo          | British Shorthair |    4 |
    |      2 | Cindy          | Maine Coon        |   12 |
    |      3 | Dumbledore     | Maine Coon        |   12 |
    |      5 | Misty          | Shorthair         |   14 |
    |      6 | George Michael | Ragdoll           |    9 |
    |      7 | Jack           | Sphynx            |    7 |
    +--------+----------------+-------------------+------+
    
    mysql> delete from cats where age=4; 
    
    mysql> select * from cats;
    +--------+----------------+------------+------+
    | cat_id | name           | breed      | age  |
    +--------+----------------+------------+------+
    |      2 | Cindy          | Maine Coon |   12 |
    |      3 | Dumbledore     | Maine Coon |   12 |
    |      5 | Misty          | Shorthair  |   14 |
    |      6 | George Michael | Ragdoll    |    9 |
    |      7 | Jack           | Sphynx     |    7 |
    +--------+----------------+------------+------+
    
    mysql> delete from cats where age=cat_id;
    
    mysql> select * from cats;
    +--------+----------------+------------+------+
    | cat_id | name           | breed      | age  |
    +--------+----------------+------------+------+
    |      2 | Cindy          | Maine Coon |   12 |
    |      3 | Dumbledore     | Maine Coon |   12 |
    |      5 | Misty          | Shorthair  |   14 |
    |      6 | George Michael | Ragdoll    |    9 |
    +--------+----------------+------------+------+
    

Truncate ira resetar os ids fazendo com novos registros tenham começem pelo id 1;
    
    mysql> truncate cats; 
    or 
    mysql> delete from cats;
    
    mysql> select * from cats;
    Empty set (0.00 sec)

#### Executando SQL a partir de um arquivo

    mysql> source teste.sql;
    
    
## Funções de string

#### concat()

    mysql> select concat('Hello', '...', 'World');
    +---------------------------------+
    | concat('Hello', '...', 'World') |
    +---------------------------------+
    | Hello...World                   |
    +---------------------------------+

    select * from books;
    +---------+-----------------------------------------------------+--------------+----------------+---------------+----------------+-------+
    | book_id | title                                               | author_fname | author_lname   | released_year | stock_quantity | pages |
    +---------+-----------------------------------------------------+--------------+----------------+---------------+----------------+-------+
    |       1 | The Namesake                                        | Jhumpa       | Lahiri         |          2003 |             32 |   291 |
    |       2 | Norse Mythology                                     | Neil         | Gaiman         |          2016 |             43 |   304 |
    |       3 | American Gods                                       | Neil         | Gaiman         |          2001 |             12 |   465 |
    |       4 | Interpreter of Maladies                             | Jhumpa       | Lahiri         |          1996 |             97 |   198 |
    |       5 | A Hologram for the King: A Novel                    | Dave         | Eggers         |          2012 |            154 |   352 |
    |       6 | The Circle                                          | Dave         | Eggers         |          2013 |             26 |   504 |
    |       7 | The Amazing Adventures of Kavalier & Clay           | Michael      | Chabon         |          2000 |             68 |   634 |
    |       8 | Just Kids                                           | Patti        | Smith          |          2010 |             55 |   304 |
    |       9 | A Heartbreaking Work of Staggering Genius           | Dave         | Eggers         |          2001 |            104 |   437 |
    |      10 | Coraline                                            | Neil         | Gaiman         |          2003 |            100 |   208 |
    |      11 | What We Talk About When We Talk About Love: Stories | Raymond      | Carver         |          1981 |             23 |   176 |
    |      12 | Where I'm Calling From: Selected Stories            | Raymond      | Carver         |          1989 |             12 |   526 |
    |      13 | White Noise                                         | Don          | DeLillo        |          1985 |             49 |   320 |
    |      14 | Cannery Row                                         | John         | Steinbeck      |          1945 |             95 |   181 |
    |      15 | Oblivion: Stories                                   | David        | Foster Wallace |          2004 |            172 |   329 |
    |      16 | Consider the Lobster                                | David        | Foster Wallace |          2005 |             92 |   343 |
    +---------+-----------------------------------------------------+--------------+----------------+---------------+----------------+-------+
    
    mysql> SELECT CONCAT (author_fname, ' ', author_lname) FROM books;
    +------------------------------------------+
    | CONCAT (author_fname, ' ', author_lname) |
    +------------------------------------------+
    | Jhumpa Lahiri                            |
    | Neil Gaiman                              |
    | Neil Gaiman                              |
    | Jhumpa Lahiri                            |
    | Dave Eggers                              |
    | Dave Eggers                              |
    | Michael Chabon                           |
    | Patti Smith                              |
    | Dave Eggers                              |
    | Neil Gaiman                              |
    | Raymond Carver                           |
    | Raymond Carver                           |
    | Don DeLillo                              |
    | John Steinbeck                           |
    | David Foster Wallace                     |
    | David Foster Wallace                     |
    +------------------------------------------+
    
    mysql> SELECT CONCAT (author_fname, ' ', author_lname) AS full_name FROM books;
    +----------------------+
    | full_name            |
    +----------------------+
    | Jhumpa Lahiri        |
    | Neil Gaiman          |
    | Neil Gaiman          |
    | Jhumpa Lahiri        |
    | Dave Eggers          |
    | Dave Eggers          |
    | Michael Chabon       |
    | Patti Smith          |
    | Dave Eggers          |
    | Neil Gaiman          |
    | Raymond Carver       |
    | Raymond Carver       |
    | Don DeLillo          |
    | John Steinbeck       |
    | David Foster Wallace |
    | David Foster Wallace |
    +----------------------+
    
    mysql>     SELECT author_fname AS first, author_lname AS last, 
        ->     CONCAT(author_fname, ' ', author_lname) AS full
        ->     FROM books;
    +---------+----------------+----------------------+
    | first   | last           | full                 |
    +---------+----------------+----------------------+
    | Jhumpa  | Lahiri         | Jhumpa Lahiri        |
    | Neil    | Gaiman         | Neil Gaiman          |
    | Neil    | Gaiman         | Neil Gaiman          |
    | Jhumpa  | Lahiri         | Jhumpa Lahiri        |
    | Dave    | Eggers         | Dave Eggers          |
    | Dave    | Eggers         | Dave Eggers          |
    | Michael | Chabon         | Michael Chabon       |
    | Patti   | Smith          | Patti Smith          |
    | Dave    | Eggers         | Dave Eggers          |
    | Neil    | Gaiman         | Neil Gaiman          |
    | Raymond | Carver         | Raymond Carver       |
    | Raymond | Carver         | Raymond Carver       |
    | Don     | DeLillo        | Don DeLillo          |
    | John    | Steinbeck      | John Steinbeck       |
    | David   | Foster Wallace | David Foster Wallace |
    | David   | Foster Wallace | David Foster Wallace |
    +---------+----------------+----------------------+
    
    
#### concat_ws()

    mysql> SELECT CONCAT(title, '-', author_fname, '-', author_lname) FROM books;
    +--------------------------------------------------------------------+
    | CONCAT(title, '-', author_fname, '-', author_lname)                |
    +--------------------------------------------------------------------+
    | The Namesake-Jhumpa-Lahiri                                         |
    | Norse Mythology-Neil-Gaiman                                        |
    | American Gods-Neil-Gaiman                                          |
    | Interpreter of Maladies-Jhumpa-Lahiri                              |
    | A Hologram for the King: A Novel-Dave-Eggers                       |
    | The Circle-Dave-Eggers                                             |
    | The Amazing Adventures of Kavalier & Clay-Michael-Chabon           |
    | Just Kids-Patti-Smith                                              |
    | A Heartbreaking Work of Staggering Genius-Dave-Eggers              |
    | Coraline-Neil-Gaiman                                               |
    | What We Talk About When We Talk About Love: Stories-Raymond-Carver |
    | Where I'm Calling From: Selected Stories-Raymond-Carver            |
    | White Noise-Don-DeLillo                                            |
    | Cannery Row-John-Steinbeck                                         |
    | Oblivion: Stories-David-Foster Wallace                             |
    | Consider the Lobster-David-Foster Wallace                          |
    +--------------------------------------------------------------------+
    
    mysql> SELECT CONCAT_WS('-', title, author_fname, author_lname) FROM books;
    +--------------------------------------------------------------------+
    | CONCAT_WS('-', title, author_fname, author_lname)                  |
    +--------------------------------------------------------------------+
    | The Namesake-Jhumpa-Lahiri                                         |
    | Norse Mythology-Neil-Gaiman                                        |
    | American Gods-Neil-Gaiman                                          |
    | Interpreter of Maladies-Jhumpa-Lahiri                              |
    | A Hologram for the King: A Novel-Dave-Eggers                       |
    | The Circle-Dave-Eggers                                             |
    | The Amazing Adventures of Kavalier & Clay-Michael-Chabon           |
    | Just Kids-Patti-Smith                                              |
    | A Heartbreaking Work of Staggering Genius-Dave-Eggers              |
    | Coraline-Neil-Gaiman                                               |
    | What We Talk About When We Talk About Love: Stories-Raymond-Carver |
    | Where I'm Calling From: Selected Stories-Raymond-Carver            |
    | White Noise-Don-DeLillo                                            |
    | Cannery Row-John-Steinbeck                                         |
    | Oblivion: Stories-David-Foster Wallace                             |
    | Consider the Lobster-David-Foster Wallace                          |
    +--------------------------------------------------------------------+

#### SUBSTRING() ou SUBSTR()

SUBSTRING() e SUBSTR() são exatamente a mesma função

começa a pegar na primeira posição "H" até a quarta posição "l";

    mysql> SELECT SUBSTRING('Hello World', 1, 4);
    +--------------------------------+
    | SUBSTRING('Hello World', 1, 4) |
    +--------------------------------+
    | Hell                           |
    +--------------------------------+

começa a pegar na setima posição "W" até o final;
    
    mysql> SELECT SUBSTRING('Hello World', 7);
    +-----------------------------+
    | SUBSTRING('Hello World', 7) |
    +-----------------------------+
    | World                       |
    +-----------------------------+      
    
outro exemplo

    mysql> SELECT SUBSTRING('Hello World', 3, 8);
    +--------------------------------+
    | SUBSTRING('Hello World', 3, 8) |
    +--------------------------------+
    | llo Worl                       |
    +--------------------------------+
    1 row in set (0.00 sec)      
    
Numeros negativos começão a contar do final para o inicio;

    mysql> SELECT SUBSTRING('Hello World', -3);
    +------------------------------+
    | SUBSTRING('Hello World', -3) |
    +------------------------------+
    | rld                          |
    +------------------------------+    

Outros exemplos

    mysql> SELECT title FROM books;
    +-----------------------------------------------------+
    | title                                               |
    +-----------------------------------------------------+
    | The Namesake                                        |
    | Norse Mythology                                     |
    | American Gods                                       |
    | Interpreter of Maladies                             |
    | A Hologram for the King: A Novel                    |
    | The Circle                                          |
    | The Amazing Adventures of Kavalier & Clay           |
    | Just Kids                                           |
    | A Heartbreaking Work of Staggering Genius           |
    | Coraline                                            |
    | What We Talk About When We Talk About Love: Stories |
    | Where I'm Calling From: Selected Stories            |
    | White Noise                                         |
    | Cannery Row                                         |
    | Oblivion: Stories                                   |
    | Consider the Lobster                                |
    +-----------------------------------------------------+  
    
    mysql> SELECT SUBSTRING(title, 1, 10) FROM books;
    +-------------------------+
    | SUBSTRING(title, 1, 10) |
    +-------------------------+
    | The Namesa              |
    | Norse Myth              |
    | American G              |
    | Interprete              |
    | A Hologram              |
    | The Circle              |
    | The Amazin              |
    | Just Kids               |
    | A Heartbre              |
    | Coraline                |
    | What We Ta              |
    | Where I'm               |
    | White Nois              |
    | Cannery Ro              |
    | Oblivion:               |
    | Consider t              |
    +-------------------------+  
    
    mysql> SELECT SUBSTRING(title, 1, 10) AS 'short title' FROM books;                                                                                 
    +-------------+
    | short title |
    +-------------+
    | The Namesa  |
    | Norse Myth  |
    | American G  |
    | Interprete  |
    | A Hologram  |
    | The Circle  |
    | The Amazin  |
    | Just Kids   |
    | A Heartbre  |
    | Coraline    |
    | What We Ta  |
    | Where I'm   |
    | White Nois  |
    | Cannery Ro  |
    | Oblivion:   |
    | Consider t  |
    +-------------+   
   
Combinando com outras funções

    SELECT CONCAT(SUBSTRING(title, 1, 10), '...') FROM books;
    +----------------------------------------+
    | CONCAT(SUBSTRING(title, 1, 10), '...') |
    +----------------------------------------+
    | The Namesa...                          |
    | Norse Myth...                          |
    | American G...                          |
    | Interprete...                          |
    | A Hologram...                          |
    | The Circle...                          |
    | The Amazin...                          |
    | Just Kids...                           |
    | A Heartbre...                          |
    | Coraline...                            |
    | What We Ta...                          |
    | Where I'm ...                          |
    | White Nois...                          |
    | Cannery Ro...                          |
    | Oblivion: ...                          |
    | Consider t...                          |
    +----------------------------------------+   
    
    SELECT CONCAT
        (
            SUBSTRING(title, 1, 10),
            '...'
        ) AS 'short title'
        FROM books;
        
    +---------------+
    | short title   |
    +---------------+
    | The Namesa... |
    | Norse Myth... |
    | American G... |
    | Interprete... |
    | A Hologram... |
    | The Circle... |
    | The Amazin... |
    | Just Kids...  |
    | A Heartbre... |
    | Coraline...   |
    | What We Ta... |
    | Where I'm ... |
    | White Nois... |
    | Cannery Ro... |
    | Oblivion: ... |
    | Consider t... |
    +---------------+    

####REPLACE()

    SELECT REPLACE('Hello World', 'Hell', '%$#@');
    +----------------------------------------+
    | REPLACE('Hello World', 'Hell', '%$#@') |
    +----------------------------------------+
    | %$#@o World                            |
    +----------------------------------------+
    
    SELECT REPLACE('Hello World', 'l', '7');
    +----------------------------------+
    | REPLACE('Hello World', 'l', '7') |
    +----------------------------------+
    | He77o Wor7d                      |
    +----------------------------------+
    
    SELECT REPLACE ('cheese bread coffee milk', ' ', ' and ');
    +----------------------------------------------------+
    | REPLACE ('cheese bread coffee milk', ' ', ' and ') |
    +----------------------------------------------------+
    | cheese and bread and coffee and milk               |
    +----------------------------------------------------+

É Case-sensitive

    SELECT REPLACE('HellO World', 'o', '*');                                                                                                    
    +----------------------------------+
    | REPLACE('HellO World', 'o', '*') |
    +----------------------------------+
    | HellO W*rld                      |
    +----------------------------------+
    
    SELECT title FROM books;
    +-----------------------------------------------------+
    | title                                               |
    +-----------------------------------------------------+
    | The Namesake                                        |
    | Norse Mythology                                     |
    | American Gods                                       |
    | Interpreter of Maladies                             |
    | A Hologram for the King: A Novel                    |
    | The Circle                                          |
    | The Amazing Adventures of Kavalier & Clay           |
    | Just Kids                                           |
    | A Heartbreaking Work of Staggering Genius           |
    | Coraline                                            |
    | What We Talk About When We Talk About Love: Stories |
    | Where I'm Calling From: Selected Stories            |
    | White Noise                                         |
    | Cannery Row                                         |
    | Oblivion: Stories                                   |
    | Consider the Lobster                                |
    +-----------------------------------------------------+
    
    SELECT SUBSTRING(REPLACE(title, 'e', '3'), 1, 10) FROM books;                                                                               
    +--------------------------------------------+
    | SUBSTRING(REPLACE(title, 'e', '3'), 1, 10) |
    +--------------------------------------------+
    | Th3 Nam3sa                                 |
    | Nors3 Myth                                 |
    | Am3rican G                                 |
    | Int3rpr3t3                                 |
    | A Hologram                                 |
    | Th3 Circl3                                 |
    | Th3 Amazin                                 |
    | Just Kids                                  |
    | A H3artbr3                                 |
    | Coralin3                                   |
    | What W3 Ta                                 |
    | Wh3r3 I'm                                  |
    | Whit3 Nois                                 |
    | Cann3ry Ro                                 |
    | Oblivion:                                  |
    | Consid3r t                                 |
    +--------------------------------------------+
    
#### REVERSE()

    SELECT REVERSE('Hello World');
    +------------------------+
    | REVERSE('Hello World') |
    +------------------------+
    | dlroW olleH            |
    +------------------------+
    
    SELECT author_fname FROM books;                                                                                                             
    +--------------+
    | author_fname |
    +--------------+
    | Jhumpa       |
    | Neil         |
    | Neil         |
    | Jhumpa       |
    +--------------+
    
    SELECT REVERSE(author_fname) FROM books;
    +-----------------------+
    | REVERSE(author_fname) |
    +-----------------------+
    | apmuhJ                |
    | lieN                  |
    | lieN                  |
    | apmuhJ                |
    +-----------------------+ 

Obtendo um palindromo
    
    SELECT CONCAT('woof', REVERSE('woof'));
    +---------------------------------+
    | CONCAT('woof', REVERSE('woof')) |
    +---------------------------------+
    | wooffoow                        |
    +---------------------------------   
    
    
####CHAR_LENGTH

conta os caracteres de uma string;

    SELECT CHAR_LENGTH('Hello World');
    +----------------------------+
    | CHAR_LENGTH('Hello World') |
    +----------------------------+
    |                         11 |
    +----------------------------+
    
    SELECT author_Lname FROM books;                                                                                                    
    +----------------+
    | author_Lname   |
    +----------------+
    | Lahiri         |
    | Gaiman         |
    | Gaiman         |
    | Lahiri         |
    | Eggers         |
    | Eggers         |
    | Chabon         |
    | Smith          |
    | Eggers         |
    | Gaiman         |
    | Carver         |
    | Carver         |
    | DeLillo        |
    | Steinbeck      |
    | Foster Wallace |
    | Foster Wallace |
    +----------------+
    
    SELECT author_lname, CHAR_LENGTH(author_lname) AS 'length' FROM books;
    +----------------+--------+
    | author_lname   | length |
    +----------------+--------+
    | Lahiri         |      6 |
    | Gaiman         |      6 |
    | Gaiman         |      6 |
    | Lahiri         |      6 |
    | Eggers         |      6 |
    | Eggers         |      6 |
    | Chabon         |      6 |
    | Smith          |      5 |
    | Eggers         |      6 |
    | Gaiman         |      6 |
    | Carver         |      6 |
    | Carver         |      6 |
    | DeLillo        |      7 |
    | Steinbeck      |      9 |
    | Foster Wallace |     14 |
    | Foster Wallace |     14 |
    +----------------+--------+
    
    SELECT CONCAT(author_lname, ' is ', CHAR_LENGTH(author_lname), ' characters long') FROM books;                                              
    +-----------------------------------------------------------------------------+
    | CONCAT(author_lname, ' is ', CHAR_LENGTH(author_lname), ' characters long') |
    +-----------------------------------------------------------------------------+
    | Lahiri is 6 characters long                                                 |
    | Gaiman is 6 characters long                                                 |
    | Gaiman is 6 characters long                                                 |
    | Lahiri is 6 characters long                                                 |
    | Eggers is 6 characters long                                                 |
    | Eggers is 6 characters long                                                 |
    | Chabon is 6 characters long                                                 |
    | Smith is 5 characters long                                                  |
    | Eggers is 6 characters long                                                 |
    | Gaiman is 6 characters long                                                 |
    | Carver is 6 characters long                                                 |
    | Carver is 6 characters long                                                 |
    | DeLillo is 7 characters long                                                |
    | Steinbeck is 9 characters long                                              |
    | Foster Wallace is 14 characters long                                        |
    | Foster Wallace is 14 characters long                                        |
    +-----------------------------------------------------------------------------+   

####UPPER() E LOWER()

    select upper('Hello World');
    +----------------------+
    | upper('Hello World') |
    +----------------------+
    | HELLO WORLD          |
    +----------------------+
    
    lower('Hello World');                                                                                                                
    +----------------------+
    | lower('Hello World') |
    +----------------------+
    | hello world          |
    +----------------------+

#### DISTINC

Pega apenas uma linha de cada valor repetido;

    SELECT author_fname from books;
    +--------------+
    | author_fname |
    +--------------+
    | Jhumpa       |
    | Neil         |
    | Neil         |
    | Jhumpa       |
    | Dave         |
    | Dave         |
    | Michael      |
    | Patti        |
    | Dave         |
    | Neil         |
    | Raymond      |
    | Raymond      |
    | Don          |
    | John         |
    | David        |
    | David        |
    | Dan          |
    | Freida       |
    | George       |
    +--------------+
    
    SELECT DISTINCT author_fname FROM books;
    +--------------+
    | author_fname |
    +--------------+
    | Jhumpa       |
    | Neil         |
    | Dave         |
    | Michael      |
    | Patti        |
    | Raymond      |
    | Don          |
    | John         |
    | David        |
    | Dan          |
    | Freida       |
    | George       |
    +--------------+
    
    SELECT author_fname, author_lname FROM books;
    +--------------+----------------+
    | author_fname | author_lname   |
    +--------------+----------------+
    | Jhumpa       | Lahiri         |
    | Neil         | Gaiman         |
    | Neil         | Gaiman         |
    | Jhumpa       | Lahiri         |
    | Dave         | Eggers         |
    | Dave         | Eggers         |
    | Michael      | Chabon         |
    | Patti        | Smith          |
    | Dave         | Eggers         |
    | Neil         | Gaiman         |
    | Raymond      | Carver         |
    | Raymond      | Carver         |
    | Don          | DeLillo        |
    | John         | Steinbeck      |
    | David        | Foster Wallace |
    | David        | Foster Wallace |
    | Dan          | Harris         |
    | Freida       | Harris         |
    | George       | Saunders       |
    +--------------+----------------+
    
    SELECT DISTINCT author_lname FROM books;                                                                                                    
    +----------------+
    | author_lname   |
    +----------------+
    | Lahiri         |
    | Gaiman         |
    | Eggers         |
    | Chabon         |
    | Smith          |
    | Carver         |
    | DeLillo        |
    | Steinbeck      |
    | Foster Wallace |
    | Harris         |
    | Saunders       |
    +----------------+
    
    SELECT DISTINCT CONCAT(author_fname, ' ', author_lname) FROM books; 
    +-----------------------------------------+
    | CONCAT(author_fname, ' ', author_lname) |
    +-----------------------------------------+
    | Jhumpa Lahiri                           |
    | Neil Gaiman                             |
    | Dave Eggers                             |
    | Michael Chabon                          |
    | Patti Smith                             |
    | Raymond Carver                          |
    | Don DeLillo                             |
    | John Steinbeck                          |
    | David Foster Wallace                    |
    | Dan Harris                              |
    | Freida Harris                           |
    | George Saunders                         |
    +-----------------------------------------+
    
    SELECT DISTINCT author_fname, author_lname FROM books;
    +--------------+----------------+
    | author_fname | author_lname   |
    +--------------+----------------+
    | Jhumpa       | Lahiri         |
    | Neil         | Gaiman         |
    | Dave         | Eggers         |
    | Michael      | Chabon         |
    | Patti        | Smith          |
    | Raymond      | Carver         |
    | Don          | DeLillo        |
    | John         | Steinbeck      |
    | David        | Foster Wallace |
    | Dan          | Harris         |
    | Freida       | Harris         |
    | George       | Saunders       |
    +--------------+----------------+
    
#### ORDER BY

Ordem alfabética ascendente serve para colunas numéricas também.

    SELECT author_lname FROM books ORDER BY author_lname;
    SELECT author_lname FROM books ORDER BY author_lname ASC;
    
Ordem alfabética descendente serve para colunas numéricas também.

    SELECT author_lname FROM books ORDER BY author_lname DESC;

Ira ordenar pela 2ª coluna estabelecida na query, no caso author_fname.
    
    SELECT title, author_fname, author_lname 
    FROM books ORDER BY 2;
    
Ordenação sequencial, ira ordenar por author_lname, depois por author_fname, sem ofender a
primeira regra de ordenação. Oberse a linha David e Freida com relação as outras querys;
    
    SELECT author_fname, author_lname FROM books 
    ORDER BY author_lname, author_fname;  
    +--------------+----------------+
    | author_fname | author_lname   |
    +--------------+----------------+
    | Raymond      | Carver         |
    | Raymond      | Carver         |
    | Michael      | Chabon         |
    | Don          | DeLillo        |
    | Dave         | Eggers         |
    | Dave         | Eggers         |
    | Dave         | Eggers         |
    | David        | Foster Wallace |
    | David        | Foster Wallace |
    | Neil         | Gaiman         |
    | Neil         | Gaiman         |
    | Neil         | Gaiman         |
    | Dan          | Harris         |
    | Freida       | Harris         |
    | Jhumpa       | Lahiri         |
    | Jhumpa       | Lahiri         |
    | George       | Saunders       |
    | Patti        | Smith          |
    | John         | Steinbeck      |
    +--------------+----------------+
    
    SELECT author_fname, author_lname FROM books ORDER BY author_lname;
    +--------------+----------------+
    | author_fname | author_lname   |
    +--------------+----------------+
    | Raymond      | Carver         |
    | Raymond      | Carver         |
    | Michael      | Chabon         |
    | Don          | DeLillo        |
    | Dave         | Eggers         |
    | Dave         | Eggers         |
    | Dave         | Eggers         |
    | David        | Foster Wallace |
    | David        | Foster Wallace |
    | Neil         | Gaiman         |
    | Neil         | Gaiman         |
    | Neil         | Gaiman         |
    | Freida       | Harris         |
    | Dan          | Harris         |
    | Jhumpa       | Lahiri         |
    | Jhumpa       | Lahiri         |
    | George       | Saunders       |
    | Patti        | Smith          |
    | John         | Steinbeck      |
    +--------------+----------------+
    
    SELECT author_fname, author_lname FROM books ORDER BY author_fname;                                                                   
    +--------------+----------------+
    | author_fname | author_lname   |
    +--------------+----------------+
    | Dan          | Harris         |
    | Dave         | Eggers         |
    | Dave         | Eggers         |
    | Dave         | Eggers         |
    | David        | Foster Wallace |
    | David        | Foster Wallace |
    | Don          | DeLillo        |
    | Freida       | Harris         |
    | George       | Saunders       |
    | Jhumpa       | Lahiri         |
    | Jhumpa       | Lahiri         |
    | John         | Steinbeck      |
    | Michael      | Chabon         |
    | Neil         | Gaiman         |
    | Neil         | Gaiman         |
    | Neil         | Gaiman         |
    | Patti        | Smith          |
    | Raymond      | Carver         |
    | Raymond      | Carver         |
    +--------------+----------------+
    
#### LIMIT

Limita o numero de resultados

    SELECT title FROM books LIMIT 3;
    +-----------------+
    | title           |
    +-----------------+
    | The Namesake    |
    | Norse Mythology |
    | American Gods   |
    +-----------------+
    
    SELECT title, released_year FROM books ORDER BY released_year LIMIT 5;
    +-----------------------------------------------------+---------------+
    | title                                               | released_year |
    +-----------------------------------------------------+---------------+
    | Cannery Row                                         |          1945 |
    | What We Talk About When We Talk About Love: Stories |          1981 |
    | White Noise                                         |          1985 |
    | Where I'm Calling From: Selected Stories            |          1989 |
    | Interpreter of Maladies                             |          1996 |
    +-----------------------------------------------------+---------------+
    
    SELECT title, released_year FROM books ORDER BY released_year DESC LIMIT 5;                                                                
    +----------------------------------+---------------+
    | title                            | released_year |
    +----------------------------------+---------------+
    | Lincoln In The Bardo             |          2017 |
    | Norse Mythology                  |          2016 |
    | 10% Happier                      |          2014 |
    | The Circle                       |          2013 |
    | A Hologram for the King: A Novel |          2012 |
    +----------------------------------+---------------+

Neste 0 representa a primeira linha da tabela, 5 a quantidade de linhas que a query pegara
    
    SELECT title, released_year FROM books ORDER BY released_year DESC LIMIT 0,5;                                                               
    +----------------------------------+---------------+
    | title                            | released_year |
    +----------------------------------+---------------+
    | Lincoln In The Bardo             |          2017 |
    | Norse Mythology                  |          2016 |
    | 10% Happier                      |          2014 |
    | The Circle                       |          2013 |
    | A Hologram for the King: A Novel |          2012 |
    +----------------------------------+---------------+

Ignoramos a Linha 0
    
    SELECT title, released_year FROM books ORDER BY released_year DESC LIMIT 1,5;                                                               
    +----------------------------------+---------------+
    | title                            | released_year |
    +----------------------------------+---------------+
    | Norse Mythology                  |          2016 |
    | 10% Happier                      |          2014 |
    | The Circle                       |          2013 |
    | A Hologram for the King: A Novel |          2012 |
    | Just Kids                        |          2010 |
    +----------------------------------+---------------+

Ignoramos tudo o que vem antes da linha 4;
    
    SELECT title, released_year FROM books ORDER BY released_year DESC LIMIT 4,5;
    +----------------------------------+---------------+
    | title                            | released_year |
    +----------------------------------+---------------+
    | A Hologram for the King: A Novel |          2012 |
    | Just Kids                        |          2010 |
    | Consider the Lobster             |          2005 |
    | Oblivion: Stories                |          2004 |
    | Coraline                         |          2003 |
    +----------------------------------+---------------+
    
Nota: o recurso limit pode ser usado para paginação;

#### LIKE

Note: O Like pode ser usado para buscas;

    SELECT title, author_fname FROM books WHERE author_fname LIKE '%da%';                                                                       
    +-------------------------------------------+--------------+
    | title                                     | author_fname |
    +-------------------------------------------+--------------+
    | A Hologram for the King: A Novel          | Dave         |
    | The Circle                                | Dave         |
    | A Heartbreaking Work of Staggering Genius | Dave         |
    | Oblivion: Stories                         | David        |
    | Consider the Lobster                      | David        |
    | 10% Happier                               | Dan          |
    | fake_book                                 | Freida       |
    +-------------------------------------------+--------------+
    
A operação select no exemplo acima poderia se substituida pela delete ou update.
'%da%' pode haver qualquer numero e qualquer caractere antes e depois de da.

    SELECT title, author_fname FROM books WHERE author_fname LIKE '%da'; 
    +-----------+--------------+
    | title     | author_fname |
    +-----------+--------------+
    | fake_book | Freida       |
    +-----------+--------------+
    
    SELECT title, author_fname FROM books WHERE author_fname LIKE 'da%';                                                                        
    +-------------------------------------------+--------------+
    | title                                     | author_fname |
    +-------------------------------------------+--------------+
    | A Hologram for the King: A Novel          | Dave         |
    | The Circle                                | Dave         |
    | A Heartbreaking Work of Staggering Genius | Dave         |
    | Oblivion: Stories                         | David        |
    | Consider the Lobster                      | David        |
    | 10% Happier                               | Dan          |
    +-------------------------------------------+--------------+
    
    SELECT title, author_fname FROM books WHERE author_fname LIKE 'dave';                                                                       
    +-------------------------------------------+--------------+
    | title                                     | author_fname |
    +-------------------------------------------+--------------+
    | A Hologram for the King: A Novel          | Dave         |
    | The Circle                                | Dave         |
    | A Heartbreaking Work of Staggering Genius | Dave         |
    +-------------------------------------------+--------------+
    
    SELECT title, stock_quantity FROM books WHERE stock_quantity LIKE '%';
    +-----------------------------------------------------+----------------+
    | title                                               | stock_quantity |
    +-----------------------------------------------------+----------------+
    | The Namesake                                        |             32 |
    | Norse Mythology                                     |             43 |
    | American Gods                                       |             12 |
    | Interpreter of Maladies                             |             97 |
    | A Hologram for the King: A Novel                    |            154 |
    | The Circle                                          |             26 |
    | The Amazing Adventures of Kavalier & Clay           |             68 |
    | Just Kids                                           |             55 |
    | A Heartbreaking Work of Staggering Genius           |            104 |
    | Coraline                                            |            100 |
    | What We Talk About When We Talk About Love: Stories |             23 |
    | Where I'm Calling From: Selected Stories            |             12 |
    | White Noise                                         |             49 |
    | Cannery Row                                         |             95 |
    | Oblivion: Stories                                   |            172 |
    | Consider the Lobster                                |             92 |
    | 10% Happier                                         |             29 |
    | fake_book                                           |            287 |
    | Lincoln In The Bardo                                |           1000 |
    +-----------------------------------------------------+----------------+
    
    SELECT title, stock_quantity FROM books WHERE stock_quantity LIKE '__';
    +-----------------------------------------------------+----------------+
    | title                                               | stock_quantity |
    +-----------------------------------------------------+----------------+
    | The Namesake                                        |             32 |
    | Norse Mythology                                     |             43 |
    | American Gods                                       |             12 |
    | Interpreter of Maladies                             |             97 |
    | The Circle                                          |             26 |
    | The Amazing Adventures of Kavalier & Clay           |             68 |
    | Just Kids                                           |             55 |
    | What We Talk About When We Talk About Love: Stories |             23 |
    | Where I'm Calling From: Selected Stories            |             12 |
    | White Noise                                         |             49 |
    | Cannery Row                                         |             95 |
    | Consider the Lobster                                |             92 |
    | 10% Happier                                         |             29 |
    +-----------------------------------------------------+----------------+
    
    SELECT title, stock_quantity FROM books WHERE stock_quantity LIKE '____';
    +----------------------+----------------+
    | title                | stock_quantity |
    +----------------------+----------------+
    | Lincoln In The Bardo |           1000 |
    +----------------------+----------------+
    
Para o padrão (235)234-0987 poderiamos usar LIKE '(___)___-____' 
Caso você fazer uma busca em uma string que contém um caractere coringa ele podem ser
escapados com '\'. Ex;.: '%\%%', '\_'
    
     SELECT title FROM books WHERE title LIKE '%\%%';
    +-------------+
    | title       |
    +-------------+
    | 10% Happier |
    +-------------+
   
####Comentários
   
    -- SELECT
    --    CONCAT
    --    (
    --        SUBSTRING(title, 1, 10),
    --        '...'
    --    ) AS 'short title'
    -- FROM books; 
    
####Caracter Curinga (wildcards)

'%' - Letra ou numero

'_' - Numero    
                
#### Count()

Conta o numero de registros;

    SELECT COUNT(*) FROM books;
    +----------+
    | COUNT(*) |
    +----------+
    |       19 |
    +----------+
    
    SELECT COUNT(DISTINCT author_fname) FROM books;                                                                            
    +------------------------------+
    | COUNT(DISTINCT author_fname) |
    +------------------------------+
    |                           12 |
    +------------------------------+
    
    SELECT COUNT(*) FROM books WHERE title LIKE '%the%';
    +----------+
    | COUNT(*) |
    +----------+
    |        6 |
    +----------+
    
#### GROUP_BY()

    SELECT title, author_fname, author_lname FROM books;                                                                       
    +-----------------------------------------------------+--------------+----------------+
    | title                                               | author_fname | author_lname   |
    +-----------------------------------------------------+--------------+----------------+
    | The Namesake                                        | Jhumpa       | Lahiri         |
    | Norse Mythology                                     | Neil         | Gaiman         |
    | American Gods                                       | Neil         | Gaiman         |
    | Interpreter of Maladies                             | Jhumpa       | Lahiri         |
    | A Hologram for the King: A Novel                    | Dave         | Eggers         |
    | The Circle                                          | Dave         | Eggers         |
    | The Amazing Adventures of Kavalier & Clay           | Michael      | Chabon         |
    | Just Kids                                           | Patti        | Smith          |
    | A Heartbreaking Work of Staggering Genius           | Dave         | Eggers         |
    | Coraline                                            | Neil         | Gaiman         |
    | What We Talk About When We Talk About Love: Stories | Raymond      | Carver         |
    | Where I'm Calling From: Selected Stories            | Raymond      | Carver         |
    | White Noise                                         | Don          | DeLillo        |
    | Cannery Row                                         | John         | Steinbeck      |
    | Oblivion: Stories                                   | David        | Foster Wallace |
    | Consider the Lobster                                | David        | Foster Wallace |
    | 10% Happier                                         | Dan          | Harris         |
    | fake_book                                           | Freida       | Harris         |
    | Lincoln In The Bardo                                | George       | Saunders       |
    +-----------------------------------------------------+--------------+----------------+
    19 rows in set (0.00 sec)

Funcionou de maneira semelhante ao distinct, mas ao contrario do mesmo ele agrupa as linhas em vez de discrimina-las.
    
    SELECT title, author_fname, author_lname FROM books GROUP BY author_lname;                                             
    +-----------------------------------------------------+--------------+----------------+
    | title                                               | author_fname | author_lname   |
    +-----------------------------------------------------+--------------+----------------+
    | What We Talk About When We Talk About Love: Stories | Raymond      | Carver         |
    | The Amazing Adventures of Kavalier & Clay           | Michael      | Chabon         |
    | White Noise                                         | Don          | DeLillo        |
    | A Hologram for the King: A Novel                    | Dave         | Eggers         |
    | Oblivion: Stories                                   | David        | Foster Wallace |
    | Norse Mythology                                     | Neil         | Gaiman         |
    | 10% Happier                                         | Dan          | Harris         |
    | The Namesake                                        | Jhumpa       | Lahiri         |
    | Lincoln In The Bardo                                | George       | Saunders       |
    | Just Kids                                           | Patti        | Smith          |
    | Cannery Row                                         | John         | Steinbeck      |
    +-----------------------------------------------------+--------------+----------------+
    11 rows in set (0.00 sec)

Note funciona de maneira diferente do  distinct, agrupou os sobrenomes e contou quanto cada um deles possui.
O count se refere aos grupos de linhas do group by na coluna author_lname;

    SELECT title, author_fname, author_lname, COUNT(*) FROM books GROUP BY author_lname;                                       
    +-----------------------------------------------------+--------------+----------------+----------+
    | title                                               | author_fname | author_lname   | COUNT(*) |
    +-----------------------------------------------------+--------------+----------------+----------+
    | What We Talk About When We Talk About Love: Stories | Raymond      | Carver         |        2 |
    | The Amazing Adventures of Kavalier & Clay           | Michael      | Chabon         |        1 |
    | White Noise                                         | Don          | DeLillo        |        1 |
    | A Hologram for the King: A Novel                    | Dave         | Eggers         |        3 |
    | Oblivion: Stories                                   | David        | Foster Wallace |        2 |
    | Norse Mythology                                     | Neil         | Gaiman         |        3 |
    | 10% Happier                                         | Dan          | Harris         |        2 |
    | The Namesake                                        | Jhumpa       | Lahiri         |        2 |
    | Lincoln In The Bardo                                | George       | Saunders       |        1 |
    | Just Kids                                           | Patti        | Smith          |        1 |
    | Cannery Row                                         | John         | Steinbeck      |        1 |
    +-----------------------------------------------------+--------------+----------------+----------+
    11 rows in set (0.00 sec)  
        
Note que há um problema, como estamos agrupando apenas por sobrenome  Dan Harris e Freid Harris são agrupados juntos, 
podemos resolver isto da seguinte maneira.

     SELECT title, author_fname, author_lname, COUNT(*) FROM books GROUP BY author_lname, author_fname;
    +-----------------------------------------------------+--------------+----------------+----------+
    | title                                               | author_fname | author_lname   | COUNT(*) |
    +-----------------------------------------------------+--------------+----------------+----------+
    | What We Talk About When We Talk About Love: Stories | Raymond      | Carver         |        2 |
    | The Amazing Adventures of Kavalier & Clay           | Michael      | Chabon         |        1 |
    | White Noise                                         | Don          | DeLillo        |        1 |
    | A Hologram for the King: A Novel                    | Dave         | Eggers         |        3 |
    | Oblivion: Stories                                   | David        | Foster Wallace |        2 |
    | Norse Mythology                                     | Neil         | Gaiman         |        3 |
    | 10% Happier                                         | Dan          | Harris         |        1 |
    | fake_book                                           | Freida       | Harris         |        1 |
    | The Namesake                                        | Jhumpa       | Lahiri         |        2 |
    | Lincoln In The Bardo                                | George       | Saunders       |        1 |
    | Just Kids                                           | Patti        | Smith          |        1 |
    | Cannery Row                                         | John         | Steinbeck      |        1 |
    +-----------------------------------------------------+--------------+----------------+----------+
    12 rows in set (0.00 sec)
    
Outro Exemplo

    select released_year from books;
    +---------------+
    | released_year |
    +---------------+
    |          2003 |
    |          2016 |
    |          2001 |
    |          1996 |
    |          2012 |
    |          2013 |
    |          2000 |
    |          2010 |
    |          2001 |
    |          2003 |
    |          1981 |
    |          1989 |
    |          1985 |
    |          1945 |
    |          2004 |
    |          2005 |
    |          2014 |
    |          2001 |
    |          2017 |
    +---------------+
    19 rows
    
    select released_year, count(*) from books GROUP BY released_year;
    +---------------+----------+
    | released_year | count(*) |
    +---------------+----------+
    |          1945 |        1 |
    |          1981 |        1 |
    |          1985 |        1 |
    |          1989 |        1 |
    |          1996 |        1 |
    |          2000 |        1 |
    |          2001 |        3 |
    |          2003 |        2 |
    |          2004 |        1 |
    |          2005 |        1 |
    |          2010 |        1 |
    |          2012 |        1 |
    |          2013 |        1 |
    |          2014 |        1 |
    |          2016 |        1 |
    |          2017 |        1 |
    +---------------+----------+
    16 rows
    
    SELECT CONCAT('In ', released_year,' ',COUNT(*), ' book(s) released' ) FROM books GROUP BY released_year;                  
    +-----------------------------------------------------------------+
    | CONCAT('In ', released_year,' ',COUNT(*), ' book(s) released' ) |
    +-----------------------------------------------------------------+
    | In 1945 1 book(s) released                                      |
    | In 1981 1 book(s) released                                      |
    | In 1985 1 book(s) released                                      |
    | In 1989 1 book(s) released                                      |
    | In 1996 1 book(s) released                                      |
    | In 2000 1 book(s) released                                      |
    | In 2001 3 book(s) released                                      |
    | In 2003 2 book(s) released                                      |
    | In 2004 1 book(s) released                                      |
    | In 2005 1 book(s) released                                      |
    | In 2010 1 book(s) released                                      |
    | In 2012 1 book(s) released                                      |
    | In 2013 1 book(s) released                                      |
    | In 2014 1 book(s) released                                      |
    | In 2016 1 book(s) released                                      |
    | In 2017 1 book(s) released                                      |
    +-----------------------------------------------------------------+
    16 rows
    
#### MIN() E MAX()     

     SELECT MIN(released_year) FROM books;                                                                                     
    +--------------------+
    | MIN(released_year) |
    +--------------------+
    |               1945 |
    +--------------------+
    1 row
    
    SELECT MAX(released_year) FROM books;                                                                                      
    +--------------------+
    | MAX(released_year) |
    +--------------------+
    |               2017 |
    +--------------------+
    
    SELECT MIN(pages) FROM books;                                                                                              
    +------------+
    | MIN(pages) |
    +------------+
    |        176 |
    +------------+
    1 row
    
    SELECT MAX(pages) FROM books;                                                                                              
    +------------+
    | MAX(pages) |
    +------------+
    |        634 |
    +------------+
    
Note a seguinte situação abaixo

    SELECT title, pages FROM books;                                                                                            
    +-----------------------------------------------------+-------+
    | title                                               | pages |
    +-----------------------------------------------------+-------+
    | The Namesake                                        |   291 |
    | Norse Mythology                                     |   304 |
    | American Gods                                       |   465 |
    | Interpreter of Maladies                             |   198 |
    | A Hologram for the King: A Novel                    |   352 |
    | The Circle                                          |   504 |
    | The Amazing Adventures of Kavalier & Clay           |   634 |
    | Just Kids                                           |   304 |
    | A Heartbreaking Work of Staggering Genius           |   437 |
    | Coraline                                            |   208 |
    | What We Talk About When We Talk About Love: Stories |   176 |
    | Where I'm Calling From: Selected Stories            |   526 |
    | White Noise                                         |   320 |
    | Cannery Row                                         |   181 |
    | Oblivion: Stories                                   |   329 |
    | Consider the Lobster                                |   343 |
    | 10% Happier                                         |   256 |
    | fake_book                                           |   428 |
    | Lincoln In The Bardo                                |   367 |
    +-----------------------------------------------------+-------+
    19 rows
    
    SELECT MAX(pages), title FROM books;                                                                                       
    +------------+--------------+
    | MAX(pages) | title        |
    +------------+--------------+
    |        634 | The Namesake |
    +------------+--------------+
    1 row
    
Ele esta trazendo o titulo da primeira linha, mas o numero de páginas de outro livro. Cuidado pois pode gerar erros.
A função MAX e MIN são idenpendentes de outras linhas  e colunas. Uma solução seria:

    SELECT title, pages FROM books WHERE pages = (SELECT MAX(pages) FROM books);                                               
    +-------------------------------------------+-------+
    | title                                     | pages |
    +-------------------------------------------+-------+
    | The Amazing Adventures of Kavalier & Clay |   634 |
    +-------------------------------------------+-------+
    1 row

O custo desta solução é relativamente alta em termos de custo. Note que a sub query retornaria 634.
Deixando esta query equivalente ao mesmo abaixo.  

    SELECT title, pages FROM books WHERE pages = 634;
    
Uma solução mais rápida em termos de máquina para obter esta linha é usar
o order by com limit 1; 

Min e Max vão bem com group by;

1º Ex.: Encontrar o ano em que cada autor publicou seu primeiro livro;

    SELECT author_fname, author_lname, MIN(released_year) FROM books GROUP BY author_lname, author_fname;
    +--------------+----------------+--------------------+
    | author_fname | author_lname   | MIN(released_year) |
    +--------------+----------------+--------------------+
    | Raymond      | Carver         |               1981 |
    | Michael      | Chabon         |               2000 |
    | Don          | DeLillo        |               1985 |
    | Dave         | Eggers         |               2001 |
    | David        | Foster Wallace |               2004 |
    | Neil         | Gaiman         |               2001 |
    | Dan          | Harris         |               2014 |
    | Freida       | Harris         |               2001 |
    | Jhumpa       | Lahiri         |               1996 |
    | George       | Saunders       |               2017 |
    | Patti        | Smith          |               2010 |
    | John         | Steinbeck      |               1945 |
    +--------------+----------------+--------------------+
    12 rows
    
2º Ex.: Encontar quantas páginas tem o maior livro publicado de cada autor;

    SELECT author_fname, author_lname, title, MAX(pages) FROM books GROUP BY author_lname, author_fname;                      
    +--------------+----------------+-----------------------------------------------------+------------+
    | author_fname | author_lname   | title                                               | MAX(pages) |
    +--------------+----------------+-----------------------------------------------------+------------+
    | Raymond      | Carver         | What We Talk About When We Talk About Love: Stories |        526 |
    | Michael      | Chabon         | The Amazing Adventures of Kavalier & Clay           |        634 |
    | Don          | DeLillo        | White Noise                                         |        320 |
    | Dave         | Eggers         | A Hologram for the King: A Novel                    |        504 |
    | David        | Foster Wallace | Oblivion: Stories                                   |        343 |
    | Neil         | Gaiman         | Norse Mythology                                     |        465 |
    | Dan          | Harris         | 10% Happier                                         |        256 |
    | Freida       | Harris         | fake_book                                           |        428 |
    | Jhumpa       | Lahiri         | The Namesake                                        |        291 |
    | George       | Saunders       | Lincoln In The Bardo                                |        367 |
    | Patti        | Smith          | Just Kids                                           |        304 |
    | John         | Steinbeck      | Cannery Row                                         |        181 |
    +--------------+----------------+-----------------------------------------------------+------------+
    12 rows
    
    SELECT 
        CONCAT(author_fname, ' ', author_lname) AS author,
        MAX(pages) AS 'longest book'
    FROM books
    GROUP BY author_lname, author_fname;
    
    +----------------------+--------------+
    | author               | longest book |
    +----------------------+--------------+
    | Raymond Carver       |          526 |
    | Michael Chabon       |          634 |
    | Don DeLillo          |          320 |
    | Dave Eggers          |          504 |
    | David Foster Wallace |          343 |
    | Neil Gaiman          |          465 |
    | Dan Harris           |          256 |
    | Freida Harris        |          428 |
    | Jhumpa Lahiri        |          291 |
    | George Saunders      |          367 |
    | Patti Smith          |          304 |
    | John Steinbeck       |          181 |
    +----------------------+--------------+
    12 rows
    
####SUM

Soma dados

    SELECT Sum(pages) FROM books;
    +------------+
    | Sum(pages) |
    +------------+
    |       6623 |
    +------------+
    
    SELECT Sum(released_year) FROM books;
    +--------------------+
    | Sum(released_year) |
    +--------------------+
    |              37996 |
    +--------------------+
    
1º Ex.: Somar todas as paginas que cada author escreveu

    SELECT author_fname, author_lname, Sum(pages)
    FROM books 
    GROUP BY author_lname, author_fname;
    
    +--------------+----------------+------------+
    | author_fname | author_lname   | Sum(pages) |
    +--------------+----------------+------------+
    | Raymond      | Carver         |        702 |
    | Michael      | Chabon         |        634 |
    | Don          | DeLillo        |        320 |
    | Dave         | Eggers         |       1293 |
    | David        | Foster Wallace |        672 |
    | Neil         | Gaiman         |        977 |
    | Dan          | Harris         |        256 |
    | Freida       | Harris         |        428 |
    | Jhumpa       | Lahiri         |        489 |
    | George       | Saunders       |        367 |
    | Patti        | Smith          |        304 |
    | John         | Steinbeck      |        181 |
    +--------------+----------------+------------+
    12 rows
    
#### Função AVG average

Função que obtem a média     

Ex.: Obtendo a média de dos anos de lançamentos dos livros

    SELECT AVG(released_year) FROM books;
    +--------------------+
    | AVG(released_year) |
    +--------------------+
    |          1999.7895 |
    +--------------------+
    1 row
    
Ex.: Obtendo a média das paginas dos livros publicados

    SELECT AVG(pages) FROM books;                                                                                              
    +------------+
    | AVG(pages) |
    +------------+
    |   348.5789 |
    +------------+    
    
Ex.: Calcular o stoque médio de livros lançados no mesmo ano

    SELECT AVG(stock_quantity) FROM books GROUP BY released_year;
    +---------------------+
    | AVG(stock_quantity) |
    +---------------------+
    |             95.0000 |
    |             23.0000 |
    |             49.0000 |
    |             12.0000 |
    |             97.0000 |
    |             68.0000 |
    |            134.3333 |
    |             66.0000 |
    |            172.0000 |
    |             92.0000 |
    |             55.0000 |
    |            154.0000 |
    |             26.0000 |
    |             29.0000 |
    |             43.0000 |
    |           1000.0000 |
    +---------------------+
    16 rows
    
    SELECT released_year, AVG(stock_quantity) FROM books GROUP BY released_year;                                               
    +---------------+---------------------+
    | released_year | AVG(stock_quantity) |
    +---------------+---------------------+
    |          1945 |             95.0000 |
    |          1981 |             23.0000 |
    |          1985 |             49.0000 |
    |          1989 |             12.0000 |
    |          1996 |             97.0000 |
    |          2000 |             68.0000 |
    |          2001 |            134.3333 |
    |          2003 |             66.0000 |
    |          2004 |            172.0000 |
    |          2005 |             92.0000 |
    |          2010 |             55.0000 |
    |          2012 |            154.0000 |
    |          2013 |             26.0000 |
    |          2014 |             29.0000 |
    |          2016 |             43.0000 |
    |          2017 |           1000.0000 |
    +---------------+---------------------+
    16 rows    

Media de paginas publicadas por author;

    SELECT author_fname, author_lname, AVG(pages) FROM books GROUP BY author_lname, author_fname;                              
    +--------------+----------------+------------+
    | author_fname | author_lname   | AVG(pages) |
    +--------------+----------------+------------+
    | Raymond      | Carver         |   351.0000 |
    | Michael      | Chabon         |   634.0000 |
    | Don          | DeLillo        |   320.0000 |
    | Dave         | Eggers         |   431.0000 |
    | David        | Foster Wallace |   336.0000 |
    | Neil         | Gaiman         |   325.6667 |
    | Dan          | Harris         |   256.0000 |
    | Freida       | Harris         |   428.0000 |
    | Jhumpa       | Lahiri         |   244.5000 |
    | George       | Saunders       |   367.0000 |
    | Patti        | Smith          |   304.0000 |
    | John         | Steinbeck      |   181.0000 |
    +--------------+----------------+------------+
    12 rows
    
## Tipos de dados

**varchar** (0-255): tipo texto tamanho variavél, a string ficara fiel desde que não estoure o tamanho do campo, neste caso ficara truncada.
    
**char**: tipo texto tamanho fixo, no caso de string menor adicionara espaços na memoria como se fosse maior (estes espaços não são recuperaveis)
 e for maior a string ficara truncada. É mais rapido que varchar se tratando de 
textos de mesmo tamanho.

**decimal**(1-65,0-30): o primeiro parametro representa o numero total de digitos da query (incluindo decimais), o segundo o numero de casas apos o zero.

**float** e **double**: Ao contrario do decimal não possui numero de casas decimais fixas, armazenas numeros grandes usa menos espaço mas é menos preciso

float: 4 Bytes, Precisão de ~7 digitos 
Double: 8 Bytes, Precisão de ~15 digitos 

Por conta do problema de ponto flutuante procure usar sempre o tipo decimal quando os resultados tem de ser sempre precisos,
principalmente em questões financeiras.

Exemplo de problema de arredondamento de ponto flutuante:

    CREATE TABLE thingies (price FLOAT);
    INSERT INTO thingies (price) VALUES (8888), (8888.888), (8888888888888);
    select * from thingies;
    +---------------+
    | price         |
    +---------------+
    |          8888 |
    |       8888.89 |
    | 8888890000000 |
    +---------------+
    
**Date**: apenas datas (YYYY-mm-dd);   

**Time**: apenas o tempo sem data (HH:MM:SS);

**DateTime**: Data e tempo combinados (YYYY-mm-dd HH:MM:SS);

#### Funções de tempo

    +-----------+-------------+------+-----+---------+-------+
    | Field     | Type        | Null | Key | Default | Extra |
    +-----------+-------------+------+-----+---------+-------+
    | name      | varchar(20) | YES  |     | NULL    |       |
    | birthdate | date        | YES  |     | NULL    |       |
    | birthtime | time        | YES  |     | NULL    |       |
    | birthdt   | datetime    | YES  |     | NULL    |       |
    +-----------+-------------+------+-----+---------+-------+
    
    select * from people;
    +---------+------------+-----------+---------------------+
    | name    | birthdate  | birthtime | birthdt             |
    +---------+------------+-----------+---------------------+
    | Padma   | 1983-11-11 | 10:07:35  | 1983-11-11 10:07:35 |
    | Larry   | 1943-12-25 | 04:10:42  | 1943-12-25 04:10:42 |
    | Toaster | 2017-04-21 | 19:12:43  | 2017-04-21 19:12:43 |
    +---------+------------+-----------+---------------------+
    3 rows

**CURDATE()** - Data atual;

    Select Curdate();
    +------------+
    | Curdate()  |
    +------------+
    | 2018-07-14 |
    +------------+

**CURTIME()** - Tempo Atual;

    select CURTIME();
    +-----------+
    | CURTIME() |
    +-----------+
    | 18:08:09  |
    +-----------+

**NOW()** - Data e tempo atuais;

    select now();
    +---------------------+
    | now()               |
    +---------------------+
    | 2018-07-14 18:08:33 |
    +---------------------+
    
Ex.:     

    insert into people (name, birthdate, birthtime, birthdt)
    values
    'Microwave', curdate(), curtime(), now());

    select * from people;
    
    +-----------+------------+-----------+---------------------+
    | name      | birthdate  | birthtime | birthdt             |
    +-----------+------------+-----------+---------------------+
    | Padma     | 1983-11-11 | 10:07:35  | 1983-11-11 10:07:35 |
    | Larry     | 1943-12-25 | 04:10:42  | 1943-12-25 04:10:42 |
    | Toaster   | 2017-04-21 | 19:12:43  | 2017-04-21 19:12:43 |
    | Microwave | 2018-07-14 | 18:13:16  | 2018-07-14 18:13:16 |
    +-----------+------------+-----------+---------------------+
    
## Formatando datas 

**DAY()**: Extrai o dia;

        select name, birthdate, day(birthdate) from people;                                                                        
        +-----------+------------+----------------+
        | name      | birthdate  | day(birthdate) |
        +-----------+------------+----------------+
        | Padma     | 1983-11-11 |             11 |
        | Larry     | 1943-12-25 |             25 |
        | Toaster   | 2017-04-21 |             21 |
        | Microwave | 2018-07-14 |             14 |
        +-----------+------------+----------------+

**DAYNAME()**:

    select name, birthdate, dayname(birthdate) from people;                                                                    
    +-----------+------------+--------------------+
    | name      | birthdate  | dayname(birthdate) |
    +-----------+------------+--------------------+
    | Padma     | 1983-11-11 | Friday             |
    | Larry     | 1943-12-25 | Saturday           |
    | Toaster   | 2017-04-21 | Friday             |
    | Microwave | 2018-07-14 | Saturday           |
    +-----------+------------+--------------------+
    4 rows

**DAYOFWEEK()**:

    select name, birthdate, dayofweek(birthdate) from people;                                                                  
    +-----------+------------+----------------------+
    | name      | birthdate  | dayofweek(birthdate) |
    +-----------+------------+----------------------+
    | Padma     | 1983-11-11 |                    6 |
    | Larry     | 1943-12-25 |                    7 |
    | Toaster   | 2017-04-21 |                    6 |
    | Microwave | 2018-07-14 |                    7 |
    +-----------+------------+----------------------+
    4 rows

**DAYOFYEAR()**:

    select name, birthdate, dayofyear(birthdate) from people;                                                                  
    +-----------+------------+----------------------+
    | name      | birthdate  | dayofyear(birthdate) |
    +-----------+------------+----------------------+
    | Padma     | 1983-11-11 |                  315 |
    | Larry     | 1943-12-25 |                  359 |
    | Toaster   | 2017-04-21 |                  111 |
    | Microwave | 2018-07-14 |                  195 |
    +-----------+------------+----------------------+
    4 rows

#### DATE_FORMAT(date,format)

    mysql> SELECT DATE_FORMAT('2009-10-04 22:23:00', '%W %M %Y');
            -> 'Sunday October 2009'
            
    mysql> SELECT DATE_FORMAT('2007-10-04 22:23:00', '%H:%i:%s');
            -> '22:23:00'
            
    mysql> SELECT DATE_FORMAT('1900-10-04 22:23:00', '%D %y %a %d %m %b %j');
            -> '4th 00 Thu 04 10 Oct 277'
            
    mysql> SELECT DATE_FORMAT('1997-10-04 22:23:00', '%H %k %I %r %T %S %w');
            -> '22 22 10 10:23:00 PM 22:23:00 00 6'
            
    mysql> SELECT DATE_FORMAT('1999-01-01', '%X %V');
            -> '1998 52'
            
    mysql> SELECT DATE_FORMAT('2006-06-00', '%d');
            -> '00'
            
            
    select date_format(now(), "%m/%d/%Y");
    +--------------------------------+
    | date_format(now(), "%m/%d/%Y") |
    +--------------------------------+
    | 07/14/2018                     |
    +--------------------------------+

    #https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_date-format
    
#### Funções de data

     https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html
     
## Operações com datas

#### datediff()

    select name, birthdate, datediff(now(), birthdate) from people;                                                            
    +-----------+------------+----------------------------+
    | name      | birthdate  | datediff(now(), birthdate) |
    +-----------+------------+----------------------------+
    | Padma     | 1983-11-11 |                      12664 |
    | Larry     | 1943-12-25 |                      27230 |
    | Toaster   | 2017-04-21 |                        449 |
    | Microwave | 2018-07-14 |                          0 |
    +-----------+------------+----------------------------+
    
#### date_add

    SELECT birthdt, DATE_ADD(birthdt, interval 1 month) from people;                                                           
    +---------------------+-------------------------------------+
    | birthdt             | DATE_ADD(birthdt, interval 1 month) |
    +---------------------+-------------------------------------+
    | 1983-11-11 10:07:35 | 1983-12-11 10:07:35                 |
    | 1943-12-25 04:10:42 | 1944-01-25 04:10:42                 |
    | 2017-04-21 19:12:43 | 2017-05-21 19:12:43                 |
    | 2018-07-14 18:13:16 | 2018-08-14 18:13:16                 |
    +---------------------+-------------------------------------+
    
    SELECT birthdt, birthdt + INTERVAL 15 MONTH + INTERVAL 10 HOUR FROM people;
    +---------------------+------------------------------------------------+
    | birthdt             | birthdt + INTERVAL 15 MONTH + INTERVAL 10 HOUR |
    +---------------------+------------------------------------------------+
    | 1983-11-11 10:07:35 | 1985-02-11 20:07:35                            |
    | 1943-12-25 04:10:42 | 1945-03-25 14:10:42                            |
    | 2017-04-21 19:12:43 | 2018-07-22 05:12:43                            |
    | 2018-07-14 18:13:16 | 2019-10-15 04:13:16                            |
    +---------------------+------------------------------------------------+   
    
#### TIMESTAMPS

**datetime vs timestamp**:  são identicos mas suportam periodos diferentes

timestamp vai de '1970-01-01 00:00:01' UTC até '2038-01-19 03:14:07' UTC      
     
datetime vai de '1000-01-01 00:00:01' até '9999-12-31 23:59:59'

Timestamp é mais rápido, 4 bytes vs 8 bytes do DateTime.   


Inserindo a data de criação automaticamente

    CREATE TABLE comments(
             content VARCHAR(100),
             create_at TIMESTAMP DEFAULT NOW()        
         ); 
         
         
    INSERT INTO comments (content) VALUES ('lol what a funny article');
    INSERT INTO comments (content) VALUES ('I found this offensive');
    
    select * from comments;
    
    +--------------------------+---------------------+
    | content                  | create_at           |
    +--------------------------+---------------------+
    | lol what a funny article | 2018-07-14 19:22:36 |
    | I found this offensive   | 2018-07-14 19:23:06 |
    +--------------------------+---------------------+
    
    mysql> UPDATE comments SET content='Not a lot funny' WHERE content='lol what a funny article';
   
    mysql> select * from comments;
    +------------------------+---------------------+
    | content                | create_at           |
    +------------------------+---------------------+
    | Not a lot funny        | 2018-07-14 19:22:36 |
    | I found this offensive | 2018-07-14 19:23:06 |
    +------------------------+---------------------+
    
Inserindo data de update automaticamente

    CREATE TABLE comments2(
             content VARCHAR(100),
             changed_at TIMESTAMP DEFAULT NOW() ON UPDATE CURRENT_TIMESTAMP        
         ); 
         
         
    INSERT INTO comments2 (content) VALUES ('LOL LOL LOL');
    INSERT INTO comments2 (content) VALUES ('I Like cats and dogs');
    
    select * from comments2;
    +----------------------+---------------------+
    | content              | changed_at          |
    +----------------------+---------------------+
    | LOL LOL LOL          | 2018-07-14 19:29:59 |
    | I Like cats and dogs | 2018-07-14 19:30:01 |
    +----------------------+---------------------+
    
    UPDATE comments2 SET content='THIS IS NOT GIBBERISH' WHERE content='LOL LOL LOL';
    
    select * from comments2;
    +-----------------------+---------------------+
    | content               | changed_at          |
    +-----------------------+---------------------+
    | THIS IS NOT GIBBERISH | 2018-07-14 19:32:23 |
    | I Like cats and dogs  | 2018-07-14 19:30:01 |
    +-----------------------+---------------------+
   
   
Nota: CURRENT_TIMESTAMP faz o mesmo que NOW();

#### Operadores lógicos

https://dev.mysql.com/doc/refman/8.0/en/non-typed-operators.html
https://dev.mysql.com/doc/refman/8.0/en/comparison-operators.html

**Diferente de (Not Equal)**: Resultado inverso do operador igual

    !=

**Not Like**: Resultados Inverso do operador Like
 
     NOT LIKE 'W%';
     
**Maior que e Menor que**

    >
    <

**Maior ou igual que e Menor ou igual que**    

    >=
    <=
        
Os operadores podem ser usados junto com where para retornar algum resultado ou como teste lógico usando apenas o select

    +------+
    | 99>1 |
    +------+
    |    1 |
    +------+            

Neste caso acima retornou true

    select 'b'>'a';
    +---------+
    | 'b'>'a' |
    +---------+
    |       1 |
    +---------+
    
    select 'a'>'b';
    +---------+
    | 'a'>'b' |
    +---------+
    |       0 |
    +---------+
    
    select 'A'>'a';
    +---------+
    | 'A'>'a' |
    +---------+
    |       0 |
    +---------+
    
    select 'A'='a';
    +---------+
    | 'A'='a' |
    +---------+
    |       1 |
    +---------+
    
** And **
    
    &&
    
Combina duas condições, os resultados deveram atender ambas as condições, ambas devem true.

    Select title, author_lname, released_year from books where author_lname = 'Eggers' And released_year > 2010;   
    +----------------------------------+--------------+---------------+
    | title                            | author_lname | released_year |
    +----------------------------------+--------------+---------------+
    | A Hologram for the King: A Novel | Eggers       |          2012 |
    | The Circle                       | Eggers       |          2013 |
    +----------------------------------+--------------+---------------+
    2 rows
    
    Select title, author_lname, released_year from books where author_lname = 'Eggers' && released_year > 2010;   
    +----------------------------------+--------------+---------------+
    | title                            | author_lname | released_year |
    +----------------------------------+--------------+---------------+
    | A Hologram for the King: A Novel | Eggers       |          2012 |
    | The Circle                       | Eggers       |          2013 |
    +----------------------------------+--------------+---------------+
    2 rows
    
    Select title, author_lname, released_year from books where author_lname = 'Eggers';
    +-------------------------------------------+--------------+---------------+
    | title                                     | author_lname | released_year |
    +-------------------------------------------+--------------+---------------+
    | A Hologram for the King: A Novel          | Eggers       |          2012 |
    | The Circle                                | Eggers       |          2013 |
    | A Heartbreaking Work of Staggering Genius | Eggers       |          2001 |
    +-------------------------------------------+--------------+---------------+
    3 row

**OU (OR)**

    ||
    OR
    
Trara os resultados que satisfasem uma ou mais condições.

    Select title, author_lname, released_year from books where author_lname = 'Eggers' || released_year > 2010;             
    +-------------------------------------------+--------------+---------------+
    | title                                     | author_lname | released_year |
    +-------------------------------------------+--------------+---------------+
    | Norse Mythology                           | Gaiman       |          2016 |
    | A Hologram for the King: A Novel          | Eggers       |          2012 |
    | The Circle                                | Eggers       |          2013 |
    | A Heartbreaking Work of Staggering Genius | Eggers       |          2001 |
    | 10% Happier                               | Harris       |          2014 |
    | Lincoln In The Bardo                      | Saunders     |          2017 |
    +-------------------------------------------+--------------+---------------+
    6 row
    
**Entre (BETWEEN)**    

    Select title, author_lname, released_year from books where author_lname = 'Eggers' And released_year > 2010;   
    +----------------------------------+--------------+---------------+
    | title                            | author_lname | released_year |
    +----------------------------------+--------------+---------------+
    | A Hologram for the King: A Novel | Eggers       |          2012 |
    | The Circle                       | Eggers       |          2013 |
    +----------------------------------+--------------+---------------+
    2 rows in set (0.00 sec)
    
    mysql>   Select title, author_lname, released_year from books where author_lname = 'Eggers' || released_year > 2010;             
    +-------------------------------------------+--------------+---------------+
    | title                                     | author_lname | released_year |
    +-------------------------------------------+--------------+---------------+
    | Norse Mythology                           | Gaiman       |          2016 |
    | A Hologram for the King: A Novel          | Eggers       |          2012 |
    | The Circle                                | Eggers       |          2013 |
    | A Heartbreaking Work of Staggering Genius | Eggers       |          2001 |
    | 10% Happier                               | Harris       |          2014 |
    | Lincoln In The Bardo                      | Saunders     |          2017 |
    +-------------------------------------------+--------------+---------------+
    6 rows in set (0.01 sec)
    
    mysql> SELECT title, released_year FROM books;
    +-----------------------------------------------------+---------------+
    | title                                               | released_year |
    +-----------------------------------------------------+---------------+
    | The Namesake                                        |          2003 |
    | Norse Mythology                                     |          2016 |
    | American Gods                                       |          2001 |
    | Interpreter of Maladies                             |          1996 |
    | A Hologram for the King: A Novel                    |          2012 |
    | The Circle                                          |          2013 |
    | The Amazing Adventures of Kavalier & Clay           |          2000 |
    | Just Kids                                           |          2010 |
    | A Heartbreaking Work of Staggering Genius           |          2001 |
    | Coraline                                            |          2003 |
    | What We Talk About When We Talk About Love: Stories |          1981 |
    | Where I'm Calling From: Selected Stories            |          1989 |
    | White Noise                                         |          1985 |
    | Cannery Row                                         |          1945 |
    | Oblivion: Stories                                   |          2004 |
    | Consider the Lobster                                |          2005 |
    | 10% Happier                                         |          2014 |
    | fake_book                                           |          2001 |
    | Lincoln In The Bardo                                |          2017 |
    +-----------------------------------------------------+---------------+
    19 rows
    
    SELECT title, released_year FROM books WHERE released_year >= 2004 && released_year <= 2015;
    +----------------------------------+---------------+
    | title                            | released_year |
    +----------------------------------+---------------+
    | A Hologram for the King: A Novel |          2012 |
    | The Circle                       |          2013 |
    | Just Kids                        |          2010 |
    | Oblivion: Stories                |          2004 |
    | Consider the Lobster             |          2005 |
    | 10% Happier                      |          2014 |
    +----------------------------------+---------------+
    6 rows
    
    SELECT title, released_year FROM books WHERE released_year BETWEEN 2004 AND 2015;
    +----------------------------------+---------------+
    | title                            | released_year |
    +----------------------------------+---------------+
    | A Hologram for the King: A Novel |          2012 |
    | The Circle                       |          2013 |
    | Just Kids                        |          2010 |
    | Oblivion: Stories                |          2004 |
    | Consider the Lobster             |          2005 |
    | 10% Happier                      |          2014 |
    +----------------------------------+---------------+
    6 rows
    
Note que And depois do between sera entido como parte dele não como o operador and. Between é inclusivo.
    
**Não esta Entre (NOT BETWEEN)**    

    SELECT title, released_year FROM books WHERE released_year NOT BETWEEN 2004 AND 2015;
    +-----------------------------------------------------+---------------+
    | title                                               | released_year |
    +-----------------------------------------------------+---------------+
    | The Namesake                                        |          2003 |
    | Norse Mythology                                     |          2016 |
    | American Gods                                       |          2001 |
    | Interpreter of Maladies                             |          1996 |
    | The Amazing Adventures of Kavalier & Clay           |          2000 |
    | A Heartbreaking Work of Staggering Genius           |          2001 |
    | Coraline                                            |          2003 |
    | What We Talk About When We Talk About Love: Stories |          1981 |
    | Where I'm Calling From: Selected Stories            |          1989 |
    | White Noise                                         |          1985 |
    | Cannery Row                                         |          1945 |
    | fake_book                                           |          2001 |
    | Lincoln In The Bardo                                |          2017 |
    +-----------------------------------------------------+---------------+
    13 rows

**Comparação Entre datas**    

Use a função cast()
    
    SELECT CAST('2017-05-02' AS DATETIME);
    +--------------------------------+
    | CAST('2017-05-02' AS DATETIME) |
    +--------------------------------+
    | 2017-05-02 00:00:00            |
    +--------------------------------+
    
    SELECT * FROM people;
    +-----------+------------+-----------+---------------------+
    | name      | birthdate  | birthtime | birthdt             |
    +-----------+------------+-----------+---------------------+
    | Padma     | 1983-11-11 | 10:07:35  | 1983-11-11 10:07:35 |
    | Larry     | 1943-12-25 | 04:10:42  | 1943-12-25 04:10:42 |
    | Toaster   | 2017-04-21 | 19:12:43  | 2017-04-21 19:12:43 |
    | Microwave | 2018-07-14 | 18:13:16  | 2018-07-14 18:13:16 |
    +-----------+------------+-----------+---------------------+
    4 rows

Observe o código abaixo, a query irá funcionar pois o Mysql ira usar sua inteligencia e código de contesão para converter
a string do between para data, pode haver situações em que isto não acontece por isso é boa pratica usar o cast. 

    SELECT name, birthdt FROM people WHERE birthdt BETWEEN '1980-01-01' AND '2000-01-01';
    +-------+---------------------+
    | name  | birthdt             |
    +-------+---------------------+
    | Padma | 1983-11-11 10:07:35 |
    +-------+---------------------+
    
Assim ficaria mais seguro

    SELECT name, birthdt FROM people WHERE birthdt BETWEEN CAST('1980-01-01' AS DATETIME) AND CAST('2000-01-01' AS DATETIME);
    +-------+---------------------+
    | name  | birthdt             |
    +-------+---------------------+
    | Padma | 1983-11-11 10:07:35 |
    +-------+---------------------+    

**IN AND NOT IN (IN) (NOT IN)**

    mysql> SELECT title, author_lname FROM books WHERE 
        -> author_lname='Carver' OR
        -> author_lname='Lahiri' OR
        -> author_lname='Smith';
    +-----------------------------------------------------+--------------+
    | title                                               | author_lname |
    +-----------------------------------------------------+--------------+
    | The Namesake                                        | Lahiri       |
    | Interpreter of Maladies                             | Lahiri       |
    | Just Kids                                           | Smith        |
    | What We Talk About When We Talk About Love: Stories | Carver       |
    | Where I'm Calling From: Selected Stories            | Carver       |
    +-----------------------------------------------------+--------------+
    5 rows
    
    SELECT title, author_lname FROM books WHERE author_lname IN ('Carver', 'Lahiri', 'Smith');
    +-----------------------------------------------------+--------------+
    | title                                               | author_lname |
    +-----------------------------------------------------+--------------+
    | The Namesake                                        | Lahiri       |
    | Interpreter of Maladies                             | Lahiri       |
    | Just Kids                                           | Smith        |
    | What We Talk About When We Talk About Love: Stories | Carver       |
    | Where I'm Calling From: Selected Stories            | Carver       |
    +-----------------------------------------------------+--------------+
    5 rows
    
NOT IN

    SELECT title, released_year FROM books WHERE released_year >= 2000 
        -> AND released_year NOT IN
        -> (2000, 2002, 2004, 2006, 2008, 2010, 2012, 2014, 2016);
    +-------------------------------------------+---------------+
    | title                                     | released_year |
    +-------------------------------------------+---------------+
    | The Namesake                              |          2003 |
    | American Gods                             |          2001 |
    | The Circle                                |          2013 |
    | A Heartbreaking Work of Staggering Genius |          2001 |
    | Coraline                                  |          2003 |
    | Consider the Lobster                      |          2005 |
    | fake_book                                 |          2001 |
    | Lincoln In The Bardo                      |          2017 |
    +-------------------------------------------+---------------+
    8 rows
    
    mysql> SELECT title, released_year FROM books WHERE released_year >= 2000 
        -> AND released_year % 2 != 0;
    +-------------------------------------------+---------------+
    | title                                     | released_year |
    +-------------------------------------------+---------------+
    | The Namesake                              |          2003 |
    | American Gods                             |          2001 |
    | The Circle                                |          2013 |
    | A Heartbreaking Work of Staggering Genius |          2001 |
    | Coraline                                  |          2003 |
    | Consider the Lobster                      |          2005 |
    | fake_book                                 |          2001 |
    | Lincoln In The Bardo                      |          2017 |
    +-------------------------------------------+---------------+
    8 rows
    
    
**CASE STATEMENTS - CONDICIONAIS**

Oberserve que a condicional abaixo funciona como o if no php

    SELECT title, released_year, 
        CASE WHEN released_year >= 2000 
                THEN 'Modern Lit'
                ELSE '20th Century Lit'
        END AS genre
    FROM books;
    +-----------------------------------------------------+---------------+------------------+
    | title                                               | released_year | genre            |
    +-----------------------------------------------------+---------------+------------------+
    | The Namesake                                        |          2003 | Modern Lit       |
    | Norse Mythology                                     |          2016 | Modern Lit       |
    | American Gods                                       |          2001 | Modern Lit       |
    | Interpreter of Maladies                             |          1996 | 20th Century Lit |
    | A Hologram for the King: A Novel                    |          2012 | Modern Lit       |
    | The Circle                                          |          2013 | Modern Lit       |
    | The Amazing Adventures of Kavalier & Clay           |          2000 | Modern Lit       |
    | Just Kids                                           |          2010 | Modern Lit       |
    | A Heartbreaking Work of Staggering Genius           |          2001 | Modern Lit       |
    | Coraline                                            |          2003 | Modern Lit       |
    | What We Talk About When We Talk About Love: Stories |          1981 | 20th Century Lit |
    | Where I'm Calling From: Selected Stories            |          1989 | 20th Century Lit |
    | White Noise                                         |          1985 | 20th Century Lit |
    | Cannery Row                                         |          1945 | 20th Century Lit |
    | Oblivion: Stories                                   |          2004 | Modern Lit       |
    | Consider the Lobster                                |          2005 | Modern Lit       |
    | 10% Happier                                         |          2014 | Modern Lit       |
    | fake_book                                           |          2001 | Modern Lit       |
    | Lincoln In The Bardo                                |          2017 | Modern Lit       |
    +-----------------------------------------------------+---------------+------------------+
    19 rows
    
    SELECT title, stock_quantity, 
            CASE WHEN stock_quantity BETWEEN 0 AND 50 THEN '*' 
                 WHEN stock_quantity BETWEEN 51 AND 100 THEN '**'
                 ELSE '***'
            END AS STOCK
    FROM books;
    +-----------------------------------------------------+----------------+-------+
    | title                                               | stock_quantity | STOCK |
    +-----------------------------------------------------+----------------+-------+
    | The Namesake                                        |             32 | *     |
    | Norse Mythology                                     |             43 | *     |
    | American Gods                                       |             12 | *     |
    | Interpreter of Maladies                             |             97 | **    |
    | A Hologram for the King: A Novel                    |            154 | ***   |
    | The Circle                                          |             26 | *     |
    | The Amazing Adventures of Kavalier & Clay           |             68 | **    |
    | Just Kids                                           |             55 | **    |
    | A Heartbreaking Work of Staggering Genius           |            104 | ***   |
    | Coraline                                            |            100 | **    |
    | What We Talk About When We Talk About Love: Stories |             23 | *     |
    | Where I'm Calling From: Selected Stories            |             12 | *     |
    | White Noise                                         |             49 | *     |
    | Cannery Row                                         |             95 | **    |
    | Oblivion: Stories                                   |            172 | ***   |
    | Consider the Lobster                                |             92 | **    |
    | 10% Happier                                         |             29 | *     |
    | fake_book                                           |            287 | ***   |
    | Lincoln In The Bardo                                |           1000 | ***   |
    +-----------------------------------------------------+----------------+-------+
    19 rows

    mysql>     SELECT title,
        ->             CASE WHEN stock_quantity BETWEEN 0 AND 50 THEN '*' 
        ->                  WHEN stock_quantity BETWEEN 51 AND 100 THEN '**'
        ->                  ELSE '***'
        ->             END AS STOCK
        ->     FROM books;
    +-----------------------------------------------------+-------+
    | title                                               | STOCK |
    +-----------------------------------------------------+-------+
    | The Namesake                                        | *     |
    | Norse Mythology                                     | *     |
    | American Gods                                       | *     |
    | Interpreter of Maladies                             | **    |
    | A Hologram for the King: A Novel                    | ***   |
    | The Circle                                          | *     |
    | The Amazing Adventures of Kavalier & Clay           | **    |
    | Just Kids                                           | **    |
    | A Heartbreaking Work of Staggering Genius           | ***   |
    | Coraline                                            | **    |
    | What We Talk About When We Talk About Love: Stories | *     |
    | Where I'm Calling From: Selected Stories            | *     |
    | White Noise                                         | *     |
    | Cannery Row                                         | **    |
    | Oblivion: Stories                                   | ***   |
    | Consider the Lobster                                | **    |
    | 10% Happier                                         | *     |
    | fake_book                                           | ***   |
    | Lincoln In The Bardo                                | ***   |
    +-----------------------------------------------------+-------+
    19 rows
    
    mysql>     SELECT title,
        ->             CASE WHEN stock_quantity <=50 THEN '*' 
        ->                  WHEN stock_quantity <=100 THEN '**'
        ->                  ELSE '***'
        ->             END AS STOCK
        ->     FROM books;
    +-----------------------------------------------------+-------+
    | title                                               | STOCK |
    +-----------------------------------------------------+-------+
    | The Namesake                                        | *     |
    | Norse Mythology                                     | *     |
    | American Gods                                       | *     |
    | Interpreter of Maladies                             | **    |
    | A Hologram for the King: A Novel                    | ***   |
    | The Circle                                          | *     |
    | The Amazing Adventures of Kavalier & Clay           | **    |
    | Just Kids                                           | **    |
    | A Heartbreaking Work of Staggering Genius           | ***   |
    | Coraline                                            | **    |
    | What We Talk About When We Talk About Love: Stories | *     |
    | Where I'm Calling From: Selected Stories            | *     |
    | White Noise                                         | *     |
    | Cannery Row                                         | **    |
    | Oblivion: Stories                                   | ***   |
    | Consider the Lobster                                | **    |
    | 10% Happier                                         | *     |
    | fake_book                                           | ***   |
    | Lincoln In The Bardo                                | ***   |
    +-----------------------------------------------------+-------+
    19 rows
    
    mysql> select title, author_lname, 
        ->     case 
        ->         when count(*) <= 1 then concat( count(*),' book')
        ->         else concat( count(*), ' books')
        ->     end as 'COUNT'
        -> from books group by author_lname, author_fname;
    +-----------------------------------------------------+----------------+---------+
    | title                                               | author_lname   | COUNT   |
    +-----------------------------------------------------+----------------+---------+
    | What We Talk About When We Talk About Love: Stories | Carver         | 2 books |
    | The Amazing Adventures of Kavalier & Clay           | Chabon         | 1 book  |
    | White Noise                                         | DeLillo        | 1 book  |
    | A Hologram for the King: A Novel                    | Eggers         | 3 books |
    | Oblivion: Stories                                   | Foster Wallace | 2 books |
    | Norse Mythology                                     | Gaiman         | 3 books |
    | 10% Happier                                         | Harris         | 1 book  |
    | fake_book                                           | Harris         | 1 book  |
    | The Namesake                                        | Lahiri         | 2 books |
    | Lincoln In The Bardo                                | Saunders       | 1 book  |
    | Just Kids                                           | Smith          | 1 book  |
    | Cannery Row                                         | Steinbeck      | 1 book  |
    +-----------------------------------------------------+----------------+---------+
    
    mysql> select title, author_lname,
        ->     case
        ->         when title like '%stories%' then 'Short Stories'
        ->         when title like '%Just Kids%' or title like '%Heartbreaking Work%' then 'Memoir'
        ->         else 'Novel'
        ->     end as 'TYPE'
        -> from books; 
    +-----------------------------------------------------+----------------+---------------+
    | title                                               | author_lname   | TYPE          |
    +-----------------------------------------------------+----------------+---------------+
    | The Namesake                                        | Lahiri         | Novel         |
    | Norse Mythology                                     | Gaiman         | Novel         |
    | American Gods                                       | Gaiman         | Novel         |
    | Interpreter of Maladies                             | Lahiri         | Novel         |
    | A Hologram for the King: A Novel                    | Eggers         | Novel         |
    | The Circle                                          | Eggers         | Novel         |
    | The Amazing Adventures of Kavalier & Clay           | Chabon         | Novel         |
    | Just Kids                                           | Smith          | Memoir        |
    | A Heartbreaking Work of Staggering Genius           | Eggers         | Memoir        |
    | Coraline                                            | Gaiman         | Novel         |
    | What We Talk About When We Talk About Love: Stories | Carver         | Short Stories |
    | Where I'm Calling From: Selected Stories            | Carver         | Short Stories |
    | White Noise                                         | DeLillo        | Novel         |
    | Cannery Row                                         | Steinbeck      | Novel         |
    | Oblivion: Stories                                   | Foster Wallace | Short Stories |
    | Consider the Lobster                                | Foster Wallace | Novel         |
    | 10% Happier                                         | Harris         | Novel         |
    | fake_book                                           | Harris         | Novel         |
    | Lincoln In The Bardo                                | Saunders       | Novel         |
    +-----------------------------------------------------+----------------+---------------+
    19 rows

***Exemplo de problema**

Encontrar autores que comecem com a letra c ou s

    +-----------------------------------------------------+--------------+
    | title                                               | author_lname |
    +-----------------------------------------------------+--------------+
    | The Amazing Adventures of Kavalier & Clay           | Chabon       |
    | Just Kids                                           | Smith        |
    | What We Talk About When We Talk About Love: Stories | Carver       |
    | Where I'm Calling From: Selected Stories            | Carver       |
    | Cannery Row                                         | Steinbeck    |
    | Lincoln In The Bardo                                | Saunders     |
    +-----------------------------------------------------+--------------+
    6 rows 
    
Podemos usar três abordagem para obter este resultado

    select title, author_lname from books
    where substr(author_lname,1,1) IN ('C','S');
    
    select author_fname, author_lname from books 
    where substr(author_lname,1,1) = 'c'
    or substr(author_lname,1,1) = 's';
    
    select author_fname, author_lname from books 
    where author_lname like 'c%'
    or author_lname like 's%';   
    
##Relação um para muitos - One to Many

    CREATE TABLE customers(
        id INT AUTO_INCREMENT PRIMARY KEY,
        first_name VARCHAR(100),
        last_name VARCHAR(100),
        email VARCHAR(100)
    );
    CREATE TABLE orders(
        id INT AUTO_INCREMENT PRIMARY KEY,
        order_date DATE,
        amount DECIMAL(8,2),
        customer_id INT,
        FOREIGN KEY(customer_id) REFERENCES customers(id)
    );
    
    select * from orders;
    +----+------------+--------+-------------+
    | id | order_date | amount | customer_id |
    +----+------------+--------+-------------+
    |  1 | 2016-02-10 |  99.99 |           1 |
    |  2 | 2017-11-11 |  35.50 |           1 |
    |  3 | 2014-12-12 | 800.67 |           2 |
    |  4 | 2015-01-03 |  12.50 |           2 |
    |  5 | 1999-04-11 | 450.25 |           5 |
    +----+------------+--------+-------------+
    5 row
    
    select * from customers;
    +----+------------+-----------+------------------+
    | id | first_name | last_name | email            |
    +----+------------+-----------+------------------+
    |  1 | Boy        | George    | george@gmail.com |
    |  2 | George     | Michael   | gm@gmail.com     |
    |  3 | David      | Bowie     | david@gmail.com  |
    |  4 | Blue       | Steele    | blue@gmail.com   |
    |  5 | Bette      | Davis     | bette@aol.com    |
    +----+------------+-----------+------------------+
    5 rows  
    
    
O select das orders de um cliente seria algo como

    SELECT * FROM orders WHERE customer_id = 1;
    
    ou 
    
    SELECT * FROM orders WHERE customer_id = (
        SELECT id FROM customers
        WHERE last_name = 'George'
    );
    
    +----+------------+--------+-------------+
    | id | order_date | amount | customer_id |
    +----+------------+--------+-------------+
    |  1 | 2016-02-10 |  99.99 |           1 |
    |  2 | 2017-11-11 |  35.50 |           1 |
    +----+------------+--------+-------------+      
    
Um select cross join juntando as tabelas custumer e orders;

     SELECT * FROM customers, orders;
    +----+------------+-----------+------------------+----+------------+--------+-------------+
    | id | first_name | last_name | email            | id | order_date | amount | customer_id |
    +----+------------+-----------+------------------+----+------------+--------+-------------+
    |  1 | Boy        | George    | george@gmail.com |  1 | 2016-02-10 |  99.99 |           1 |
    |  2 | George     | Michael   | gm@gmail.com     |  1 | 2016-02-10 |  99.99 |           1 |
    |  3 | David      | Bowie     | david@gmail.com  |  1 | 2016-02-10 |  99.99 |           1 |
    |  4 | Blue       | Steele    | blue@gmail.com   |  1 | 2016-02-10 |  99.99 |           1 |
    |  5 | Bette      | Davis     | bette@aol.com    |  1 | 2016-02-10 |  99.99 |           1 |
    |  1 | Boy        | George    | george@gmail.com |  2 | 2017-11-11 |  35.50 |           1 |
    |  2 | George     | Michael   | gm@gmail.com     |  2 | 2017-11-11 |  35.50 |           1 |
    |  3 | David      | Bowie     | david@gmail.com  |  2 | 2017-11-11 |  35.50 |           1 |
    |  4 | Blue       | Steele    | blue@gmail.com   |  2 | 2017-11-11 |  35.50 |           1 |
    |  5 | Bette      | Davis     | bette@aol.com    |  2 | 2017-11-11 |  35.50 |           1 |
    |  1 | Boy        | George    | george@gmail.com |  3 | 2014-12-12 | 800.67 |           2 |
    |  2 | George     | Michael   | gm@gmail.com     |  3 | 2014-12-12 | 800.67 |           2 |
    |  3 | David      | Bowie     | david@gmail.com  |  3 | 2014-12-12 | 800.67 |           2 |
    |  4 | Blue       | Steele    | blue@gmail.com   |  3 | 2014-12-12 | 800.67 |           2 |
    |  5 | Bette      | Davis     | bette@aol.com    |  3 | 2014-12-12 | 800.67 |           2 |
    |  1 | Boy        | George    | george@gmail.com |  4 | 2015-01-03 |  12.50 |           2 |
    |  2 | George     | Michael   | gm@gmail.com     |  4 | 2015-01-03 |  12.50 |           2 |
    |  3 | David      | Bowie     | david@gmail.com  |  4 | 2015-01-03 |  12.50 |           2 |
    |  4 | Blue       | Steele    | blue@gmail.com   |  4 | 2015-01-03 |  12.50 |           2 |
    |  5 | Bette      | Davis     | bette@aol.com    |  4 | 2015-01-03 |  12.50 |           2 |
    |  1 | Boy        | George    | george@gmail.com |  5 | 1999-04-11 | 450.25 |           5 |
    |  2 | George     | Michael   | gm@gmail.com     |  5 | 1999-04-11 | 450.25 |           5 |
    |  3 | David      | Bowie     | david@gmail.com  |  5 | 1999-04-11 | 450.25 |           5 |
    |  4 | Blue       | Steele    | blue@gmail.com   |  5 | 1999-04-11 | 450.25 |           5 |
    |  5 | Bette      | Davis     | bette@aol.com    |  5 | 1999-04-11 | 450.25 |           5 |
    +----+------------+-----------+------------------+----+------------+--------+-------------+
    25 rows  

Este exemplo acima é um join simples de tabelas, na maioria dos casos há melhores maieras de fazer. Esta pegando cada
custumer e dando um join com cada order (5 orders * 5 customers = 25 rows) todas as combinações de custumers e orders são
executadas nesta query.


####Inner Join Implicito

Em uma cross join onde relacionacemos cada ordem com seu custumer seria algo como:

    SELECT * FROM customers, orders WHERE customers.id = orders.customer_id;
    +----+------------+-----------+------------------+----+------------+--------+-------------+
    | id | first_name | last_name | email            | id | order_date | amount | customer_id |
    +----+------------+-----------+------------------+----+------------+--------+-------------+
    |  1 | Boy        | George    | george@gmail.com |  1 | 2016-02-10 |  99.99 |           1 |
    |  1 | Boy        | George    | george@gmail.com |  2 | 2017-11-11 |  35.50 |           1 |
    |  2 | George     | Michael   | gm@gmail.com     |  3 | 2014-12-12 | 800.67 |           2 |
    |  2 | George     | Michael   | gm@gmail.com     |  4 | 2015-01-03 |  12.50 |           2 |
    |  5 | Bette      | Davis     | bette@aol.com    |  5 | 1999-04-11 | 450.25 |           5 |
    +----+------------+-----------+------------------+----+------------+--------+-------------+
    5 rows
    
Note que temos 5 orders e 5 linhas note o id da tabela orderm bate com customer_id.

    SELECT first_name, last_name, order_date, amount  FROM customers, orders WHERE customers.id = orders.customer_id;          
    +------------+-----------+------------+--------+
    | first_name | last_name | order_date | amount |
    +------------+-----------+------------+--------+
    | Boy        | George    | 2016-02-10 |  99.99 |
    | Boy        | George    | 2017-11-11 |  35.50 |
    | George     | Michael   | 2014-12-12 | 800.67 |
    | George     | Michael   | 2015-01-03 |  12.50 |
    | Bette      | Davis     | 1999-04-11 | 450.25 |
    +------------+-----------+------------+--------+
    5 rows
    
####Inner Join Explicito

    SELECT * FROM customers
        JOIN orders
            ON customers.id = orders.customer_id;
    +----+------------+-----------+------------------+----+------------+--------+-------------+
    | id | first_name | last_name | email            | id | order_date | amount | customer_id |
    +----+------------+-----------+------------------+----+------------+--------+-------------+
    |  1 | Boy        | George    | george@gmail.com |  1 | 2016-02-10 |  99.99 |           1 |
    |  1 | Boy        | George    | george@gmail.com |  2 | 2017-11-11 |  35.50 |           1 |
    |  2 | George     | Michael   | gm@gmail.com     |  3 | 2014-12-12 | 800.67 |           2 |
    |  2 | George     | Michael   | gm@gmail.com     |  4 | 2015-01-03 |  12.50 |           2 |
    |  5 | Bette      | Davis     | bette@aol.com    |  5 | 1999-04-11 | 450.25 |           5 |
    +----+------------+-----------+------------------+----+------------+--------+-------------+
    5 rows
    
    SELECT * FROM customers
        JOIN orders
            ON orders.customer_id = customers.id;
    +----+------------+-----------+------------------+----+------------+--------+-------------+
    | id | first_name | last_name | email            | id | order_date | amount | customer_id |
    +----+------------+-----------+------------------+----+------------+--------+-------------+
    |  1 | Boy        | George    | george@gmail.com |  1 | 2016-02-10 |  99.99 |           1 |
    |  1 | Boy        | George    | george@gmail.com |  2 | 2017-11-11 |  35.50 |           1 |
    |  2 | George     | Michael   | gm@gmail.com     |  3 | 2014-12-12 | 800.67 |           2 |
    |  2 | George     | Michael   | gm@gmail.com     |  4 | 2015-01-03 |  12.50 |           2 |
    |  5 | Bette      | Davis     | bette@aol.com    |  5 | 1999-04-11 | 450.25 |           5 |
    +----+------------+-----------+------------------+----+------------+--------+-------------+
    5 rows
    
    SELECT * FROM orders
        JOIN customers
            ON customers.id = orders.customer_id;
    +----+------------+--------+-------------+----+------------+-----------+------------------+
    | id | order_date | amount | customer_id | id | first_name | last_name | email            |
    +----+------------+--------+-------------+----+------------+-----------+------------------+
    |  1 | 2016-02-10 |  99.99 |           1 |  1 | Boy        | George    | george@gmail.com |
    |  2 | 2017-11-11 |  35.50 |           1 |  1 | Boy        | George    | george@gmail.com |
    |  3 | 2014-12-12 | 800.67 |           2 |  2 | George     | Michael   | gm@gmail.com     |
    |  4 | 2015-01-03 |  12.50 |           2 |  2 | George     | Michael   | gm@gmail.com     |
    |  5 | 1999-04-11 | 450.25 |           5 |  5 | Bette      | Davis     | bette@aol.com    |
    +----+------------+--------+-------------+----+------------+-----------+------------------+
    5 rows
    
Note como a ordem dos fatores não afeta o resultado.

    SELECT first_name, last_name, order_date, amount FROM orders
        JOIN customers
            ON customers.id = orders.customer_id;

    +------------+-----------+------------+--------+
    | first_name | last_name | order_date | amount |
    +------------+-----------+------------+--------+
    | Boy        | George    | 2016-02-10 |  99.99 |
    | Boy        | George    | 2017-11-11 |  35.50 |
    | George     | Michael   | 2014-12-12 | 800.67 |
    | George     | Michael   | 2015-01-03 |  12.50 |
    | Bette      | Davis     | 1999-04-11 | 450.25 |
    +------------+-----------+------------+--------+
    5 rows
    
    SELECT first_name, last_name, order_date, amount FROM orders
        INNER JOIN customers
            ON customers.id = orders.customer_id;
    +------------+-----------+------------+--------+
    | first_name | last_name | order_date | amount |
    +------------+-----------+------------+--------+
    | Boy        | George    | 2016-02-10 |  99.99 |
    | Boy        | George    | 2017-11-11 |  35.50 |
    | George     | Michael   | 2014-12-12 | 800.67 |
    | George     | Michael   | 2015-01-03 |  12.50 |
    | Bette      | Davis     | 1999-04-11 | 450.25 |
    +------------+-----------+------------+--------+
    5 rows in set (0.00 sec)
    
Ambas as querys acima são a mesma o inner pode ser omitido.    
    
Outros Exemplos 
    
    mysql> select first_name, last_name, order_date, amount
        -> from customers
        -> join orders on customers.id = orders.id order by amount;
    +------------+-----------+------------+--------+
    | first_name | last_name | order_date | amount |
    +------------+-----------+------------+--------+
    | Blue       | Steele    | 2015-01-03 |  12.50 |
    | George     | Michael   | 2017-11-11 |  35.50 |
    | Boy        | George    | 2016-02-10 |  99.99 |
    | Bette      | Davis     | 1999-04-11 | 450.25 |
    | David      | Bowie     | 2014-12-12 | 800.67 |
    +------------+-----------+------------+--------+
    5 rows
    
    mysql> select first_name, last_name, order_date, amount
        -> from customers
        -> join orders on customers.id = orders.id group by orders.customer_id;
    +------------+-----------+------------+--------+
    | first_name | last_name | order_date | amount |
    +------------+-----------+------------+--------+
    | Boy        | George    | 2016-02-10 |  99.99 |
    | David      | Bowie     | 2014-12-12 | 800.67 |
    | Bette      | Davis     | 1999-04-11 | 450.25 |
    +------------+-----------+------------+--------+
    3 row
    
    mysql>  select first_name, last_name, order_date, SUM(amount) as total_spent
        -> from customers
        -> join orders on customers.id = orders.id group by orders.customer_id
        -> order by total_spent desc;
    +------------+-----------+------------+-------------+
    | first_name | last_name | order_date | total_spent |
    +------------+-----------+------------+-------------+
    | David      | Bowie     | 2014-12-12 |      813.17 |
    | Bette      | Davis     | 1999-04-11 |      450.25 |
    | Boy        | George    | 2016-02-10 |      135.49 |
    +------------+-----------+------------+-------------+
    3 rows       
    
    
#### left Join

Pega tudo da primeira tabela, na segunda tabela tras os dados que batem com a chave estrangeira relacionada
 a primeira tabela. No caso do inner join os usuarios sem compras seriam ignorados

        
    mysql>     SELECT first_name, last_name, order_date, amount FROM customers
        ->         LEFT JOIN orders
        ->             ON customers.id = orders.customer_id;
    +------------+-----------+------------+--------+
    | first_name | last_name | order_date | amount |
    +------------+-----------+------------+--------+
    | Boy        | George    | 2016-02-10 |  99.99 |
    | Boy        | George    | 2017-11-11 |  35.50 |
    | George     | Michael   | 2014-12-12 | 800.67 |    
    | George     | Michael   | 2015-01-03 |  12.50 |
    | David      | Bowie     | NULL       |   NULL |
    | Blue       | Steele    | NULL       |   NULL |
    | Bette      | Davis     | 1999-04-11 | 450.25 |
    +------------+-----------+------------+--------+
    7 rows
    
    mysql>     SELECT  
        ->         first_name, 
        ->         last_name,
        ->         SUM(amount) as total_spent
        ->     FROM customers
        ->         LEFT JOIN orders
        ->             ON customers.id = orders.customer_id 
        ->         GROUP BY customers.id;
    +------------+-----------+-------------+
    | first_name | last_name | total_spent |
    +------------+-----------+-------------+
    | Boy        | George    |      135.49 |
    | George     | Michael   |      813.17 |
    | David      | Bowie     |        NULL |
    | Blue       | Steele    |        NULL |
    | Bette      | Davis     |      450.25 |
    +------------+-----------+-------------+
    5 rows
    
    mysql>     SELECT  
        ->         first_name, 
        ->         last_name,
        ->         IFNULL(SUM(amount), 0)
        ->             as total_spent
        ->     FROM customers
        ->         LEFT JOIN orders
        ->             ON customers.id = orders.customer_id 
        ->         GROUP BY customers.id;
    +------------+-----------+-------------+
    | first_name | last_name | total_spent |
    +------------+-----------+-------------+
    | Boy        | George    |      135.49 |
    | George     | Michael   |      813.17 |
    | David      | Bowie     |        0.00 |
    | Blue       | Steele    |        0.00 |
    | Bette      | Davis     |      450.25 |
    +------------+-----------+-------------+
    5 rows 


#### Right Join

Pega tudo da segunda tabela, na primeira tabela tras os dados que batem com a chave estrangeira relacionada
 a segunda tabela. 
 
    mysql>     SELECT *
        ->     FROM customers
        ->         INNER JOIN orders
        ->             ON customers.id = orders.customer_id;
    +----+------------+-----------+------------------+----+------------+--------+-------------+
    | id | first_name | last_name | email            | id | order_date | amount | customer_id |
    +----+------------+-----------+------------------+----+------------+--------+-------------+
    |  1 | Boy        | George    | george@gmail.com |  1 | 2016-02-10 |  99.99 |           1 |
    |  1 | Boy        | George    | george@gmail.com |  2 | 2017-11-11 |  35.50 |           1 |
    |  2 | George     | Michael   | gm@gmail.com     |  3 | 2014-12-12 | 800.67 |           2 |
    |  2 | George     | Michael   | gm@gmail.com     |  4 | 2015-01-03 |  12.50 |           2 |
    |  5 | Bette      | Davis     | bette@aol.com    |  5 | 1999-04-11 | 450.25 |           5 |
    +----+------------+-----------+------------------+----+------------+--------+-------------+
    5 rows
    
    
    mysql>     SELECT *
        ->     FROM customers
        ->         RIGHT JOIN orders
        ->             ON customers.id = orders.customer_id;
    +------+------------+-----------+------------------+----+------------+--------+-------------+
    | id   | first_name | last_name | email            | id | order_date | amount | customer_id |
    +------+------------+-----------+------------------+----+------------+--------+-------------+
    |    1 | Boy        | George    | george@gmail.com |  1 | 2016-02-10 |  99.99 |           1 |
    |    1 | Boy        | George    | george@gmail.com |  2 | 2017-11-11 |  35.50 |           1 |
    |    2 | George     | Michael   | gm@gmail.com     |  3 | 2014-12-12 | 800.67 |           2 |
    |    2 | George     | Michael   | gm@gmail.com     |  4 | 2015-01-03 |  12.50 |           2 |
    |    5 | Bette      | Davis     | bette@aol.com    |  5 | 1999-04-11 | 450.25 |           5 |
    +------+------------+-----------+------------------+----+------------+--------+-------------+  
    
Deletando tabelas com chaves estrangeiras;

    DROP TABLE orders, custumers;
    
Deletamos primeiro a tabela orders pois nela estão as chaves relacionadas a tabelea custumers;
não consigueremos deletar a segunda sem antes deletar a primeira;


#### Deletando em cascata

quando um custumer for deletado suas orders tabém serão

        CREATE TABLE orders(
            id INT AUTO_INCREMENT PRIMARY KEY,
            order_date DATE,
            amount DECIMAL(8,2),
            customer_id INT,
            FOREIGN KEY(customer_id) 
                REFERENCES customers(id)
                ON DELETE CASCADE
        );
        
        
## Many to many

Considere um sistema de avaliação de séries por usuarios onde temos as tabelas

    desc series;
    +---------------+--------------+------+-----+---------+----------------+
    | Field         | Type         | Null | Key | Default | Extra          |
    +---------------+--------------+------+-----+---------+----------------+
    | id            | int(11)      | NO   | PRI | NULL    | auto_increment |
    | title         | varchar(100) | YES  |     | NULL    |                |
    | released_year | year(4)      | YES  |     | NULL    |                |
    | genre         | varchar(100) | YES  |     | NULL    |                |
    +---------------+--------------+------+-----+---------+----------------+
    4 rows 
    
    desc reviewers;
    +------------+--------------+------+-----+---------+----------------+
    | Field      | Type         | Null | Key | Default | Extra          |
    +------------+--------------+------+-----+---------+----------------+
    | id         | int(11)      | NO   | PRI | NULL    | auto_increment |
    | first_name | varchar(100) | YES  |     | NULL    |                |
    | last_name  | varchar(100) | YES  |     | NULL    |                |
    +------------+--------------+------+-----+---------+----------------+
    3 rows
    
    desc reviews;
    +-------------+--------------+------+-----+---------+----------------+
    | Field       | Type         | Null | Key | Default | Extra          |
    +-------------+--------------+------+-----+---------+----------------+
    | id          | int(11)      | NO   | PRI | NULL    | auto_increment |
    | rating      | decimal(2,1) | YES  |     | NULL    |                |
    | series_id   | int(11)      | YES  | MUL | NULL    |                |
    | reviewer_id | int(11)      | YES  | MUL | NULL    |                |
    +-------------+--------------+------+-----+---------+----------------+
    4 rows
    
#### IS NULL

Questão: Pegar todas as séries que não possuem review

    SELECT title AS unreviewed_series
    FROM series
    LEFT JOIN reviews
        ON series.id = reviews.series_id
    WHERE rating IS NULL;
    
    +-----------------------+
    | unreviewd_series      |
    +-----------------------+
    | Malcolm In The Middle |
    | Pushing Daisies       |
    +-----------------------+
    2 rows
    
Questão: Pegar a media de avaliação por genero

    SELECT 
        genre,
        ROUND(avg(rating),2) as avg_rating
    FROM series
    INNER JOIN reviews
        ON series.id = reviews.series_id
    GROUP BY series.genre;
    
    +-----------+------------+
    | genre     | avg_rating |
    +-----------+------------+
    | Animation |    7.86000 |
    | Comedy    |    8.16250 |
    | Drama     |    8.04375 |
    +-----------+------------+
    
Outro exemplo

    SELECT 
        first_name, 
        last_name, 
        count(rating) as 'COUNT', 
        ifnull(min(rating),0) as 'MIN', 
        ifnull(max(rating),0) as 'MAX',
        ifnull(avg(rating),0) as 'AVG',
        CASE WHEN count(rating)>=1 
            THEN 'ACTIVE'
            ELSE 'INACTIVE'
        END AS 'STATUS'
    FROM reviewers
    LEFT JOIN reviews
        ON reviewers.id = reviews.reviewer_id
    GROUP BY reviewers.id;

ou com **IF**

    SELECT 
        first_name, 
        last_name, 
        count(rating) as 'COUNT', 
        ifnull(min(rating),0) as 'MIN', 
        ifnull(max(rating),0) as 'MAX',
        ifnull(avg(rating),0) as 'AVG',
        IF(COUNT(rating) >=1, 'ACTIVE', 'INACTIVE') AS 'STATUS'
    FROM reviewers
    LEFT JOIN reviews
        ON reviewers.id = reviews.reviewer_id
    GROUP BY reviewers.id;
    
 resulta em 
 
    +------------+-----------+-------+-----+-----+---------+----------+
    | first_name | last_name | COUNT | MIN | MAX | AVG     | STATUS   |
    +------------+-----------+-------+-----+-----+---------+----------+
    | Thomas     | Stoneman  |     5 | 7.0 | 9.5 | 8.02000 | ACTIVE   |
    | Wyatt      | Skaggs    |     9 | 5.5 | 9.3 | 7.80000 | ACTIVE   |
    | Kimbra     | Masters   |     9 | 6.8 | 9.0 | 7.98889 | ACTIVE   |
    | Domingo    | Cortes    |    10 | 5.8 | 9.1 | 7.83000 | ACTIVE   |
    | Colt       | Steele    |    10 | 4.5 | 9.9 | 8.77000 | ACTIVE   |
    | Pinkie     | Petit     |     4 | 4.3 | 8.8 | 7.25000 | ACTIVE   |
    | Marlon     | Crafford  |     0 | 0.0 | 0.0 | 0.00000 | INACTIVE |
    +------------+-----------+-------+-----+-----+---------+----------+
    7 rows 
    
####Exemplo com as três tabelas (incluindo a tabela pivo)

    SELECT 
        title,
        rating, 
        CONCAT(first_name, ' ' ,last_name) AS reviewer
    FROM reviewers
    INNER JOIN reviews 
        ON reviewers.id = reviews.reviewer_id
    INNER JOIN series
        ON series.id = reviews.series_id
    ORDER BY title 
    LIMIT 15;
    
    +----------------------+--------+-----------------+
    | title                | rating | reviewer        |
    +----------------------+--------+-----------------+
    | Archer               |    8.0 | Thomas Stoneman |
    | Archer               |    7.7 | Domingo Cortes  |
    | Archer               |    8.5 | Kimbra Masters  |
    | Archer               |    7.5 | Wyatt Skaggs    |
    | Archer               |    8.9 | Colt Steele     |
    | Arrested Development |    8.1 | Thomas Stoneman |
    | Arrested Development |    6.0 | Domingo Cortes  |
    | Arrested Development |    8.0 | Kimbra Masters  |
    | Arrested Development |    8.4 | Pinkie Petit    |
    | Arrested Development |    9.9 | Colt Steele     |
    | Bob's Burgers        |    7.0 | Thomas Stoneman |
    | Bob's Burgers        |    8.0 | Domingo Cortes  |
    | Bob's Burgers        |    7.1 | Kimbra Masters  |
    | Bob's Burgers        |    7.5 | Pinkie Petit    |
    | Bob's Burgers        |    8.0 | Colt Steele     |
    +----------------------+--------+-----------------+
    15 rows
    
##TRIGGERS
    
Roda algum código sql quando um evento é disparado

sintax

    CREATE TRIGGER trigger_name
        trigger_time trigger_event ON table_name FOR EACH ROW
        BEGIN  
            ...
        END;
        
        

    
**trigger_time**: BEFORE ou AFTER

**trigger_event**: INSERT, UPDATE, DELETE    

Pode ser usado para validar dados (não é um bom uso);

Ex.: Previne usuario de ser criado se a idade for menor que 18

    DELIMITER $$
    
    CREATE TRIGGER must_be_adult
         BEFORE INSERT ON users FOR EACH ROW
         BEGIN
              IF NEW.age < 18
              THEN
                  SIGNAL SQLSTATE '45000'
                        SET MESSAGE_TEXT = 'Must be an adult!';
              END IF;
         END;
    $$
    
    DELIMITER ;

Nota 45000 representa um erro genérico definido pelo usuário ( uma condição nativa não lançaria este erro).
Sobre o $$ ele é usado como delimitador caso o contrario o sql executaria no primeiro ponto e virgula que encontrasse.
    
    DELIMITER $$
    
    CREATE TRIGGER example_cannot_follow_self
         BEFORE INSERT ON follows FOR EACH ROW
         BEGIN
              IF NEW.follower_id = NEW.following_id
              THEN
                   SIGNAL SQLSTATE '45000'
                        SET MESSAGE_TEXT = 'Cannot follow yourself, silly';
              END IF;
         END;
    $$
    
    DELIMITER ;
    
#### Trigger de Log

Provavelmente o uso mais comum de triggers

        DELIMITER $$
        
        CREATE TRIGGER capture_unfollow
            AFTER DELETE ON follows FOR EACH ROW 
        BEGIN
            INSERT INTO unfollows
            SET follower_id = OLD.follower_id,
                followee_id = OLD.followee_id;
        END$$
        
        DELIMITER ;
        
Neste exemplo poderia ser usado o insert tradicional

    SHOW TRIGGERS;
    DROP TRIGGER trigger_name;
    
NOTA: Usar triggers tornam o debug da aplicação dificil.
    
    
    





 









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

	select uf, coun(*) from clients group by uf;
	
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
	








 









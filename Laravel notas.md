##Criando novo projeto laravel

    composer create-project laravel/laravel NOME_PROJETO

Rodando o Servido e hambiente Laravel

    php artisan serve
    //ou
    php -S localhost:8000 -t public public/index.php
    

IDE Helper

	$ composer require --dev barryvdh/laravel-ide-helper

Adicionar o serviço view Helper em Providers/AppServiceProvider.php
	
	public function register()
	{
    if ($this->app->environment() !== 'production') {
        $this->app->register(\Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider::class);
    }
    // ...
	}

Gere a documentação

	php artisan clear-compiled
	php artisan ide-helper:generate
	

Requitos do laravel

	php -m //Para verificar se as extensões abaixo estão ativas
	
	openssl
	PDO
	mbstring
	tokenizer
	
## php artisan

Gerar Um Controller

	php artisan make:controller NomeController
	
Gerar um Model

	php artisan make:model NomeModel
	
Para gerar o Model e seu migração basta adicionar o -m no final

	php artisan make:model NomeModel -m
	
##Tiker e Models

Iniciar o Tinker

	php artisan tinker
	
Exemplo de uso de um Model no Tinker

	$user = App\User::create(['name'=> 'beltrano', 'email' => 'cicrano@email.com', 'password'=>bcrypt('mypassword')])
	
No tinker então você pode acessar os dados do usuário

	$user->name;
	
Todos os Usuários

	$all = App\User::all()
	
Usuario por Id 

	$user = App\User::find(2)
	
Editando o Usuário

	$user->update(['name'=>'Zezinho'])

Excluindo o Usuário

	$user->delete()	
	
##Migrations
	
Migrations - no exemplo abaixo o 'create_products_table' é o nome do arquivo de migration e o 'products' é o nome da tabela, apenas o arquivo é gerado o banco de dados não é tocado
	
	php artisan make:migration create_products_table --create=products
	
Para colocar as as migrations no bd executar o comando

	php artisan migrate	
    
Para removermos as tabelas do bando de dados executa o comnado

	php artisan migrate:reset
	
Podemos criar uma migration de atualização

	php artisan make:migration update_products_table --table=products
	
Revertendo uma migration

	php artisan migrate:rollback

##Laravel Installer
	
Laravel Installer Globalmente

	composer global require "laravel/installer"
	
Criar uma nova aplicação laravel

	laravel new PASTADOPROJETO
	//ou
	laravel new --5.2 PASTADOPROJETO
	
Visualizar comandos do laravel installer

	larave --help
	
##Convenções

Se uma Model tem nome de user no banco de dados sua tabela devera estar no plural com o nome de users.


	
##Geral

Traduzir um arquivos (mensagens de erro de formulario e outros)
	
	resources/lang
	https://github.com/caouecs/Laravel-lang
	
A variavel $_ENV é global no laravel, e contem um array com dados do arquivo .env este arquivo não dev estar no controle de versão

Mudamos o mode de desenvolvimento e de produção no arquivo .env

Modo de desenvolvimento no arquivo .env

	APP_ENV=local
	APP_DEBUG=true
	
Modo de produção no arquivo .env

	APP_ENV=prod
	APP_DEBUG=false
	
Modo de manutenção

	php artisan down
	
Saindo do modo de manutenção

	php artisan up
	
Ao alterar as variaveis de ambiente é necessário reiniciar o artisan serve


Função env() é usada para variáveis de ambiente, as configurações são feitas no arquivo .env

#### Tinker DB::

Inserindo registro ex.:

	DB::insert('insert into users (name, email, password) values (?,?,?)', ['Fulano', 'email@email.com', bcrypt('123456')])
	
	//ou 
	
	DB::table('users')->insert(['name'=> 'Fulano', 'email' => 'email@email.com', 'password'=> bcrypt(123456) ])
	
Selecionando um Registro ex.:

	DB::select('select * from users where id = :id', ['id' => 1])

Atualizando um registro ex.:

	DB::update('update users set name = :name where id = :id', ['name' => 'ciclano', 'id'=>1])
	
Deletando um registro

	DB::delete('delete from users where id = :id', ['id'=>1])
	
Query genéricas usa-se statement

	DB::statement('drop table users')
	
Podemos usar mais de uma conecção, imaginenemos que no arquivo de configuração esteja como padrão o mysql mas também temos uma configuração sqlite, podemos usala passando a flag

	DB::connection('sqlite')->statement...
	DB::connection('sqlite')->delete...
	DB::connection('sqlite')->update...
	DB::connection('sqlite')->select...
	DB::connection('sqlite')->inserte...
	
##QueryBuilder
	
Query Builder é uma abstração de colsultas ao banco de dados ( sql ) no exempo abaixo iserimos dados na tabela user.

	php artisan tinker

	DB::table('users')->insert(['name' => 'Fulano', 'email' => 'exemplo@email.com', 'password' => bcrypt('mypassword')]) 
	
Com este comando pegados a lista de todos os usuários

	DB::table('users')->get()

Podemos usar o where

	//Com get Retorna uma lista
	DB::table('users')->where('id', 1)->get()
	
	//Com o first retorna um único valor
	DB::table('users')->where('id', 1)->first()
	
Outra form de usar o where

	DB::table('users')->where('id', '=', '1')->get()
	DB::table('users')->where('id', '<', '1')->get()
	DB::table('users')->where('id', '>', '1')->get()
	
Where multiplo, cada valor retornado deve satisfazer a ambas as listas.

	DB::table('users')->where([ ['id', '=', '1'], ['email', '=', 'email@email.com'] ])->get()
	
Usando o orwhere, um resultado precisa satisfazer apenas um where

	DB::table('users')->where('id', 4)->orwhere('id', 1)->get()
	
Usando o whereBetween, retorna todos os registros em um intervalo

	DB::table('users')->whereBetween('id', [2,3])->get();
	
Usando o whereNotBetween, retorna todos os registros exceto os do intervalo

	DB::table('users')->whereNotBetween('id', [2,3])->get();
	
Usando o whereIn, retorna os registros da lista

	DB::table('users')->whereIn('id', [1,2,3])->get();
	
Usando o whereNotIn, retorna todos os registros que não estão na lista
	
	DB::table('users')->whereNotIn('id', [1,3] )->get()


Where Null, verificar nulos, pega todos os campus onde o campo é nulo.

	DB::table('users')->whereNull('remenber_token')->get()
	
Where Not Null, verificar nulos, pega todos os campus onde o campo não é nulo.

	DB::table('users')->whereNotNull('remenber_token')->get()
	
Usando o linke

	DB::table('users')->where('name', 'like', 'fu%')->get()
	
Atualizando os Dados

	DB::table('users')->where('id', 1)->update(['name' => 'Ciclano'])
	
Removendo Registro

	DB::table('users')->where('id', 1)->delete()
	
	//Exclui todos os registros
	DB::table('users')->delete()
	
	//Truncar tablea (exclui todos registros)
	DB::table('users')->truncate()
	
Inserindo Multiplos Registros

	DB::table('users')->insert([
		['name'=> 'A', 'email' => 'a@a.com', 'password'=>bcrypt('123')],
		['name'=> 'B', 'email' => 'b@b.com', 'password'=>bcrypt('123')] 
	])
	
Usando agredadores

	DB::table('users')->count()
	DB::table('users')->max()
	DB::table('users')->min()
	DB::table('users')->avg('nome_da_tabela')
	
Pegando todos os dados de uma coluna 
	
	DB::table('users')->select('email')->get()
	
Usando um alias

	DB::table('users')->select('email as user_email')->get()
	
Ordenando 

	DB::table('users')->orderBy('id', 'desc')->get()
	DB::table('users')->orderBy('name', 'asc')->get()
	
Pegando só os dois primeiros registrios

	DB::table('users')->orderBy('name', 'asc')->take(2)->get()

Pegando só dois registros mas pulando o primeiro (util para paginação)

	DB::table('users')->orderBy('name', 'asc')->take(2)->skip(1)->get()
	
Exemplo de DB::raw - usa uma função do sql
	
	DB::table('users')->select(DB::raw('count(*) as quantity'))->get()
	
#### Schema builder

Criando uma Tabela

	Schema::create('users', function ($table) {
		$table->increments('id');
		$table->string('name');
	})
	
Adicionando colunas a uma tabela

	Schema::table('users', function ($table) {
		$table->string('email', 100)->unique(); 
		$table->smallInteger('level')->nullable()->default(1)->unsigned()->after('name');
	});
	
Para alterar uma coluna precisamos instalar o doctrine / dbal para usar o methodo change

	composer require doctrine/dbal

Agora podemos fazer o seguinte, o campo preservara o que não for sobreescrito, ele continuara sendo unico por exemplo,

	Schema::table('users', function($table){
		$table->string('email')->nullable()->change();
	})
	
Apagando uma coluna e renomeando outra

	Schema::table('users', function($table) {
		$table->renameColumn('name', 'username');
		$table->dropColumn('level');
	})
	
Renomeando uma tabela
	
	Schema::table('users', function($table) {
		$table->renameColumn('name', 'username');
		$table->dropColumn('level');
	})
	
Apagando uma tabela

	Schema::drop('accounts')
	
#### Indices e chaves estrangeiras

Tabela Pivo = contem dois indices de chaves primarias. Geralmente uma chave composta

	Schema::create('table_pivo', function ($table) {
		$table->primary(['tablea_id', 'tableb_id']);
	})

Chave Unica Composta, a e b podem se repetir individualmente mas não  compostamente

	$table->unique(['a', 'b'])
	
Definindo index

	$table->index('colunadoIndex')

Exemplo de criação de tabela com chave estrangeira

	Schema::create('posts', function ($table) {
		$table->increments('id');
		$table->string('slug');
		$table->integer('user_id')->unsigned(); 
		$table->unique('slug');
		$table->foreign('user_id')->references('id')->on('users')->onUpdate('cascade')->onDelete('cascade');
	})
	
	
------

exemplo de where compostos, procura primeiro o id 3, caso não encontre procura onde o created at é null.

	DB::table('posts')->where('id', 3)->orWhere(function ($query){ $query->whereNull('created_at');})->get()


Usando o join

	DB::table('users')->join('posts', 'users.id', '=', 'posts.user_id')->get()
	
Usando o join com select

	DB::table('users')->join('posts', 'users.id', '=', 'posts.user_id')->select('users.name','posts.title')->get()

Multiplos Joins

	DB::table('users')->join('posts', 'users.id', '=', 'posts.user_id')->select('users.name','posts.title')->join('comments', 'users.id', '=', 'comments.user_id')->get()

Left Join

	DB::table('users')->leftJoin('posts', 'users.id', '=', 'posts.user_id')->get()

right Join

	DB::table('users')->rightJoin('posts', 'users.id', '=', 'posts.user_id')->get()

























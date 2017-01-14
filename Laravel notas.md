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
	
Atualizando os Dados

	DB::table('users')->where('id', 1)->update(['name' => 'Ciclano'])
	
Removendo Registro

	DB::table('users')->where('id', 1)->delete()

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

	


























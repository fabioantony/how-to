Criando novo projeto laravel

    $ composer create-project laravel/laravel NOME_PROJETO

Rodando o Servido e hambiente Laravel

    $ php artisan serve

Função env() é usada para variáveis de ambiente, as configurações são feitas no arquivo .env

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


    

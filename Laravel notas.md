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
	
Union (Resultados Distintos)

    $first = DB::table('posts')->whereNull('created_at')
    DB::table('posts')->whereNull('updated_at')->union($first)->get()
    
Union All (Resultados NÃO Distintos)

    $first = DB::table('posts')->whereNull('created_at')
    DB::table('posts')->whereNull('updated_at')->unionAll($first)->get()
    
#### Join Avançado

Join e function

    DB::table('users')->join('posts', function($join){ 
        $join->on('users.id', '=', 'posts.user_id')->whereNull('posts.created_at');
    })->select('users.name', 'posts.title')->get()
    
    
    DB::table('users')->join('posts', function($join){
        $join->on('users.id', '=', 'posts.user_id')->whereNotNull('posts.created_at');
    })->select('users.name', 'posts.title')->get()
    
#### Seeding

Criar seeding

    php artisan make:seeder NomeDoSeeder
    
Registrar a Classe Seeder no metodo run em 
    
    database/seeds/DatabaseSeeder.php

Rodar no Terminal o Composer dump-autoload
    
    composer dump-autoload
    
Executando seed

    php artisan db:seed
    
Rodando um seeder especifico

    php artisan db:seed --class=NomeDoSeeder
    
Remove as Tabelas, Cria as Tabelas e executa o seed

    php artisan migrate:refresh --seed
    
#### Operações basicas
 
Criando um Moldel

    php artisan make:model NomeDoModel
    
Criando um Model e junto sua Migration

    php artisan make:model NomeDoModel --migration
    
# Active Record

## Fillable

Todos os campos que serão inseridos ou editados atres de metodos como create ou update precisan estar na variavel fillable do ActiveRecord isso é uma proteção contra 'mass assignment'
    
    protected $fillable = ['title', 'body'];   

## Guarded
   
Funciona de maneira inversa ao fillable neste parametro do active record registramos os campus que não podem ser alterados.
    
    protected $guarded = ['id', 'created_at', 'updated_at']
    
Importante use fillable ou guarded nunca os dois juntos.
        
Criando um active record 
   
    $post = new App\Post;
    $post->title = 'First Post'
    $post->body = 'This is my first post'
    $post->save //Return bool
    $post // agora é os dados do post
    
Listando todos os registro do bd de um active record

    App\Post::all()
    
Where No Active Recorde
        
    // Com get Retorna todos os registros    
    App\Post::where('type', 'text')->get()
     
    // Com first Retorna somente o primeiro registro no caso a primeira linha
    App\Post::where('type', 'text')->first() 
    
    // Com first e o order by podemos retornar apenas o ultimo registro
    App\Post::where('type', 'text')->orderBy('created_at', 'desc')->first() 
    
Pegar Um registro atraves de um id

    App\Post::find(1)
    
Editando um registro

    $first = App\Post::find(1);
    $first->title = 'First Post Edited';
    $first->save();
    
Deletando um Registro
    
    $first = App\Post::find(1);
    $first->delete();
    
Retornando exception caso um registro não exista

    App\Post::find(1) // retorna null
    App\Post::findOrFail(1) // lança uma exception
    
O Methodo Create pode inserir uma linha no banco

	$user = App\User::create([
	    'name'=> 'beltrano',
	    'email' => 'cicrano@email.com',
	    'password'=>bcrypt('mypassword')
	])
	
# Convenções dos Models

- Não precisamos digitar o nome da tabela do model
- Considera o Nome da chave primaria como id
- As classes tem nome CamelCase e tem seu nome equivalente na tabela de bancos de dados convertidos para nome_models com o s no final para indicar plural

## Mudando as Convenções

Podemos estabelecer o nome da tabela como a propridade abaixo

    protected $table = 'nome_da_tabela'
	
Podemos estabelecer o nome da Chave Primaria com a propriedade abaixo

    protected $primaryKey = 'primary_key_id'
    
Caso você não use o created_at e o updated_at é bom desativar os timestamps() com a propriedade abaixo ou o laravel ira continuar procurando por eles

    public $timestamps =  false
    
Mudando a coneção padrão para um model especifico
 
    protected $connection = 'sqlite';
    
Usando um object Carbon onde existe um timestamp para trabalhar melhor com datas
 
    protected $dates = ['created_at', 'updated_at'] ;
    
## Accessors    

Accessors são funções que modificam as informações do banco de dados antes de apresenta-las ao usuário    
    
## Muttators    

São funções que modificam as informações antes de serem inseridas no banco de dados

### Definando ambos

    public function setTitleAttribute($value)
    {
        $this->attributes['title'] = strtolower($value);
    }

    public function getTitleAttribute($value)
    {
        return ucfirst($value);
    }
        
### Casting

Retorna os dados do banco de dado não como string mas como o tipo que você estabelecer

    protected $casts = [
        'is_complete' => 'boolean',
        'age' => 'integer',
        'price' => 'double',
        'created_at' => 'datetime',
        'birthday' => 'date',
        'name' => 'string',
    ];
    
### Scoping

Facilita consultas longas que são muita repetidas no sistema

    public function scopeText($quey){
        $query->where('type', 'text');
    }
    
usando o Scope

    App\Post::text()->get()

Outro ex.

    public function scopeOfText($quey, $type){
        $query->where('type', $type);
    }
    
usando o Scope

    App\Post::ofType('text')->get()
    
#### Scoping Global

O scope global aplicara implicidamente este filtro a qualquer consulta, Em um model

    use Illuminate\Database\Eloquent\Builder;
    use Carbon\Carbon;
    ...
    
    protected static function boot()
    {
        parent::boot();
        static::addGlobalScope('published', function (Builder $builder){
            $builder->where('published_at', '<', Carbon::now()->format('Y-m-d H:i:s'));
        })
    }
    
    
Então para realizar uma consulta sem que este scope seja aplicado

    App\Post::withoutGlobalScope('published')->get()
    
Se ouver mais de um scopo use um array

    App\Post::withoutGlobalScope(['published', 'another_scope'])->get()
    

## Soft Deleting

Na migration colocar o seguinte campo
        
    $table->softDeletes();
    
No model 

    use Illuminate\Database\Eloquent\SoftDeletes;
    
    ...
    
    protected $dates = ['created_at', 'udpated_at', 'deleted_at'];

Pegando todos os registros mesmo os excluidos
    
    App\Post::withTrashed()->get()
    
Pegando somente registros excluidos

    App\Post::onlyTrashed()->get()
    
Verificando se um registro foi excluido

    $post->trashed(); // retorna um boolean

Recuperando Registro excluido

    //instanciamos o registro
    $post = App\Post::withTrashed()->find(1)
        
    //recuperamos o registro
    $post->restore;
    
### Relações entre tabelas

#### One to One

Na classe que possui uma outra classa em ralação

    public function possui()
    {
        return $this->hasOne(ModelPossuido::class, 'user_id') //user_id opcional
    }
    
Na classe que é possuida por outra classe

    public function possuidaPor()
    {
        return $this->belongsTo(Possui::class, 'user_id') //user_id opcional
    }
    
Exemplo de Uso
    
    $possuida = $possui->possuida //Pode acessar como se fosse uma propriedade
    
    //agora posso udar a possuida como um objeto normal do laravel chamando os metodos do model
    
    $possuida->delete()

#### One to many

Classe que possui muitos

    possuiMuitos()
    {
        return $this->hasMany(Possuida::class)
    }

Classe possuida por muitos

    public function possuida()
    {
        return $this-belongsTo(PossuiMuitos::class)
    }
    
Associando um registro existente

    $possuida->possuida()->associate($possuiMuitas)


#### Many to many

Neste tipo de relação é necessario uma tabela pivo. Esta Tabela não precisa de um model

Um post possui muitas tags, um tag possui muitos posts.

Exemplo de tabela pivo a convenção para o nome é no singular e em ordem alfabetica

    class CreatePostTagTable extends Migration
    {
        Schema::create('post_tag', function (Blueprint $table) {
            $table->integer('post_id')->unsigned();
            $table->integer('tag_id')->unsigned();
            
            $table->primary(['post_id', 'tag_id']);
            
            $table->foreign('post_id')->references('id')->on('posts')
                ->onDelete('cascade')->onUpdate('cascade');
            
            $table->foreign('tag_id')->references('id')->on('tags')
                ->onDelete('cascade')->onUpdate('cascade');
        });
    }

Exemplo de ralação no Model 
    
    class Post exetends Model
    {
        ...
    
        public function tags()
        {
            //os tres ultimos parametros poderiam ser omitidos pois seguem a convenção de nomeação
            return $this->belongsToMany(Tag::class, 'post_tag', 'post_id', 'tag_id') 
        }
      
    }
    
    class Tag extends Model
    {
        ...
        
        public function posts()
        {
            return $this->belongsToMany(Post::class);
        }
    }
        
Atribuindo Tags ao Post

    $post->tags()->attach([1, 2]) //Dentro do Attach a lista com os ids das tags
    

Atribuindo Posts a task

    $tag->posts()->attach([1, 2]) //Dentro do Attach a lista com os ids dos posts
    
RemovendoTags ao Post

    $post->tags()->detach([1, 2]) //Dentro do detach a lista com os ids das tags

Sync remove todos os vinculos anteriores e deixam apenas os que você estabelecer
         
    $post->tags()->sync([2]) //agora post tera apenas a tag 2
    
Timestamps na tabela pivo

    // depois de adicionar a coluna na migration
    public function tags()
    {
        return $this->belongsToMany(Tag::class)->withTimestamps()
    }
    
Caso a tabela pivo tenha alguma outra coluna

        // depois de adicionar a coluna na migration
        public function tags()
        {
            return $this->belongsToMany(Tag::class)->withPivot('colun1', 'colun2')
        }
        
Inserindo valores nos campos customizados

        //em modelo funciona com sync attach e outros 
        $post->tags()->sync([1 => ['colum1' => 'foo', 'colum2'=>'bar'], 2 => ['colum1' => 'foo', 'colum2'=>'bar']])
    

### campo nulo em uma migration

    Schema::create ('posts', function (Blueprint $table) {
        $table->integer('user_id')->unsigned()->nullable();
    }
    
### unsigned

Todos os tipos inteiros podem ter um atributo opcional (não padronizado) UNSIGNED. Tipo não assinado pode ser usado para permitir apenas números não negativos em uma coluna ou quando você precisa de um intervalo numérico superior maior para a coluna. Por exemplo, se uma coluna INT for UNSIGNED, o tamanho do intervalo da coluna é o mesmo, mas seus pontos de extremidade mudam de -2147483648 e 2147483647 para 0 e 4294967295.
Quando devo usá-lo?

Pergunte a si mesmo esta pergunta: Será que este campo sempre contém um valor negativo? 
Se a resposta é não, então você quer um UNSIGNED tipo de dados.

Um erro comum é usar uma chave primária que é um auto-incremento INT partir de zero, mas o tipo é SIGNED , nesse caso, você nunca mais vai tocar em nenhum dos números negativos e você está reduzindo o leque de possíveis id para metade .


### Polymorphic

Vamos supor que classes de tipos diferentes mantem um contrato em comun, 
Neste exemplo temos um Model Like (e sua tabela) que representa o contrato likeable.
A relação é uma classe qualquer pode ter muitos likes.

A migration:

    class CreateLikesTable extends Migration
    {
        public function up()
        {
            Schema::create('likes', function (Blueprint $table){
                $table->increments('id');
                $table->morphs('likeable'); //criara dua colunas likeable_id e likeable_table
                $table->timestamps(); 
            });
        }
    }
    
O Model
    
    class Like extends Model
    {
        public function likeable()
        {
            return $this->morphTo();
        }
    }
    
Os models que podem receber likes deve implementar uma função como a de baixo
    
    public function likes
    {
        $this->morphMany(Like::class, 'likeable')
    }
    
Para atribuir likes funciona da mesma forma que relação de um para muitos
 
    $model->likes()->create([])


### Polymorphic many to many

Neste exemplo voltamos ao exemplo das tags, existem muitas classes de tipos diferentes que podem uma ou mais tags, e um tags pode pertencer a diferentes classes e tipos

Vamos precisar de uma tabela pivo chamada taggable

    class CreateTaggablesTable extends Migration
    {
        public function up()
        {
            Schema::create('taggable', function(Blueprint $table) {
                $table->integer('tag_id')->unsigned();
                $table->morphs('taggable');
            });
        }
    }
    
Não precisamos de um model para a tabela pivo, mas ainda precisamos de um model e tabela tag como exemplo many to many, vinculamos o taggable a um model de seguinte maneira

    public function tags() {
        return $this-morphToMany(Tag::class, 'taggable');
    }

No model tag todos as classes taggables precisam de sua própria função

    class Tag extends Model
    {
        protected $fillable = ['name'];
        public $timestamps = false;
        
        public function texts() 
        {
            return $this->morphedByMany(Text::class, 'taggable');
        }
        
        public function images() 
        {
            return $this->morphedByMany(Image::class, 'taggable');
        }
    }

Obtendo tags

    $text->tags()->sync([1, 2])
    $tag->texts



#[Laravel - Eloquent](https://www.schoolofnet.com/curso-laravel-eloquent/)

## Operações Básicas

criando um model e sua seed ao mesmo tempo

    php artisan make:model Nome --migration

## Mass assigned

para usarmos o metodo create os parametros precisam estar registrados no model na propriedade fillable ou na lista de exclusão guarded.

## Convenções

Model em CamelCase tabela em snake_case

    protected $table = 'nome_da_tabela';

Chave primária sera 'id'

    protected $primaryKey = 'nome_id';

Todos os model tem timestamps

    public $timestamps = false;
    
Tabela fora da coneccção padrão

    protected $connection = 'sqlite';
    
Carbon nos campos data

    protected $dates = ['created_at', 'updated_at', 'published_at'];
    
## Mutators

Nos models  podem ser inseridos os metodos de acesso de um atributo

tratando um atributo que sera inserido no bd

    public function setTitleAttribute($value) 
    {
        $this->attributes['title'] = strtolower($value);
    }
    
tratando um valor pego do bd

    public function gatTitleAttribute($value) 
    {
        return ucfirst($value);
    }

tipos de valores ou attribute casting

    protected $casts = [
        'is_true' => 'boolean',
        'created_at' => 'datetime',
    ]
    
## Scopes    

você pode criar funções dentro dos models para executar consultas especificas

    public function scopeText($query) {
        return $query->where('type', 'text')
    }
    
Agora podemos executar a query chamando o metodos do model

    App\Model::text()->get()
    
Também podemos usar parametros 

    public function scopeText($query, $type) {
        return $query->where($type, 'text')
    }
    ...
   
    App\Model::text()->get('text')
   
Scopo global
    
    protected static function boot()
    {
        parent::boot()
        
        static::addGlobalScope('published', function (Builder $builder){
            $builder->where(...);
        });
    }
    
    agora qualquer consulta tem retornara de acordo o scope
        
        
# [Laravel Eventos e filas](/var/www/curso-ministrado-pela-school-of-net-banco-de-dados-e-eloquent)

## Criando e disparando eventos simples

Os eventos são registrados no metodo boot arquivo

    app/Providers/EventServiceProvider
    
# Erros comuns

 [Illuminate\Database\QueryException]                                                                                     
  SQLSTATE[42000]: Syntax error or access violation: 1071 Specified key was too long; max key length is 767 bytes (SQL: a  
  lter table `users` add unique `users_email_unique`(`email`))     


Index Lengths & MySQL / MariaDB

Laravel uses the utf8mb4 character set by default, which includes support for storing "emojis" in the database. If you are running a version of MySQL older than the 5.7.7 release or MariaDB older than the 10.2.2 release, you may need to manually configure the default string length generated by migrations in order for MySQL to create indexes for them. You may configure this by calling the Schema::defaultStringLength method within your AppServiceProvider:

    use Illuminate\Support\Facades\Schema;

    public function boot()
    {
        Schema::defaultStringLength(191);
    }

Alternatively, you may enable the innodb_large_prefix option for your database. Refer to your database's documentation for instructions on how to properly enable this option.

























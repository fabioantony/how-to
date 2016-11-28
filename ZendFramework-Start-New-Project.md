## ZendFrameWork - Starting a New Project with Skeleton Application

*Commands basead on OS Fedora 24 with Apache and PHP*

*Replace PROJECT_NAME always same string, recomended use StudlyCaps*

*********************************************************

Configure Apache vhosts

```bash
 
 $ sudo vim /etc/httpd/conf.d/PROJECT_NAME.conf
 
```
 
```
   
<VirtualHost *:80>
    ServerName PROJECT_NAME.localhost
    DocumentRoot /var/www/PROJECT_NAME/public
    SetEnv APPLICATION_ENV "development"
    <Directory /var/www/PROJECT_NAME/public>
        DirectoryIndex index.php
        AllowOverride All
        Order allow,deny
        Allow from all
    </Directory>
</VirtualHost>

```
    
********************************************

Mapping the address by the local ip

```bash

$ sudo vim /etc/hosts

```

Add this line

```

127.0.0.1    PROJECT_NAME.localhost    localhost

```

********************************************

Get Zend Framework Skeleton

Create Project without questions
  
```bash

$ composer create-project -n -sdev zendframework/skeleton-application /var/www/PROJECT_NAME

```

Create Project with questions

```bash

$ composer create-project -n -sdev zendframework/skeleton-application /var/www/PROJECT_NAME

```


********************************************

Restart Apache

```bash

$ sudo systemctl restart httpd

```

********************************************

Access in your browser


```bash

PROJECT_NAME.localhost/

#must be a zfskeleton theme

PROJECT_NAME.localhost/whatever

#cause an error 404. This error must have the skeleton theme, otherwise the rewrite mode is not working

```

********************************************

Create the database and the user of the project.

```bash

$ mysql -u root -p
#enter root password

#don't forget the ; on the final!
> CREATE DATABASE PROJECT_NAME; 
> CREATE DATABASE PROJECT_NAMETests;
> CREATE USER 'PROJECT_NAMEUser'@'localhost' IDENTIFIED BY 'PROJECT_NAMEPassword';
> GRANT all ON PROJECT_NAME.* TO 'PROJECT_NAMEUser' IDENTIFIED BY 'PROJECT_NAMEPassword' WITH GRANT OPTION;
> GRANT all ON PROJECT_NAMETests.* TO 'PROJECT_NAMEUser' IDENTIFIED BY 'PROJECT_NAMEPassword' WITH GRANT OPTION;

```

Or

```bash

$ touch /var/www/PROJECT_NAME/data/PROJECT_NAMEDatabase.db

```

********************************************

Initialize Git Repository

```bash

$ cd /var/www/PROJECT_NAME/
$ git init
$ git status
$ git remote add origin https://github.com/VENDOR/REPOSITORY.git
$ git add .
$ git commit -m='First Commit'
$ git push -u origin master

```

********************************************

SELinux permission

```bash

$ sudo /sbin/restorecon -R /var/www/PROJECT_NAME
#It may be necessary to repeat this command at different stages of development. Take note or disable SELinux

```

********************************************

Edit file public/index.php and add:

```

    /**
     * Display all errors when APPLICATION_ENV is development.
    */
    if ($_SERVER['APPLICATION_ENV'] == 'development') {
        error_reporting(E_ALL);
        ini_set("display_errors", 1);
    }
    
```
    
********************************************

Register modules for autoload in composer.json

```json

    "autoload": {
        "psr-4": {
            "Application\\": "module/Application/src/",
            "PROJECT_NAME\\": "module/PROJECT_NAME"
        }
    },


```

run command

```bash

$ composer dump-autoload

```
********************************************

Register your module in config/modules.config.php

```bash
return [
    #...
    'PROJECT_NAME',
];
```

********************************************

##Config Database

********************************************

File config/autoload/local.php (or global)

Ex.: 

```php

return [
    'db' => [
        'driver' => 'Pdo',
        'dsn' => sprintf('sqlite:%s/data/PROJECT_NAMEDatabase.db', realpath(getcwd()))
    ]
];

```

Ex-2.:

```php

 return [
     'db' => [
         'driver'         => 'Pdo',
         'username' => 'PROJECT_NAMEUser',
         'password' => 'PROJECT_NAMEPassword',
         'dsn'            => 'mysql:dbname=PROJECT_NAME;host=localhost',
         'driver_options' => [
             PDO::MYSQL_ATTR_INIT_COMMAND => 'SET NAMES \'UTF8\''
         ],
    ],
];
```

********************************************

##Doctrine Configuration

********************************************

Install Doctrine (optional)

```bash

$ cd /var/www/PROJECT_NAME
$ composer require doctrine/doctrine-orm-module
$ mkdir -p /var/www/PROJECT_NAME/data/DoctrineORMModule/Proxy/
$ chmod -R 755  /var/www/PROJECT_NAME/data/DoctrineORMModule/Proxy/

```

********************************************

Register the doctrine module config/modules.config.php

```php

    return [
        'modules' => [
            'DoctrineModule',
            'DoctrineORMModule',
            # ...
        ],
        
```

********************************************

create file config/autoload/doctrine_orm.local.php

```php

<?php
return [
    'doctrine' => [
        'connection' => [
            'orm_default' => [
                'driverClass' => 'Doctrine\DBAL\Driver\PDOMySql\Driver',
                'params' => [
                    'host' => 'localhost',
                    'port' => '3306',
                    'user' => 'PROJECT_NAMEUser',
                    'password' => 'PROJECT_NAMEPassword',
                    'dbname' => 'PROJECT_NAME',
                    'driverOptions' => [
                        PDO::MYSQL_ATTR_INIT_COMMAND => "SET NAMES 'UTF8'"
                    ]
                ]
            ]
        ]
    ]
];

```


********************************************

Test conection

```bash

#Run the command, If no error occurs, the connection to the database was successful
$ /var/www/PROJECT_NAME/vendor/bin/doctrine-module

```

********************************************

Register Doctrine with service in config/module.config.php

```php

'doctrine' => [
    'driver' => [
        __NAMESPACE__ . '_driver' => [
            'class' => 'Doctrine\ORM\Mapping\Driver\AnnotationDriver',
            'cache' => 'array',
            'paths' => [__DIR__ . '/../src/' . __NAMESPACE__ . '/Entity']
        ],
        'orm_default' => [
            'drivers' => [
                __NAMESPACE__ . '\Entity' => __NAMESPACE__ . '_driver'
            ]
        ]
    ]
]

```

********************************************

## Module.php

********************************************

module/PROJECT_NAME/src/Module.php

Ex1:

```php
<?php
class Module
{
    public function getConfig()
    {
        return include __DIR__ . "/../config/module.config.php";
    }

    /*
     * Conceito de Serviços
     *
     * Criamos um serviço para chama-lo de qualquer local da aplicação
     * sem a necessidade de ficar instanciando os objetos manualmente
     * usa o padrão de factories ( factory é design pattern criacional)
     *
     * O Objetivo é ter acesso a serviços em locais como dentro do controller
     */
    public function getServiceConfig()
    {
        return [
            'factories' => [
                // $conteiner tem acesso a todos os serviços
                Model\PostTable::class => function ($container) {
                    $tableGateway = $container->get(Model\PostTableGateway::class);
                    return new Model\PostTable($tableGateway);
                },
                // AdapterInterface - Vem do ZendDb, pegua a key 'db' do arquivo autoload/global e gera o adapter
                Model\PostTableGateway::class => function ($container) {
                    $dbAdapter = $container->get(AdapterInterface::class);
                    // Todos os resultados do dbAdapter são do typo ResultSet
                    $resultSetPrototype = new ResultSet();
                    //O result Set pode ser modelado para a saida desejada, no caso no formato da entidade post
                    $resultSetPrototype->setArrayObjectPrototype(new Model\Post());

                    //Toda vez que a TableGateway for buscar informações ela vai retornar
                    // no formato $resultSetPrototype (entidade post)
                    return new TableGateway('post', $dbAdapter, null, $resultSetPrototype);
                }
            ]
        ];
    }

    /* é executado autmaticamente pelo zend mvc para
    carregar nossos controllers, quando o controller tem uma
    dependencia é melhor usar ele do que o arquivo module.config. */
    public function getControllerConfig()
    {
        return [
            'factories' => [
                BlogController::class => function ($container) {
                    return new BlogController(
                        $container->get(Model\PostTable::class)
                    );
                }
            ]
        ];

    }
}
```
Ex2:

```php
class Module implements
    ConfigProviderInterface,
    ServiceProviderInterface ,
    ControllerProviderInterface
{
    public function getConfig()
    {
        return include __DIR__."/../config/module.config.php";
    }

    public function getServiceConfig()
    {
        return[
            'factories' => [
                // $conteiner tem acesso a todos os serviços
                Model\PostTable::class => PostTableFactory::class,
                Model\PostTableGateway::class => PostTableGatewayFactory::class
            ]
        ];
    }

    /* é executado autmaticamente pelo zend mvc para
    carregar nossos controllers, quando o controller tem uma
    dependencia é melhor usar ele do que o arquivo module.config. */
    public function getControllerConfig(){
        return[
            'factories' => [
                BlogController::class => BlogControllerFactory::class
            ]
        ];

    }
}
```




    
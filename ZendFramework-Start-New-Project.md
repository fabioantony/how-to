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

```

PROJECT_NAME.localhost 

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
> GRANT all ON PROJECT_NAME.* TO 'PROJECT_NAMEUser' WITH GRANT OPTION;
> GRANT all ON PROJECT_NAMETests.* TO 'PROJECT_NAMEUser' WITH GRANT OPTION;

```

********************************************

Install Doctrine (optional)

```bash

$ cd /var/www/PROJECT_NAME
$ composer require doctrine/doctrine-orm-module
$ mkdir -p /var/www/PROJECT_NAME/data/DoctrineORMModule/Proxy/
$ chmod -R 755  /var/www/PROJECT_NAME/data/DoctrineORMModule/Proxy/

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

##Doctrine Configuration

********************************************

Register the doctrine module config/application.config.php

```php
    return array(
        'modules' => array(
            'DoctrineModule',
            'DoctrineORMModule',
            # ...
        ),
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


    
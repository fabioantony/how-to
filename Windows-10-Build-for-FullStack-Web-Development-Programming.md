Debian 9 Stretch 64bits Build for FullStack Web Development Programming

---------------------

Install Composer PHP 7

Download PHP 7 (VC15 x64 Non Thread Safe)

```
https://windows.php.net/download#php-7.2
```

Extract the contents of the zip file into C:\PHP7
Copy C:\PHP7\php.ini-development to C:\PHP7\php.ini
Edit php.ini

```
 extension_dir = “ext”
```
 
```
extension=bz2
extension=curl
extension=fileinfo
extension=gd2
extension=gettext
extension=gmp
extension=intl
extension=imap
;extension=interbase
extension=ldap
extension=mbstring
;extension=exif      ; Must be after mbstring as it depends on it
;extension=mysqli
;extension=oci8_12c  ; Use with Oracle Database 12c Instant Client
extension=odbc
extension=openssl
;extension=pdo_firebird
extension=pdo_mysql
;extension=pdo_oci
extension=pdo_odbc
extension=pdo_pgsql
extension=pdo_sqlite
extension=pgsql
extension=shmop
``` 

Add C:\PHP7 to the Windows 10 system path environment variable.

Open Cmd and check

```
php -v
```

---------------------

Install Composer

On cmd

```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
echo @php "%~dp0composer.phar" %*>composer.bat
```

Check installation

On cmd
```
composer -v
```

On git bash
```
composer.phar -v
```

---------------------
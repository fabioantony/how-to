---------------------

Add Non-free

Use your favourite text editor and edit /etc/apt/sources.list by adding non-free contrib

---------------------

Atualização

$ apt update
$ apt upgrade

----------------------

Add Usuário ao Sudo

$ usermod -a -G sudo <username>

----------------------

VirtualBox


Adicionar esta fonte ao repositórios no arquivo /etc/apt/sources.list

deb http://download.virtualbox.org/virtualbox/debian stretch contrib

2. Instale a chave de autenticação: 

# wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | apt-key add - 

3. Atualize seus repositórios: 

# apt update 

4. Instale o VirtualBox: 

# apt install -y virtualbox-5.1



---------------------
Nvidia Drivers

$ apt install nvidia-driver

---------------------

Google Chrome

wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
$ sudo dpkg -i google-chrome-stable_current_amd64.deb
$ sudo apt-get -f install
caso encontre erro execute abaixo e volte ao inicio
$ apt --fix-broken install

--------------------

Dropbox

$ apt install nautilus-dropbox

----------------

Clamav

$ sudo apt-get install clamav clamtk
$ sudo freshclam #update database
$ sudo clamtk #open antivirus

Arquivo de configuração /etc/clamav/freshclam.conf

Caso encontre os erros abaixo

ERROR: /var/log/clamav/freshclam.log is locked by another process
ERROR: Problem with internal logger (UpdateLogFile = /var/log/clamav/freshclam.log).

$ sudo rm /var/log/clamav/freshclam.log

------------------

PHP environment

sudo apt -y install php7.0 php7.0-common php7.0-interbase php7.0-bz2 php7.0-curl php7.0-zip php7.0-xsl php7.0-xmlrpc php7.0-xml php7.0-mysql php7.0-mbstring php7.0-mcrypt php7.0-soap php7.0-sqlite3 php7.0-gd php7.0-pgsql libtool mysql-server phpunit phpdox php-xdebug php-apigen composer vagrant git git-gui

---------------

Config MySql

$ sudo systemctl status mysql
$ sudo mysql_secure_installation #yes for all

--------------

Config PHP

$ sudo vim 	/etc/php/7.0/cli/php.ini

1) Set your Timezone
ex.: date.timezone = "America/Sao_Paulo"

2) Error Reporting
error_reporting = E_ALL

3) Display Errors
display_errors = On

4) Display Startup Errors
display_startup_errors = On

5) Track Errors
track_errors = On

6) Short Open Tag 
short_open_tag = On

7) Maximum allowed size for uploaded files.
upload_max_filesize = 200M

9) Check Php extensions.
--------------

Configure Git

$ git config --global user.name “YOUR_USERNAME”
$ git config --global user.email “YOUR@EMAIL.COM”

-------------

Configure Composer

$ composer config --global github-oauth.github.com YOUR_TOKEN

-------------

Install PHPStorm IDE

#check last version
$ wget https://download-cf.jetbrains.com/webide/PhpStorm-2017.3.2.tar.gz
$ tar -vzxf PhpStorm-*.tar.gz
$ rm PhpStorm-*.tar.gz
$ sudo mkdir /usr/local/PhpStorm
$ sudo chown YOUR_USERNAME /usr/local/PhpStorm
$ mv PhpStorm-*/* /usr/local/PhpStorm/
$ sudo chmod +x /usr/local/PhpStorm/bin/phpstorm.sh
$ /usr/local/PhpStorm/bin/phpstorm.sh

---------------

www

sudo chown -R YOUR_USERNAME /var/www

---------------

Astah

http://cdn.change-vision.com/files/astah-community-7.1.0.f2c212-0_all.deb

---------------

Install java JDK 64

$su -

#check last version before
$ wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u151-b12/e758a0de34e24606bca991d704f6dcbf/jdk-8u151-linux-x64.tar.gz

$ tar -zxvf jdk-8u*-linux-*.tar.gz
$ mkdir /usr/java/
$ mv jdk1.8.*/* /usr/java/
$ rm jdk-8u*-linux-*.tar.gz

#java
$ update-alternatives --install /usr/bin/java java /usr/java/bin/java 200000

#Install javac
$ update-alternatives --install /usr/bin/javac javac /usr/java/bin/javac 200000

#Check instalation
$ java -version
$ javac -version

----------------------

Install NodeJS

# - requires given command to be executed with root privileges either directly as a root user or by use of sudo command
$ - given command to be executed as a regular non-privileged user

$ curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
# sudo apt install nodejs
# sudo apt install build-essential libssl-dev
$ npm install express
$ node -v
$ npm -v


-----------------------

Install TypeScript

$ sudo npm install -g typescript
$ tsc -v

----------------------

Install Angular Cli
 
$ sudo npm install -g @angular/cli
$ ng -v

----------------------

Printer Epson EcoTank L365

1)Requiriments

$ sudo apt install cups lsb

Printer Driver

$ wget https://download3.ebz.epson.net/dsc/f/03/00/03/45/41/58b06443ec2b00696f49aaef0ee0e3ea3c1354d2/epson-inkjet-printer-201401w_1.0.0-1lsb3.2_amd64.deb

$ sudo dpkg -i epson-inkjet-printer-201401w_1.0.0-1lsb3.2_amd64.deb

$ rm epson-inkjet-printer-201401w_1.0.0-1lsb3.2_amd64.deb

Scan Driver

$ wget https://download2.ebz.epson.net/imagescanv3/debian/latest1/deb/x64/imagescan-bundle-debian-9-1.3.23.x64.deb.tar.gz

$ tar -vzxf imagescan-bundle-debian-9-1.3.23.x64.deb.tar.gz

$ sudo chmod +x imagescan-bundle-debian-9-1.3.23.x64.deb/install.sh

$ sudo imagescan-bundle-debian-9-1.3.23.x64.deb/install.sh

$ rm -R imagescan-bundle-debian-9-1.3.23.x64.deb

$ rm imagescan-bundle-debian-9-1.3.23.x64.deb.tar.gz

-------------

AngrySearch

$ sudo apt install python3-pyqt5
$ git clone https://github.com/DoTheEvo/ANGRYsearch.git
$ cd ANGRYsearch
$ sudo chmod +x install.sh
$ sudo ./install.sh
$ rm -R ANGRYsearch

-----------

Kdenlive

$ sudo apt install kdenlive breeze frei0r-plugins breeze-icon-theme

Configurações>style
Mude para breeze

Configurações>theme
Mude para breeze ou breeze-dark

Configurações>
Deixe marcado force  Breeze Icon Theme


-------------

Laravel Installer

$ composer global require "laravel/installer"
$ echo 'export PATH="$HOME/.config/composer/vendor/bin:$PATH"' >> ~/.bashrc
$ source ~/.bashrc

Spark Installer

$ git clone https://github.com/laravel/spark-installer.git ~/.config/composer/vendor/laravel/
$ cd ~/.config/composer/vendor/laravel/
$ mv spark-intaller spark
$ cd spark
$ composer install
$ ln -s /home/raoni/.config/composer/vendor/laravel/spark/spark ~/.config/composer/vendor/bin/spark


----

Atom Ide

$ wget https://atom.io/download/deb -O atom.deb
$ sudo dpkg -i atom.deb

Extenções
    pigments: https://atom.io/packages/pigments
    Multi Cursor: https://atom.io/packages/multi-cursor
    Fuzzy File Finder: https://atom.io/packages/fuzzy-finder
    Terminal-Plus: https://atom.io/packages/terminal-plus
    atom-typescript: https://atom.io/packages/atom-typescript
    angular-2-typescript-snippets: https://atom.io/packages/angular-2-typeScript-snippets
    file-icons: https://atom.io/packages/file-icons
    pdf-view: https://atom.io/packages/pdf-view
    emmet: https://atom.io/packages/emmet

    atom-bootstrap4
    Atom Package Library
    Git-Aware
    Command Palette: https://atom.io/packages/command-palette






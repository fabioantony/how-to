##Fedora Workstation 64bits Build for PHP Programming

Update Fedora

```bash

$ sudo dnf -y update
$ reboot

```

****************************************************************

Install RPMFusion Repositories

```bash

$ su -

$ rpm --import "http://rpmfusion.org/keys?action=AttachFile&do=get&target=RPM-GPG-KEY-rpmfusion-free-fedora-24" "http://rpmfusion.org/keys?action=AttachFile&do=get&target=RPM-GPG-KEY-rpmfusion-nonfree-fedora-24"

$ dnf install -y http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm http://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm

```

****************************************************************

Install Google Chrome

```bash

$ su -

$  cat << EOF > /etc/yum.repos.d/google-chrome.repo
[google-chrome]
name=google-chrome - \$basearch
baseurl=http://dl.google.com/linux/chrome/rpm/stable/\$basearch
enabled=1
gpgcheck=1
gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub
EOF

$ dnf install -y google-chrome-stable

```
****************************************************************

Install Clamav Antivirus

```bash

$ sudo dnf install -y clamav clamav-update clamtk
$ sudo vi /etc/freshclam.conf #edit configuration
$ sudo freshclam #update database
$ sudo clamtk #open antivirus

```

****************************************************************

Install Adobe Flash Player

```bash

$ su -
$ rpm -ivh http://linuxdownload.adobe.com/adobe-release/adobe-release-x86_64-1.0-1.noarch.rpm
$ rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-adobe-linux
$ dnf install -y flash-plugin alsa-plugins-pulseaudio libcurl

```

****************************************************************

Install VirtualBox

```bash

$ su -
$ cd /etc/yum.repos.d/
$ wget http://download.virtualbox.org/virtualbox/rpm/fedora/virtualbox.repo

#check your kernel version
$ rpm -qa kernel |sort -V |tail -n 1
$ uname -r

#dependencies
$ dnf install -y binutils gcc make patch libgomp glibc-headers glibc-devel kernel-headers kernel-devel dkms
 
#Install VirtualBox
$ dnf install -y VirtualBox-5.0

#Rebuild Kernel modules
$ /usr/lib/virtualbox/vboxdrv.sh setup

#Add your username 
$ usermod -a -G vboxusers YOUR_USERNAME

#Run VirtualBox
$ VirtualBox
$ reboot
```

****************************************************************

Install PHP environment

```bash

 $ dnf -y install libtool httpd mariadb-server php php-mysql php-gd php-mcrypt php-mbstring php-pear php-pecl-xdebug php-pear-PhpDocumentor php-phpunit-PHPUnit apigen git git-gui composer vagrant

```

****************************************************************

Configure Apache Server

```bash

$ sudo systemctl start httpd
$ sudo systemctl enable httpd
$ systemctl status httpd # check status
$ ifconfig | grep inet #get your localhost ip
$ sudo chmod -R a+w /var/www
$ sudo /sbin/restorecon -R /var/www/ #SELinux

```

****************************************************************

Configure MariaDB

```bash

$ sudo systemctl start mariadb
$ sudo systemctl enable mariadb
$ sudo systemctl status mariadb
$ mysql_secure_installation #yes for all

```
****************************************************************

Configure PHP

```bash

#Set your Timezone
#ex.: date.timezone = "America/Sao_Paulo"
$ vim /etc/php.ini 

```

****************************************************************

Configure Git

```bash

$ git config --global user.name “YOUR_USERNAME”
$ git config --global user.email “YOUR@EMAIL.COM”

```

****************************************************************

Configure Composer

```bash

$ composer config --global github-oauth.github.com YOUR_TOKEN

```

****************************************************************

Install MySql WorkBench Community

```bash

#dependencies
$ sudo dnf install -y libuuid-devel boost-devel pcre-devel unixODBC-devel gdal-devel libxml2-devel libzip-devel gdk-pixbuf2-devel glibmm24-devel pango-devel cairo-devel gtk2-devel gtkmm24-devel vsqlite++-devel tinyxml-devel python-libs python-paramiko autoconf automake libtool gcc-c++ pkgconfig libncurses.so.5

#Mysql Workbench
$ sudo rpm -ivh http://cdn.mysql.com//Downloads/MySQLGUITools/mysql-workbench-community-6.3.8-1.fc24.x86_64.rpm

```

****************************************************************

Install java JDK 64

```bash

$su -

#check last version before
$ wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u111-b14/jdk-8u111-linux-x64.rpm

$ rpm -Uvh jdk-8u111-linux-x64.rpm

#java
$ alternatives --install /usr/bin/java java /usr/java/latest/jre/bin/java 200000

#javaws
$ alternatives --install /usr/bin/javaws javaws /usr/java/latest/jre/bin/javaws 200000
 
#Java Browser (Mozilla) Plugin 64-bit
$ alternatives --install /usr/lib64/mozilla/plugins/libjavaplugin.so libjavaplugin.so.x86_64 /usr/java/latest/jre/lib/amd64/libnpjp2.so 200000
 
#Install javac
$ alternatives --install /usr/bin/javac javac /usr/java/latest/bin/javac 200000
$ alternatives --install /usr/bin/jar jar /usr/java/latest/bin/jar 200000

# export JAVA_HOME JDK/JRE 
$ export JAVA_HOME="/usr/java/latest" 

# Change version
$ alternatives --config java
$ alternatives --config javaws
$ alternatives --config libjavaplugin.so.x86_64
$ alternatives --config javac

#Check instalation
$ java -version
$ javac -version
$ javaws

```

*******************************************************************

Install Netbeans IDE

```bash

$ sudo mkdir /usr/local/NetBeans
$ sudo chown YOUR_USERNAME /usr/local/NetBeans
$ sudo mkdir /usr/local/GlassFish
$ sudo chown YOUR_USERNAME /usr/local/GlassFish
$ wget http://download.netbeans.org/netbeans/8.2/final/bundles/netbeans-8.2-linux.sh
$ sudo chmod +x netbeans-8.2-linux.sh

#note: JDK Path /usr/java/latest
#note: GlassFish Install Path /usr/local/GlassFish
#note: NetBeans Install Path /usr/local/NetBeans
$ netbeans-8.1-linux.sh 

```

****************************************************************

Install PHPStorm IDE

```bash

#check last version
$ wget https://download.jetbrains.com/webide/PhpStorm-2016.2.2.tar.gz
$ tar -vzxf PhpStorm-2016.2.2.tar.gz
$ sudo mkdir /usr/local/PhpStorm
$ sudo chown YOUR_USERNAME /usr/local/PhpStorm
$ mv -r PhpStorm-*/* /usr/local/PhpStorm/
$ sudo chmod +x /usr/local/PhpStorm/bin/phpstorm.sh
$ /usr/local/PhpStorm/bin/phpstorm.sh

```

****************************************************************

Install Windows Fonts

```bash

#dependencies
$ dnf install -y curl cabextract xorg-x11-font-utils fontconfig

#windows fonts
$ rpm -i https://downloads.sourceforge.net/project/mscorefonts2/rpms/msttcore-fonts-installer-2.6-1.noarch.rpm

```

****************************************************************

Install some useful programs

```bash

$ sudo dnf install -y poedit wine unrar gimp vim umbrello

```

****************************************************************

###Extras

****************************************************************

No password required on system start

>Right Click on desktop -> Settings -> Users

****************************************************************

Monitor does not turn off

>Right Click on desktop -> Settings -> Energy

****************************************************************

Disable recent documents

```bash

$ dconf write /org/gnome/desktop/privacy/remember-recent-files false

```

****************************************************************

Install libdvdcss for DVDs movies

```bash

$ su -
$ rpm --import "http://freshrpms.net/RPM-GPG-KEY-freshrpms"
$ dnf install -y http://ayo.freshrpms.net/fedora/linux/11/x86_64/freshrpms/RPMS/libdvdcss-1.2.10-2.fc11.x86_64.rpm
$ dnf -y install libdvdcss

```

****************************************************************

Common Programs

```bash

#Scanner Utility
$ dnf -y install xsane-common xsane 

#Media Player
$ dnf -y install vlc 

#Calendar
$ dnf -y install california

#Podcast Subscription
$ dnf -y install gpodder

#Feed Reader
$ dnf -y install liferea

#Manager Photos
$ dnf -y install digikam

#Sensor Temperature and voltage
$ dnf -y install lm_sensors

#Codecs for video e audio
$ dnf -y install gstreamer1-plugins-good gstreamer1-plugins-ugly gstreamer1-plugins-bad-free gstreamer1-plugins-bad-free-extras gstreamer1-plugins-bad-freeworld gstreamer-plugin-crystalhd gstreamer-ffmpeg gstreamer-plugins-good gstreamer-plugins-ugly gstreamer-plugins-bad gstreamer-plugins-bad-extras gstreamer-plugins-bad-free gstreamer-plugins-bad-free-extras gstreamer-plugins-bad-nonfree gstreamer-plugins-bad-extras libmpg123 gstreamer1-libav xine-lib-extras-freeworld gstreamer-plugins-schroedinger 

#Dictionary portuguese-br language for LibreOffice
$ dnf -y install libreoffice-langpack-pt-BR 

#Download Torrents
$ dnf -y install deluge

#More configuration for gnome
$ dnf -y install gnome-tweak-tool

#App Gnome for Pomodoro Technique
$ dnf -y install gnome-shell-extension-pomodoro

#Encrypted Folders
$ dnf -y install cryptkeeper

#Copy Discs Utility
$ dnf -y install brasero

#Rip DVDs Utility
$ dnf -y install k9copy

```

****************************************************************

Install Multifunction Printer (Example with epson l365)

(http://download.ebz.epson.net/dsc/search/01/search/searchModule)
(http://support.epson.net/linux/en/imagescanv3.html)

```bash

#print driver
$ sudo dnf install -y lsb #dependency
$ sudo rpm -ivh https://download3.ebz.epson.net/dsc/f/03/00/03/45/41/d95c03482376873661d7a8d4c165b385cd082cf3/epson-inkjet-printer-201401w-1.0.0-1lsb3.2.x86_64.rpm

#scanner driver
$ wget https://download2.ebz.epson.net/imagescanv3/fedora/latest1/rpm/x64/imagescan-bundle-fedora-24-1.1.13.x64.rpm.tar.gz
$ tar -vzxf imagescan-bundle-fedora-24-1.1.13.x64.rpm.tar.gz
$ chmod +x  imagescan-bundle-fedora-24-1.1.13.x64.rpm/install.sh
$ imagescan-bundle-fedora-24-1.1.13.x64.rpm/install.sh


```

## How To Install USDT Wallet app

### Server Requirements
PHP 7.3 FPM, Nginx & Redis Server
```
sudo apt update && apt-get upgrade
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:ondrej/php
sudo apt update
sudo apt install php7.3-fpm php7.3-mysql php7.3-gd php7.3-xml php7.3-bcmath php7.3-curl php7.3-dom php7.3-mbstring
sudo apt install nginx
sudo apt install redis-server
```

### PHP Multithreaded with save threading

```
sudo apt install -y libzip-dev bison autoconf build-essential pkg-config git-core \
libltdl-dev libbz2-dev libxml2-dev libxslt1-dev libssl-dev libicu-dev \
libpspell-dev libenchant-dev libmcrypt-dev libpng-dev libjpeg8-dev \
libfreetype6-dev libmysqlclient-dev libreadline-dev libcurl4-openssl-dev 
```

```
cd $HOME
wget https://github.com/php/php-src/archive/php-7.2.15.tar.gz
tar --extract --gzip --file php-7.2.15.tar.gz
```

```
cd $HOME/php-src-php-7.2.15
./buildconf --force
```

```
CONFIGURE_STRING="--prefix=/etc/php7 --with-bz2 --with-zlib --enable-zip --disable-cgi \
   --enable-soap --enable-intl --with-openssl --with-readline --with-curl \
   --enable-ftp --enable-mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd \
   --enable-sockets --enable-pcntl --with-pspell --with-enchant --with-gettext \
   --with-gd --enable-exif --with-jpeg-dir --with-png-dir --with-freetype-dir --with-xsl \
   --enable-bcmath --enable-mbstring --enable-calendar --enable-simplexml --enable-json \
   --enable-hash --enable-session --enable-xml --enable-wddx --enable-opcache \
   --with-pcre-regex --with-config-file-path=/etc/php7/cli \
   --with-config-file-scan-dir=/etc/php7/etc --enable-cli --enable-maintainer-zts \
   --with-tsrm-pthreads --enable-debug --enable-fpm \
   --with-fpm-user=www-data --with-fpm-group=www-data"

./configure $CONFIGURE_STRING
```

Build PHP.
```
make && sudo make install
```

Add Pthreads

```
sudo chmod o+x /etc/php7/bin/phpize
sudo chmod o+x /etc/php7/bin/php-config
```
```
git clone https://github.com/krakjoe/pthreads.git
cd pthreads
/etc/php7/bin/phpize
```

```
./configure \
--prefix='/etc/php7' \
--with-libdir='/lib/x86_64-linux-gnu' \
--enable-pthreads=shared \
--with-php-config='/etc/php7/bin/php-config'
```
Build and install the extension
```
make && sudo make install
```

```
cd $HOME/php-src-php-7.2.15
sudo mkdir -p /etc/php7/cli/
sudo cp php.ini-production /etc/php7/cli/php.ini

```
Add the pthreads extension to the ini file.
```
echo "extension=pthreads.so" | sudo tee -a /etc/php7/cli/php.ini
echo "zend_extension=opcache.so" | sudo tee -a /etc/php7/cli/php.ini
```
Add phpthread to bin folder, it must be different from main php
```
sudo ln -s /etc/php7/bin/php /usr/bin/phpt
```
Test PHP Thread
```
phpt -v
```

### App installation
- Example for nginx configuration [https://websiteforstudents.com/install-laravel-on-ubuntu-17-04-17-10-with-nginx-mariadb-and-php-support/]

#### Composer installation
- first find installer signature here [https://composer.github.io/pubkeys.html]
```
sudo apt install curl git unzip
cd ~
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
HASH=insert SHA-384 HASH here
php -r "if (hash_file('SHA384', 'composer-setup.php') === '$HASH') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
```
You'll see the following output.
```
Installer verified
```
Install composer
```
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
```
To test your installation, run:
```
composer
```

#### Clone repository to web folder nginx, and do this command :
```
cd usdt-coinone
composer install
cp .env.example .env
```
And run the app

To run service worker within the app, just use this command within app folder
```
php artisan omni:transaction-check
```

### Install Supervisor

```
sudo apt install supervisor
```

copy worker.conf to /etc/supervisor/conf.d
and run this command
```
sudo supervisortctl start all
```





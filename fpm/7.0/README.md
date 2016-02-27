Docker From php:7.0-fpm

Added libraries:

* apcu
* bcmath
* bz2
* calendar
* Core
* ctype
* curl
* date
* dom
* exif
* fileinfo
* filter
* ftp
* gd
* gettext
* hash
* iconv
* igbinary
* imap
* intl
* json
* libxml
* mbstring
* mcrypt
* memcached
* mysqli
* mysqlnd
* openssl
* pcntl
* pcre
* PDO
* pdo_mysql
* pdo_pgsql
* pdo_sqlite
* pgsql
* Phar
* posix
* pspell
* readline
* recode
* redis
* Reflection
* session
* shmop
* SimpleXML
* sockets
* SPL
* sqlite3
* standard
* sysvmsg
* sysvsem
* sysvshm
* tidy
* tokenizer
* wddx
* xdebug
* xml
* xmlreader
* xmlrpc
* xmlwriter
* xsl
* Zend OPcache
* zip
* zlib

Also included various tools:

 * wget
 * unzip
 * git
 * openssh-client
 * composer
 * phpunit
 * phpcs
 * nodejs
 * npm
 * bower
 * gulp

# Docker compose usage

Add to your docker-compose.yml file following:

```
phpfpm:
  command: php-fpm --allow-to-run-as-root
  image: akalongman/dock-php:7.0-fpm
  volumes:
    - /path/to/fpm.conf:/usr/local/etc/php-fpm.conf
    - /path/to/php.ini:/usr/local/etc/php/php.ini
    - /path/to/prject:/var/www/html
  privileged: true
```

Create fpm.conf and php.ini file in the path you have specified in docker-compose.yml

Samples for fpm.conf and php.ini:

 * php.ini - [https://github.com/akalongman/dock-php/wiki/php.ini](https://github.com/akalongman/dock-php/wiki/php.ini)
 * fpm.conf - [https://github.com/akalongman/dock-php/wiki/fpm.conf](https://github.com/akalongman/dock-php/wiki/fpm.conf)

# Configuring nginx docker to work with phpfpm

Add following to your docker-compose.yml:

```
nginx:
  image: nginx:1.9
  volumes:
    - /path/to/nginx.conf:/etc/nginx/conf.d/default.conf
    - /path/to/project:/var/www
  ports:
   - "80:80"
  links:
   - phpfpm
```

Create nginx.conf in the path specified in docker-compose.yml

example nginx.conf - [https://github.com/akalongman/dock-php/wiki/nginx.conf](https://github.com/akalongman/dock-php/wiki/nginx.conf)

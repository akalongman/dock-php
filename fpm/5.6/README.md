Docker From php:5.6-fpm

Added libraries:

* bcmath
* bz2
* calendar
* dba
* enchant
* exif
* ftp
* gd
* gettext
* gmp
* imap
* intl
* ldap
* mbstring
* mcrypt
* mssql
* mysql
* mysqli
* opcache
* pcntl
* pdo
* pdo_dblib
* pdo_mysql
* pdo_pgsql
* pgsql
* pspell
* shmop
* snmp
* soap
* sockets
* sysvmsg
* sysvsem
* sysvshm
* tidy
* wddx
* xmlrpc
* xsl
* zip
* xdebug


Also included php developer bundle:

 * git
 * msmtp-mta
 * openssh-client
 * nodejs
 * composer
 * phpunit
 * phpcs


# Docker compose usage

Add to your docker-compose.yml file following:

```
phpfpm:
  command: php-fpm --allow-to-run-as-root
  image: akalongman/dock-php:5.6-fpm
  volumes:
    - /path/to/fpm.conf:/usr/local/etc/php-fpm.conf
    - /path/to/php.ini:/usr/local/etc/php/php.ini
    - /path/to/project:/var/www/html
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
    - /path/to/prject:/var/www
  ports:
   - "80:80"
  links:
   - phpfpm
```

Create nginx.conf in the path specified in docker-compose.yml

example nginx.conf - [https://github.com/akalongman/dock-php/wiki/nginx.conf](https://github.com/akalongman/dock-php/wiki/nginx.conf)

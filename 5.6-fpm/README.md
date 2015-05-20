Docker From php:5.6.8-fpm

Added libraries:

 * iconv
 * json
 * mcrypt
 * mbstring
 * mysql
 * mysqli
 * pdo_mysql
 * pdo_sqlite
 * phar
 * curl
 * ftp
 * hash
 * session
 * simplexml
 * tokenizer
 * xml
 * xmlrpc
 * zip

# Docker compose usage

Add to your docker-compose.yml file following:

```
phpfpm:
  command: php-fpm --allow-to-run-as-root
  image: akalongman/dock-php:5.6-fpm
  volumes:
    - /path/to/fpm.conf:/usr/local/etc/php-fpm.conf
    - /path/to/php.ini:/usr/local/etc/php/php.ini
    - /path/to/prject:/var/www/html
  privileged: true
```

Create fpm.conf and php.ini file in the path you have specified in docker-compose.yml

Samples for fpm.conf and php.ini:

 * php.ini - [https://gist.github.com/torniker/27472db39a623fe95a2c](https://gist.github.com/torniker/27472db39a623fe95a2c)
 * fpm.conf - [https://gist.github.com/torniker/e5192a3e42b4d72092ca](https://gist.github.com/torniker/e5192a3e42b4d72092ca)

# Configuring nginx docker to work with phpfmp

Add following to your docker-compose.yml:

```
nginx:
  image: nginx:1.9.0
  volumes:
    - /path/to/nginx.conf:/etc/nginx/conf.d/default.conf
    - /path/to/prject:/var/www
  ports:
   - "80:80"
  links:
   - phpfpm
```

Create nginx.conf in the path specified in docker-compose.yml

example nginx.conf - [https://gist.github.com/torniker/d09db27177e158568a51](https://gist.github.com/torniker/d09db27177e158568a51)

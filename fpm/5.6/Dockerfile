FROM php:5.6-fpm

MAINTAINER Avtandil Kikabidze <akalongman@gmail.com>

# Install modules
RUN buildDeps=" \
        libc-client-dev \
        libkrb5-dev \
        freetds-dev \
        libbz2-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
        libsqlite3-dev \
        libssl-dev \
        libcurl3-dev \
        libxml2-dev \
        libzzip-dev \
        libmemcached-dev \
        libxslt1-dev \
        libgmp3-dev \
        libtidy-dev \
        libicu-dev \
        libpq-dev \
    " \
    && apt-get update \
    && apt-get install -y libmcrypt4 libmemcachedutil2 libsybdb5 libxslt1.1 libpq5 \
    && ln -s /usr/lib/x86_64-linux-gnu/libsybdb.a /usr/lib/libsybdb.a \
    && ln -s /usr/lib/x86_64-linux-gnu/libsybdb.so /usr/lib/libsybdb.so \
    && apt-get install -y $buildDeps \
    && ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/include/gmp.h \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-configure imap --with-imap --with-kerberos --with-imap-ssl \
    && docker-php-ext-install gd \
    && docker-php-ext-install pdo_dblib pdo_pgsql pgsql shmop intl pcntl tidy gmp gettext imap mssql sockets xsl pdo soap opcache bz2 calendar gettext bcmath exif iconv json mcrypt mbstring mysql mysqli pdo_mysql pdo_sqlite phar curl ftp hash session simplexml tokenizer xml xmlrpc zip

# Install xDebug
RUN pecl install xdebug
RUN docker-php-ext-enable xdebug

# Install memcached
RUN pecl install memcached
RUN docker-php-ext-enable memcached

# Install Redis
RUN pecl install redis
RUN docker-php-ext-enable redis

# Install additional packages
RUN apt-get update && apt-get install -y git msmtp-mta openssh-client \
    && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false -o APT::AutoRemove::SuggestsImportant=false $buildDeps \
    && rm -rf /var/lib/apt/lists/* && apt-get clean

# Install composer and put binary into $PATH
RUN curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/ \
    && ln -s /usr/local/bin/composer.phar /usr/local/bin/composer

# Install phpunit and put binary into $PATH
RUN curl -sSo phpunit.phar https://phar.phpunit.de/phpunit.phar \
    && chmod 755 phpunit.phar \
    && mv phpunit.phar /usr/local/bin/ \
    && ln -s /usr/local/bin/phpunit.phar /usr/local/bin/phpunit

# Install PHP Code sniffer
RUN curl -OL https://squizlabs.github.io/PHP_CodeSniffer/phpcs.phar \
    && chmod 755 phpcs.phar \
    && mv phpcs.phar /usr/local/bin/ \
    && ln -s /usr/local/bin/phpcs.phar /usr/local/bin/phpcs \
    && curl -OL https://squizlabs.github.io/PHP_CodeSniffer/phpcbf.phar \
    && chmod 755 phpcbf.phar \
    && mv phpcbf.phar /usr/local/bin/ \
    && ln -s /usr/local/bin/phpcbf.phar /usr/local/bin/phpcbf

CMD ["php-fpm"]

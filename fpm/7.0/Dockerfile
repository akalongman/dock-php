FROM php:7.0-fpm

MAINTAINER Avtandil Kikabidze <akalongman@gmail.com>

# Set TERM to suppress warning messages.
ENV TERM=xterm-256color

# php-xdebug version
ENV XDEBUG_VERSION 2_4_0RC4

# Install additional packages
RUN apt-get update && apt-get install -y git openssh-client unzip wget curl build-essential --no-install-recommends

# Install PHP extensions
RUN buildDeps=" \
        libicu-dev \
        libpq-dev \
        libmcrypt-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng12-dev \
        libbz2-dev \
        libxslt-dev \
        libcurl4-openssl-dev \
        libmemcached-dev \
        zlib1g-dev \
        libncurses5-dev \
        libc-client-dev \
        libkrb5-dev \
        libpspell-dev \
        libtidy-dev \
        libxml2-dev \
    " \
    && apt-get install -y $buildDeps --no-install-recommends \
    && rm -r /var/lib/apt/lists/* \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-configure pdo_mysql --with-pdo-mysql=mysqlnd \
    && docker-php-ext-install \
      gettext \
      exif \
      curl \
      calendar \
      intl \
      mbstring \
      mcrypt \
      pcntl \
      pdo_mysql \
      pdo_pgsql \
      pgsql \
      zip \
      opcache \
      iconv \
      mcrypt \
      gd \
      bcmath \
      bz2 \
      xsl \
      xmlrpc \
      mysqli \
      pspell \
      shmop \
      sockets \
      sysvmsg \
      sysvsem \
      sysvshm \
      tidy \
      ftp

# Install php-apcu
RUN pecl install apcu
RUN echo "extension=apcu.so" > /usr/local/etc/php/conf.d/apcu.ini

# Install php-memcached
RUN cd /usr/src/php/ext \
    && curl -fsSL https://github.com/php-memcached-dev/php-memcached/archive/php7.tar.gz -o memcached.tar.gz \
    && mkdir -p memcached \
    && tar -xf memcached.tar.gz -C memcached --strip-components=1 \
    && rm memcached.tar.gz \
    && docker-php-ext-install memcached


# Install php-xdebug
RUN cd /usr/src/php/ext \
    && curl -fsSL https://github.com/xdebug/xdebug/archive/XDEBUG_$XDEBUG_VERSION.tar.gz -o xdebug.tar.gz \
    && mkdir -p xdebug \
    && tar -xf xdebug.tar.gz -C xdebug --strip-components=1 \
    && rm xdebug.tar.gz \
    && docker-php-ext-configure xdebug --enable-xdebug \
    && docker-php-ext-install xdebug

# Install php-igbinary
RUN cd /usr/src/php/ext \
    && curl -fsSL https://github.com/igbinary/igbinary7/archive/master.tar.gz -o igbinary.tar.gz \
    && mkdir -p igbinary \
    && tar -xf igbinary.tar.gz -C igbinary --strip-components=1 \
    && rm igbinary.tar.gz \
    && docker-php-ext-install igbinary

# Install php-redis
RUN cd /usr/src/php/ext \
    && curl -fsSL https://github.com/phpredis/phpredis/archive/php7.tar.gz -o redis.tar.gz \
    && mkdir -p redis \
    && tar -xf redis.tar.gz -C redis --strip-components=1 \
    && rm redis.tar.gz \
    && docker-php-ext-configure redis --enable-redis-igbinary \
    && docker-php-ext-install redis

# Install php-imap
RUN docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    && docker-php-ext-install imap

# Install php-wddx
RUN docker-php-ext-configure wddx --enable-libxml \
    && docker-php-ext-install wddx

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

# Install Node.js/bower/gulp/grunt
RUN curl -sL https://deb.nodesource.com/setup_5.x | bash -
RUN apt-get install -y nodejs
RUN npm install bower gulp grunt-cli -g

# Disable xdebug for composer
RUN sed -i "s/zend_extension=/#zend_extension=/g" /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
# Load xdebug Zend extension with php command
RUN alias php="php -dzend_extension=xdebug.so"
# PHPUnit needs xdebug for coverage. In this case, just make an alias with php command prefix.
RUN alias phpunit="php $(which phpunit)"

# Clean system
RUN apt-get purge -y $buildDeps \
    && apt-get autoremove -y \
    && rm -rf /tmp/* /var/tmp/* \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*


CMD ["php7.0-fpm"]

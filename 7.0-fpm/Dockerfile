FROM ubuntu:trusty
MAINTAINER Avtandil Kikabidze <akalongman@gmail.com>

ADD run.sh /run.sh
ADD vhost.php7.conf /etc/apache2/sites-available/php7.conf

RUN chmod 775 /*.sh
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y wget apache2 libcurl4-openssl-dev libmcrypt-dev libxml2-dev libjpeg-dev libfreetype6-dev libmysqlclient-dev libt1-dev libgmp-dev libpspell-dev libicu-dev librecode-dev
RUN PHP7_DEB_ARCHIVE="http://repos.zend.com/zend-server/early-access/php7/php-7.0-latest-DEB-x86_64.tar.gz" && wget -P /tmp $PHP7_DEB_ARCHIVE && tar xzPf /tmp/php-7*.tar.gz

RUN cp /usr/local/php7/libphp7.so /usr/lib/apache2/modules/
RUN cp /usr/local/php7/php7.load /etc/apache2/mods-available/
RUN echo "\n<FilesMatch \\.php$>\nSetHandler application/x-httpd-php\n</FilesMatch>" >> /etc/apache2/apache2.conf

RUN a2dismod mpm_event && a2enmod mpm_prefork && a2enmod php7 && a2enmod rewrite
RUN a2dissite 000-default && a2ensite php7

ADD www /www
RUN mkdir -p /www/public

EXPOSE 80

CMD ["/run.sh"]
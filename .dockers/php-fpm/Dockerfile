FROM php:7.3.16-fpm
#PHP
RUN apt-get update && apt-get install -y \
        libmcrypt-dev \
        openssl \
        imagemagick \
        && apt-get install -y cron \
        && apt-get install -y git \
        && apt-get install -y libpq-dev \
        && apt-get install -y \
        && apt-get install -y zlib1g-dev \
        libxml2-dev \
        libxslt-dev \
        libsodium-dev \
		libzip-dev

RUN docker-php-ext-install -j$(nproc)\
    zip \
	xml \
    xmlrpc \
    xsl \
    opcache \
    bcmath \
    opcache 
	 


RUN pecl install -o -f redis  xdebug\
&&  rm -rf /tmp/pear \
&&  echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini 

RUN docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pcntl pgsql pdo_pgsql mysqli
RUN docker-php-ext-install \
        pdo_mysql \
        soap \
        sockets

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && ln -s $(composer config --global home) /root/composer

#настройки php-fpm.ini
#ADD conf/fpm.conf /usr/local/etc/php-fpm.d/docker.conf

RUN curl -L https://phar.phpunit.de/phpunit-6.5.phar > /bin/phpunit \
    && chmod +x /bin/phpunit

RUN apt-get install -y graphviz

RUN git clone https://github.com/Shies/PHP7-forp
RUN cd PHP7-forp/ext/forp \
 && git checkout php70+ \
 && export USE_ZEND_DTRACE=1 \
 && phpize \
 && ./configure \
 && make \
 && make install \ 
 && echo "extension=forp.so" >> /usr/local/etc/php/conf.d/forp.ini


WORKDIR /code
CMD ["php-fpm"]

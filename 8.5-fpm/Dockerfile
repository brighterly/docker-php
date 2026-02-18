FROM php:8.5-fpm

ENV PHP_OPCACHE_ENABLE="1"

RUN apt-get update && apt-get -y install \
        git \
        unzip \
        libzip-dev \
        libicu-dev \
    && apt-get clean && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-install pdo pdo_mysql zip bcmath pcntl intl \
    && pecl install redis xdebug \
    && docker-php-ext-enable redis \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN cp "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini" # FIXME
#COPY php.ini $PHP_INI_DIR/php.ini
COPY conf.d/* /usr/local/etc/php/conf.d/

RUN mkdir -p /etc/mysql/conf.d && \
    echo "[client]\nssl-verify-server-cert=0\n" > /etc/mysql/conf.d/99-fix-ssl.cnf

WORKDIR "/app"

EXPOSE 9000

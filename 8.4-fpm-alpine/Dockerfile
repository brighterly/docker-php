FROM php:8.4-fpm-alpine

ENV PHP_OPCACHE_ENABLE="1"

RUN apk add --no-cache \
        git \
        unzip \
        libzip-dev \
        icu-dev \
        $PHPIZE_DEPS \
    && docker-php-ext-install pdo pdo_mysql zip bcmath opcache pcntl intl \
    && pecl install redis \
    && docker-php-ext-enable intl redis \
    # && pecl install xdebug \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN cp "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini" # FIXME
#COPY php.ini $PHP_INI_DIR/php.ini
COPY conf.d/* /usr/local/etc/php/conf.d/

RUN mkdir -p /etc/mysql/conf.d && \
    echo "[client]\nssl-verify-server-cert=0\n" > /etc/mysql/conf.d/99-fix-ssl.cnf

WORKDIR "/app"

EXPOSE 9000

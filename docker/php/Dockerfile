FROM php:7.4-fpm-alpine

SHELL ["/bin/ash", "-oeux", "pipefail", "-c"]

# tinker(psysh)
ARG PSYSH_DIR=/usr/local/share/psysh
ARG PSYSH_PHP_MANUAL=$PSYSH_DIR/php_manual.sqlite
ARG PHP_MANUAL_URL=http://psysh.org/manual/ja/php_manual.sqlite

# timezone
ARG TZ

# composer
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer

RUN apk update && \
  apk add --update --no-cache --virtual=.build-dependencies \
  autoconf=~2.69 \
  gcc=~10.2.1 \
  g++=~10.2.1 \
  make=~4.3 \
  tzdata \
  git=~2.30.2 && \
  apk add libpng libpng-dev libjpeg-turbo-dev libwebp-dev zlib-dev libxpm-dev gd && \
  apk add --update --no-cache \
  icu-dev=~67.1 \
  libzip-dev=~1.7.3 \
  oniguruma-dev=~6.9 && \
  cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime && \
  echo ${TZ} > /etc/timezone && \
  pecl install xdebug && \
  apk del .build-dependencies && \
  docker-php-ext-install intl pdo_mysql mbstring zip bcmath gd && \
  docker-php-ext-enable xdebug && \
  docker-php-ext-install exif && \
  mkdir $PSYSH_DIR && curl $PHP_MANUAL_URL -o $PSYSH_PHP_MANUAL && \
  curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer && \
  composer config -g process-timeout 3600 && \
  composer config -g repos.packagist composer https://packagist.org

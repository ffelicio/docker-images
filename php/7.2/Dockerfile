FROM php:7.2-stretch
LABEL maintainer="boitata@leroymerlin.com.br"

# libssl-dev: required by mongodb
# zlib1g-dev: required by zip
RUN apt-get update -qq \
  && apt-get install -qq --no-install-recommends \
    git \
    zip \
    unzip \
    libssl-dev \
    zlib1g-dev \
  && apt-get clean

RUN pecl install mongodb \
  && docker-php-ext-enable \
    mongodb \
  && docker-php-ext-install \
    pcntl \
    zip

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

ARG UID=1000
ARG GID=1000

RUN groupmod -g ${GID} www-data \
  && usermod -u ${UID} -g www-data www-data \
  && mkdir -p /var/www/html \
  && chown -hR www-data:www-data \
    /var/www \
    /usr/local/

USER www-data:www-data
WORKDIR /var/www/html
ENV PATH=$PATH:/var/www/.composer/vendor/bin

FROM php:7.2-apache

RUN apt-get update \
 && apt-get install -y git zlib1g-dev \
 && docker-php-ext-install zip \
 && a2enmod rewrite \
 && sed -i 's!/var/www/html!/var/www/public!g' /etc/apache2/sites-available/000-default.conf \
 && mv /var/www/html /var/www/public \
 && curl -sS https://getcomposer.org/installer \
  | php -- --install-dir=/usr/local/bin --filename=composer \
 && echo "AllowEncodedSlashes On" >> /etc/apache2/apache2.conf

# Configuração XDebug
ARG INSTALL_XDEBUG=false
RUN if [ ${INSTALL_XDEBUG} = true ]; then \
  if [ $(php -r "echo PHP_MAJOR_VERSION;") = "5" ]; then \
    pecl install xdebug-2.5.5; \
  else \
    pecl install xdebug; \
  fi && \
  docker-php-ext-enable xdebug \
;fi

COPY ./conf/xdebug.ini $PHP_INI_DIR/conf.d/xdebug.ini
RUN sed -i "s|xdebug.remote_autostart=0|xdebug.remote_autostart=1|g" $PHP_INI_DIR/conf.d/xdebug.ini && \
    sed -i "s|xdebug.remote_enable=0|xdebug.remote_enable=1|g" $PHP_INI_DIR/conf.d/xdebug.ini && \
    sed -i "s|xdebug.cli_color=0|xdebug.cli_color=1|g" $PHP_INI_DIR/conf.d/xdebug.ini

# Configuração php.ini
ARG APPLICATION_ENVIRONMENT="production"
ENV APPLICATION_ENVIRONMENT=${APPLICATION_ENVIRONMENT}

ARG TZ="UTC"
RUN echo "date.timezone = ${TZ}" >> $PHP_INI_DIR/conf.d/date.timezone.ini

RUN docker-php-ext-install pdo_mysql

WORKDIR /var/www

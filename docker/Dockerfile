FROM php:7.2-apache
COPY ./php/php.ini /usr/local/etc/php/
COPY ./apache/*.conf /etc/apache2/sites-enabled/

RUN apt-get update \
  && apt-get install -y zlib1g-dev libpq-dev default-mysql-client unzip\
  && docker-php-ext-install zip pdo_mysql mysqli pdo_pgsql\
  && docker-php-ext-enable mysqli

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer
ENV PATH $PATH:/composer/vendor/bin

WORKDIR /var/www/html

RUN sed -i '/<Directory \/var\/www\/>/,/<\/Directory>/ s/AllowOverride None/AllowOverride All/' /etc/apache2/apache2.conf
RUN a2enmod rewrite
RUN service apache2 restart

RUN composer global require "laravel/installer"
# docker-compose exec web composer create-project "laravel/laravel=5.6.*" .
FROM php:8.1-fpm

RUN apt-get update

# Install Postgre PDO
RUN apt-get install -y libpq-dev curl \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql pgsql \

FROM php:8.1-fpm
 
ARG USER='1000'
ARG USER_ID=${USER_ID}
 
ARG GROUP_ID='1000'
ARG GROUP_ID=${GROUP_ID}

ARG PROJECT_PREFIX='develop'
ARG PROJECT_PREFIX=${PROJECT_PREFIX}
ENV PROJECT_PREFIX=${PROJECT_PREFIX}

#RUN groupadd -f -g $GROUP_ID $GROUP_ID && \
#        useradd -u $USER_ID -m -s /bin/bash -g $GROUP_ID $USER_ID

RUN sed -i -e "s/www-data:x:33:33:www-data:\/var\/www:\/usr\/sbin\/nologin/www-data:x:$USER_ID:$GROUP_ID:www-data:\/var\/www\/html:\/bin\/bash/g" /etc/passwd && \
    sed -i -e "s/www-data:x:33:/www-data:x:$GROUP_ID:/g" /etc/group

COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/bin/

RUN apt-get update && apt-get install -y --no-install-recommends \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        curl \
        bash \
    && install-php-extensions memcache \
    && docker-php-ext-enable memcache \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-configure opcache --enable-opcache \
    && docker-php-ext-install opcache \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install sockets\
    && install-php-extensions ldap \
    && install-php-extensions zip \
    && install-php-extensions exif \
    && install-php-extensions soap

ADD ./conf/www.conf "/usr/local/etc/php-fpm.d/www.conf"

WORKDIR /home/www

EXPOSE 9000
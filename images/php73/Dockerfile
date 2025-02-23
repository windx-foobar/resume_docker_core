# Для начала указываем исходный образ, он будет использован как основа
FROM php:7.3-fpm
# Необязательная строка с указанием автора образа
LABEL maintainer="windx-foobar <bezalkogoln1ycoder@gmail.com>"

# RUN выполняет идущую за ней команду в контексте нашего образа.
# В данном случае мы установим некоторые зависимости и модули PHP.
# Для установки модулей используем команду docker-php-ext-install.
# На каждый RUN создается новый слой в образе, поэтому рекомендуется объединять команды.
RUN apt-get update && apt-get install -y \
        curl \
        wget \
        git \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        zip \
        libzip-dev \
    && docker-php-ext-install -j$(nproc) iconv mysqli pdo_mysql zip \
    && docker-php-ext-configure gd --with-jpeg-dir --with-png-dir \
    && docker-php-ext-install -j$(nproc) gd
    
# Куда же без composer'а.
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer1 --version=1.10.19

# Добавим свой php.ini, можем в нем определять свои значения конфига
ADD php.ini /usr/local/etc/php/conf.d/40-custom.ini

# Указываем рабочую директорию для PHP
WORKDIR /var/www

# Добавляем отдельного пользователя
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www
# Переопределяем права
RUN chown -R 1000 /var/www/*
# Change current user to www
USER www

# Запускаем контейнер
# Из документации: The main purpose of a CMD is to provide defaults for an executing container. These defaults can include an executable, 
# or they can omit the executable, in which case you must specify an ENTRYPOINT instruction as well.
CMD php-fpm

FROM php:7.2-apache

# 安装扩展
RUN docker-php-ext-install pdo_mysql mysqli
RUN pecl install -o -f redis && echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini
RUN pecl install -o -f memcached && echo "extension=memcached.so" > /usr/local/etc/php/conf.d/memcached.ini
RUN pecl install -o -f xdebug && echo "extension=xdebug.so" > /usr/local/etc/php/conf.d/xdebug.ini

# 安装Git和Vim
RUN apt update && apt install -y git vim

# URL重写
RUN a2enmod rewrite

# 安装composer
ADD https://getcomposer.org/composer.phar /usr/local/bin/composer
RUN chmod 755 /usr/local/bin/composer && composer config -g repo.packagist composer https://packagist.laravel-china.org

# 安装代码
#COPY ./wwwroot /app/
RUN mkdir /app && chmod -R 777 /app && echo '<? phpinfo();' > /app/index.php

# 修改网站主目录
ENV APACHE_DOCUMENT_ROOT /app
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

WORKDIR /app

# 暴露工作端口
EXPOSE 80
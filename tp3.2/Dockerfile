FROM php:5.6-apache

# 安装 Git Vim
RUN apt update \
    && apt install -y zip unzip git vim libfreetype6-dev libjpeg62-turbo-dev libpng-dev \
	&& apt clean

# 安装扩展
RUN docker-php-ext-install pdo_mysql mysqli mysql \
    && pecl install http://pecl.php.net/get/redis-4.3.0.tgz \
	&& pecl install https://pecl.php.net/get/xdebug-2.5.5.tgz \
	&& docker-php-ext-enable redis xdebug \
	&& a2enmod rewrite

# 安装gd
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && docker-php-ext-install gd

# 安装composer
ADD https://getcomposer.org/composer.phar /usr/local/bin/composer
RUN chmod 755 /usr/local/bin/composer && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/

# 修改网站主目录
ENV APACHE_DOCUMENT_ROOT /app
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf \
	&& sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

# 安装代码
#COPY ./wwwroot /app/
RUN mkdir -p /app && chmod -R 777 /app && echo '<? phpinfo();' > /app/index.php

# 命令别名
RUN echo 'alias "ll=ls -alh"' >> /root/.bashrc

# 设置工作目录
WORKDIR /app

# 暴露工作端口
EXPOSE 80
FROM php:7.0-apache

# 安装 Git Vim php-gd
RUN apt update
RUN apt install -y \
    git vim libfreetype6-dev libjpeg62-turbo-dev libpng-dev \
    && docker-php-ext-install -j$(nproc) iconv \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

# 安装扩展
RUN docker-php-ext-install pdo_mysql mysqli \
    && pecl install redis \
	&& pecl install  xdebug \
	&& docker-php-ext-enable redis xdebug \
	&& a2enmod rewrite



# 安装composer
ADD https://getcomposer.org/composer.phar /usr/local/bin/composer
RUN chmod 755 /usr/local/bin/composer && composer config -g repo.packagist composer https://packagist.laravel-china.org


# 修改网站主目录
ENV APACHE_DOCUMENT_ROOT /app/public
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf \
	&& sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

# 安装代码
#COPY ./wwwroot /app/
RUN mkdir -p /app/public && chmod -R 777 /app && echo '<? phpinfo();' > /app/public/index.php

# 命令别名
RUN echo 'alias "ll=ls -alh"' >> /root/.bashrc

# 设置工作目录
WORKDIR /app

# 暴露工作端口
EXPOSE 80
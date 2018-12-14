FROM zhaishuaigan/php:7.0-dev-tp5

RUN  apt-get install -y libmcrypt-dev \
     && pecl install mcrypt-1.0.1 \
	 && docker-php-ext-enable mcrypt
# 设置工作目录
WORKDIR /app

# 暴露工作端口
EXPOSE 80
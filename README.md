# 一个php环境

## 介绍
    此镜像默认安装了 git vim php-redis php-pdo-mysql php-mysqli, 开发环境还安装了 xdebug. 

## 使用方法:
```
docker run -d -p 80:80 -v /path/to/wwwroot:/app zhaishuaigan/php
```

## 所有版本:
zhaishuaigan/php
zhaishuaigan/php:tp3.2
zhaishuaigan/php:tp5
zhaishuaigan/php:tp6


如果你使用的是ThinkPHP 5.x版本的框架, 可以使用:
```
docker run -d -p 80:80 -v /path/to/wwwroot:/app zhaishuaigan/php:tp5
```
或继承此环境, 安装其他插件:
```
# Dockerfile

# 依赖默认的tp5环境
FROM zhaishuaigan/php:tp5

# 安装其他扩展
RUN pecl install xxx

# 复制代码
COPY ./ /app
WORKDIR /app

# 执行composer扩展安装 和 数据库脚本迁移
RUN composer install && php think migrate:run
```

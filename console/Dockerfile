FROM ubuntu:16.04
MAINTAINER chenjiawen <604989282@qq.com>

#更换为阿里镜像源
COPY ./sources.list /etc/apt/sources.list

# 定义php版本号
ENV PHP_VERSION 7.0

# 安装php和基本工具
RUN apt-get update \
  && apt-get install -y openssh-server php$PHP_VERSION vim git wget unzip

RUN apt-get install -y php$PHP_VERSION-mysql \
    && apt-get install -y php$PHP_VERSION-curl \
    && apt-get install -y php$PHP_VERSION-gd \
    && apt-get install -y php$PHP_VERSION-dom \
    && apt-get install -y php$PHP_VERSION-mbstring

COPY ./composer.phar /usr/local/bin/composer
RUN chmod 777 /usr/local/bin/composer

EXPOSE 22

CMD ["/etc/init.d/ssh", "start","-D"]

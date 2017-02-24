FROM ubuntu:16.04
MAINTAINER chenjiawen <604989282@qq.com>

#更换为阿里镜像源
COPY ./sources.list /etc/apt/sources.list

# 定义php版本号
ENV PHP_VERSION 7.1


# 安装apache和php扩展
RUN apt-get update \
    && apt-get install -y language-pack-en-base \
    && locale-gen en_US.UTF-8 \
    && apt-get install -y software-properties-common \
    && LC_ALL=en_US.UTF-8 add-apt-repository ppa:ondrej/php \
    && apt-get update \
    && apt-get -y install php$PHP_VERSION \
    && apt-get -y install php$PHP_VERSION-fpm

RUN apt-get install -y php$PHP_VERSION-mysql \
    && apt-get install -y php$PHP_VERSION-curl \
    && apt-get install -y php$PHP_VERSION-gd \
    && apt-get install -y php$PHP_VERSION-dom \
    && apt-get install -y php$PHP_VERSION-mbstring

# 引入配置文件
COPY ./php-fpm.conf /etc/php/$PHP_VERSION/fpm/php-fpm.conf
COPY ./www.conf /etc/php/$PHP_VERSION/fpm/pool.d/www.conf

VOLUME /etc/php/$PHP_VERSION/

EXPOSE 9000

CMD ["php-fpm7.1"]

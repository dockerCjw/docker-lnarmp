FROM ubuntu:16.04
MAINTAINER chenjiawen <604989282@qq.com>

#更换为阿里镜像源
COPY ./sources.list /etc/apt/sources.list

# 安装php和基本工具
RUN apt-get update \
  && apt-get install -y openssh-server php vim git wget

EXPOSE 22

CMD ["/etc/init.d/ssh", "start","-D"]

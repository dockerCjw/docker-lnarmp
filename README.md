# docker-lnarmp
使用容器构建一套ngix,apache,redis,mysql,php的完整的开发环境
##1. 对上述的容器集群进行一一介绍；
1. apache-php7.1 服务: 它是一个`apache容器`,其中内部集成了php7.1模块。从dockerfile中可以发现：

	* 使用的了ubuntu16.04容器镜像；
	* 安装了php7.1及其相应的扩展 ，让apache来支持PHP。
	* 软链接了apache的重定向模块。
	* 在构建镜像的时候就把apache的站点配置文件提出来了。注意——当新添站点的时候进行软链接。
	* 把apache与php的配置文件、错误日志和工作目录进行挂载(特指工作目录)或建立数据卷。

2. nginx服务：它主要是为了做apache服务的反向代理或者配合`PHP-fpm` 运行php应用。其中可以发现：

	* 在构建镜像的时候，就把其nginx的配置文件提出来了。
	* 并且为其配置文件建立了数据卷
	* 补充：使用`ubuntu16.04`在构建nginx进行的时候比较繁琐，要关闭master_process,其结果就是每个请求都是master自己来处理，而不是无限下发到每个workder中。所以笔者还是选用了官方nginx镜像。

3. php-fpm 容器服务：主要是为nginx提供处理php脚本服务。其中可以发现：

	* 笔者分别安装了php5.6、php7.0和php7.1的php-fpm。
	* 安装了其相应的扩展。
	* 为其配置文件建立了数据卷。(注意：其php的项目如果是开发环境就使用本地磁盘挂载，如果是生产环境就使用数据卷)
	* 修改了`php-fpm.conf`文件：把其`pid = /run/php/php7.0-fpm.pid` 关闭,把守护进程也关闭`daemonize = no`;修改了`www.conf`文件：`listen = php7.0-fpm:9000` ,其中是因为php7.0-fpm与nginx进行相互依赖才能进行此配置，否则会无法解析(php7.0-fpm)导致报错；

4. console服务：主要是提供命令行的php、composer、unzip等等控制台类型的服务，把其所有的数据卷或挂载的磁盘全部关联到此容器中，进行集中管理。

5. mysqlf服务：直接使用官方的mysql5.7.17容器镜像构建。
6. redis服务：直接使用官方的redis3.2.7容器镜像构建。

## 2.对docker-compose.yml的介绍：
1. 一共安装了8个服务容器，分别为：mysql、redis、apache、nginx、php5.6-fpm、 php7.0-fpm、php7.1-fpm和 console容器。

2. 其中mysql和redis都属于数据库，直接为php相关的容器提供服务即可。有apache、php5.6-fpm、 php7.0-fpm和php7.1-fpm四类容器。当然端口也可以直接暴露对外，如果是多个容器集群就得另当别论。

3. 而所有的php-fpm或apache得依赖于nginx服务。
4. 所有的数据卷全部关联到console进行统一管理。

##推论：
1. 开发环境可以进行统一管理：
2. 生产环境可以一条命令(`docker-compose up -d`)完成构建。


 

---
title: windows上利用docker搭建lnmp开发环境
date: 2019-03-27 18:53:18
categories:
- docker
tags: 
- docker
---

# 安装docker 
到[docker官网](https://www.docker.com/)进行安装
书籍:Docker技术入门与实战 
# 开启Hyper-V

打开控制面板 - 程序和功能 - 启用或关闭Windows功能，勾选Hyper-V，然后点击确定即可，如图：
![Hyper-V](https://hexo-lwy.oss-cn-hangzhou.aliyuncs.com/hexo/v.png)

下载windwos版本docker安装包 地址

# 修改docker 镜像仓库

安装好后登陆docker（没有账号的请到官网进行注册）
![Hyper-V](https://hexo-lwy.oss-cn-hangzhou.aliyuncs.com/hexo/v2.png)

# 使用git快速获取lnmp

切换到你准备安装dnmp的目录下

```
$ git clone https://github.com/shmilylbelva/dnmp.git
$ cd dnmp
$ docker-compose up
```

完成以后可以在浏览器中访问localhost，出现界面代表ok

# 站点部署

`c:\Windows\System32\Drivers\etc\hosts`

本文有默认加了两个站点：www.site1.com（同localhost）和www.site2.com。
要在本地访问这两个域名，需要修改你的hosts文件，添加以下两行：
127.0.0.1 www.site1.com
127.0.0.1 www.site2.com
其中，www.site2.com为支持SSL/https和HTTP/2的示例站点。
因为站点2的SSL采用自签名方式，所以浏览器有安全提示，继续访问就可以了，自己的站点用第三方SSL认证证书替换即可。
如果只用到站点1，把站点2相关的目录和配置文件删除：
./conf/nginx/conf.d/certs/site2/
./conf/nginx/conf.d/site2.conf
./www/site2/
重启容器内的Nginx生效：
docker exec -it dlnmp_nginx nginx -s reload


# dnmp目录结构

```
.
├── conf                        配置目录
│   ├── conf.d              站点配置文件目录
│   │   ├── certs           SSL认证文件、密钥和加密文件目录
│   │   │   └── site2       站点2的认证文件目录
│   │   ├── site1.conf      站点1 Nginx配置文件
│   │   └── site2.conf      站点2 Nginx配置文件 
│   ├── my.cnf              MySQL配置文件           
│   ├── nginx.conf          Nginx通用配置文件
│   ├── php-fpm.d           PHP-FPM配置目录
│   │   └── www.conf        PHP-FPM配置文件
│   ├── php.ini             PHP配置文件
├── docker-compose.yml        默认容器启动配置文件
├── docker-compose54.yml      php5.4容器启动配置文件
├── docker-compose56.yml      php5.6容器启动配置文件
├── log                         日志目录
│   ├── mysql.slow.log                   MySQL日志
│   ├── nginx.error.log                   Nginx日志
│   ├── nginx.site1.error.log          
│   ├── nginx.site2.error.log           
├── mysql                       MySQL数据文件目录
├── php                          PHP版本目录
└── www                         站点根目录
    ├── site1                   站点1根目录
    └── site2                   站点2根目录
```
# MYSQL说明
在docker-compose.yml文件中，我们指定了MySQL数据库root用户的密码为123456。
所以，我们就可以在主机中通过：

```
$ mysql -h 127.0.0.1 -u root -p  #linux中
#在mac中需要先切换到mysql容器
$ docker container ls  #列出容器列表
$ docker exec -it 775c7c9ee1e1 /bin/bash  #其中的容器id不用输入完整的mysql容器id,一般3位就能区分。
$ mysql -h 127.0.0.1 -uroot -p
```
输入密码，就可以进入MySQL命令行。


说明：这里MySQL的连接主机不能用localhost，因为MySQL客户端默认使用unix socket方式连接，应该直接用本地IP。
在PHP代码中的使用方式与在主机中使用稍有不同，如下：
$pdo = new PDO('mysql:host=mysql;dbname=site1', 'root', '123456');
其中，host的值就是在docker-compose.yml里面指定的MySQL容器的名称。
这是因为PHP代码是在FPM容器中，FPM容器启动时会自动在/etc/hosts中加上：
172.17.0.2 mysql 11e55f91c4c3 dnmp_mysql
就是说，mysql自动指向了MySQL容器动态生成的IP。
注意，这里用php进行mysql连接测试会失败（在docker-compose up的时候注意到存在mbind:Operation not permitted 这个提示）

，所以还需要处理上述问题。
进入刚刚的mysql终端,内容大致如下。host为 % 表示不限制ip localhost表示本机使用 plugin非mysql_native_password 则需要修改密码

```
mysql> select host,user,plugin,authentication_string from mysql.user;    
+-----------+------------------+-----------------------+------------------------------------------------------------------------+  
| host      | user             | plugin                | authentication_string                                                  |  
+-----------+------------------+-----------------------+------------------------------------------------------------------------+  
| %         | root             | caching_sha2_password | $A$005$^]RQB}j~t!      .#v)3.UogPRFu8VJA5/GKEbK5edEQlMT5sHw2n72zYJNlIbo3 |  
| localhost | mysql.infoschema | mysql_native_password | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE                              |  
| localhost | mysql.session    | mysql_native_password | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE                              |  
| localhost | mysql.sys        | mysql_native_password | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE                              |  
| localhost | root             | caching_sha2_password | $A$005$Y6&q!59^Fmh)@-6TG58J3F5+3I/HI9L|JCadNG+-+d6W+1D_UFW+7MRD7F3 |  
+-----------+------------------+-----------------------+------------------------------------------------------------------------+ 
```

依次进行如下操作

```
#更新一下用户的密码 root用户密码为newpassword  
mysql> ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';
FLUSH PRIVILEGES;
```
mysql连接问题解决。

# 修改docker-compose.yml
如果容器已经生成，回头再编辑docker-compose.yml，用
docker-compose up
命令会直接启动原来的容器，修改的内容不会体现在启动的容器里。
所以，要使修改的docker-compose.yml生效，需要以下4步：

```
$ docker stop dnmp_nginx                      # 第一步：停止容器
$ docker rm dnmp_nginx                        # 第二步：删除容器
# !!第三步：重启Docker服务!!
$ docker-compose up -d --no-deps --build mysql  # 第四步：重新启动容器
```

其中最后一条命令参数作用：/
-d：后台执行
--no-deps：不启动link的容器
--build：启动容器前先构建镜像

# 使用Redis

Redis使用和MySQL类似。
不过需要注意的是在./php/php72中的Dockerfile末尾的

```
#源码安装方式
#php7 can install

ENV PHPREDIS_VERSION 4.0.0
RUN curl -L -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/$PHPREDIS_VERSION.tar.gz \
    && tar xfz /tmp/redis.tar.gz \
    && rm -r /tmp/redis.tar.gz \
    && mkdir -p /usr/src/php/ext \
    && mv phpredis-$PHPREDIS_VERSION /usr/src/php/ext/redis \
    && docker-php-ext-install redis \
    && rm -rf /usr/src/php
```
如果是php5.X那么这里应该是这样的（需要自己添加到对应的Dockerfile中，然后再docker-compose up）

```
#PECL安装方式
#php5 can install

#添加扩展 redis pecl方式
RUN apk add --no-cache --update libmemcached-libs zlib
RUN set -xe \
    && apk add --no-cache --update --virtual .phpize-deps $PHPIZE_DEPS \
    && pecl install -o -f redis  \
    && echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini
    && rm -rf /usr/share/php \
    && rm -rf /tmp/* \
    && apk del  .phpize-deps
```

在主机和容器内部都通过地址127.0.0.1，端口6379访问。

PHP则是跨容器访问，host参数用redis（links指定的名称），端口用6379。
修改site2的index.php文件内容如下

```
<?php
    $redis = new Redis();
    $redis->connect('192.168.1.11',6379);//修改成自己的ip
    $redis->set('name','青波');
    echo $redis->get('name');
        //检测是否连接成功
```
浏览器访问www.site2.com,出现‘青波’即代表redis扩展正常。


作者：回眸淡然笑
链接：https://www.jianshu.com/p/31c09a3e0d5d
来源：简书
简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。
[TOC]

## nginx

1. 简介

```
Nginx 是高性能的 HTTP 和反向代理的服务器，处理高并发能力是十分强大的，能经受高负
载的考验,有报告表明能支持高达 50,000 个并发连接数。
```

### 1：反向代理

正向代理

```
（1）需要在客户端配置代理服务器进行指定网站访问；

	直接访问不可以访问到；
需要在   浏览器中配置  代理服务器，通过代理服务器去访问 目标服务器
```

![image-20210920225330938](C:/Users/%E8%90%8C%E4%B8%BB/AppData/Roaming/Typora/typora-user-images/image-20210920225330938.png)

反向代理

```
暴露的是代理服务器地址，隐藏了真实服务器 IP 地址。


反向代理，其实客户端对代理是无感知的，因为客户端不需要任何配置就可以访问，我们只
需要将请求发送到反向代理服务器，由反向代理服务器去选择目标服务器获取数据后，在返
回给客户端，此时反向代理服务器和目标服务器对外就是一个服务器，暴露的是代理服务器
地址，隐藏了真实服务器 IP 地址
```

![image-20210920225351884](C:/Users/%E8%90%8C%E4%B8%BB/AppData/Roaming/Typora/typora-user-images/image-20210920225351884.png)

### 2：负载均衡

```
客户端发送多个请求到服务器，服务器处理请求，有一些可能要与数据库进行交互，服
务器处理完毕后，再将结果返回给客户端。

概念：
增加服务器的数量，然后将请求分发到各个服务器上，将原先请求集中到单个服务器上的
情况改为将请求分发到多个服务器上，将负载分发到不同的服务器，也就是我们所说的负
载均衡
```

![image-20210920225632915](C:/Users/%E8%90%8C%E4%B8%BB/AppData/Roaming/Typora/typora-user-images/image-20210920225632915.png)

### 3：动静分离

```
为了加快网站的解析速度，可以把动态页面和静态页面由不同的服务器来解析，加快解析速
度。降低原来单个服务器的压力。
```

![image-20210920230103542](C:/Users/%E8%90%8C%E4%B8%BB/AppData/Roaming/Typora/typora-user-images/image-20210920230103542.png)

### 4：安装依赖及 nginx 的安装

```shell
# 1：首先安装  pcre 依赖；

# 1.1：联网下载  pcre 压缩文件依赖
wget http://downloads.sourceforge.net/project/pcre/pcre/8.37/pcre-8.37.tar.gz

# 1.2：解压
tar –xvf pcre-8.37.tar.gz

# 1.3：在 pcre-8.37 的目录下执行
./configure

# 1.4: 在 pcre目录下，执行：
make && make install

# 1.5: 在 pcre 目录下检查版本
pcre-config --version
```

```shell
# 2：安装 openssl 、zlib 、 gcc 依赖  --->  在 pcre 目录下

yum -y install make zlib zlib-devel gcc-c++ libtool openssl openssl-devel
```

```shell
# 3：安装  nginx 
# 使用 xftp 将压缩文件传过去

# 解压缩：
tar -xvf nginx-1.20.1.tar.gz   (后面的版本名可以换)

# 解压缩完之后，在 nginx-1.20.1 目录下，执行
./congfigure

# 再执行：   --->  如果执行不成功 就需要安装系统对应的版本，一般是越新 Linux 版本安装 越高 nginx 的版本
make && make install


# 安装成功之后 会在 该目录下 的 /usr/local 中看到  nginx
```



```shell
#  我们直接访问  usr/local/nginx/rbin 目录
cd usr/local/nginx/rbin

# 然后试着启动一下 nginx ，在档期那目录下执行；
./ngnix

# 查看进程
ps -ef | grep nginx

# 在 nginx 目录下，找到  nginx.conf，使用  vim  编译器打开它,查看其中使用的端口号
vi nginx.conf

# 80  端口是默认不开放的，我们需要打开 80 打开
# 进入防火墙 , 查看  ports 选项
firewall-cmd --list-all

# 打开 80  端口  (没有 root 记得使用 sudo)
firewall-cmd --add-port=80/tcp –permanent

# 重启防火墙
firewall-cmd –reload

看到下面界面说明安装完成
```

![image-20210921011110219](C:/Users/%E8%90%8C%E4%B8%BB/AppData/Roaming/Typora/typora-user-images/image-20210921011110219.png)

### nginx 的常用操作命令

```shell
首先需要注意的时  nginx 的命令是必须在   /usr/local/nginx/sbin  下
# 0: 查看版本号
./nginx -v

# 1：启动  nginx   
./nginx

# 2: 关闭  nginx
./nginx -s stop

# 3: 重新加载
./nginx -s reload
```

### nginx 的配置文件简介

```shell
 cd  /usr/local/nginx/conf/nginx.conf  # ---> 就是他的配置文件
 
 由 3 部分组成：
 1：全局块
 	从配置文件开始到 events 块之间的内容，主要会设置一些影响 nginx 服务器整体运行的配置指令，主要包括配置运行 Nginx 服务器的用户（组）、允许生成的 worker process 数，进程 PID 存放路径、日志存放路径和类型以及配置文件的引入等。
 	
 其中 worker_processes 1  
 	这是 Nginx 服务器并发处理服务的关键配置，worker_processes 值越大，可以支持的并发处理量
 	也越多，但是会受到硬件、软件等设备的制约
 	
 	
 	
  2：events 块
  	events 块涉及的指令主要影响 Nginx 服务器与用户的网络连接，常用的设置包括是否开启对多 
work process 下的网络连接进行序列化，是否允许同时接收多个网络连接，选取哪种事件驱动模型来处理
连接请求，每个 word process 可以同时支持的最大连接数等。

其中  worker_connections  1024;
	每个 work process 支持的最大连接数为 1024.
 
这部分的配置对 Nginx 的性能影响较大，在实际中应该灵活配置。
  	
  	
   3：http 块
 这算是 Nginx 服务器配置中最频繁的部分，代理、缓存和日志定义等绝大多数功能和第三方模块的配置都
在这里。需要注意的是：http 块也可以包括 http 全局块、server 块。  	 
  
  3.1：http 全局块：
  	http 全局块配置的指令包括文件引入、MIME-TYPE 定义、日志自定义、连接超时时间、单链接请求数
 上限等。

  3.2：server 块：
    这块和虚拟主机有密切关系，虚拟主机从用户角度看，和一台独立的硬件主机是完全一样的，该技术的产
生是为了节省互联网服务器硬件成本。

```

```shell
#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}



http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       80;            #  nginx 目前监听的端口号
        server_name  localhost;     #  主机名

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}
```

### nginx 配置实例  -  反向代理1

```shell
实现效果：使用 nginx 反向代理，访问 www.123.com 直接跳转到 127.0.0.1:8080

1：配置 tomcat , 安装解压之后。进入   /bin  目录执行: （启动 tomcat）
./starpup.sh

2：查看日志文件：（里面由 tomcat 启动的信息）
cd ..          # 返回上一级文件目录
tail -f catalina.out

3：对外开放访问的端口
# 进入防火墙 , 查看  ports 选项
firewall-cmd --list-all

# 打开 8080  端口  (没有 root 记得使用 sudo)
firewall-cmd --add-port=8080/tcp --permanent

# 重启防火墙
firewall-cmd –reload

然后就可以在 浏览器端访问 虚拟机的 IP 下的 8080 端口号，使得访问 tomcat
```

![image-20210921082156120](C:/Users/%E8%90%8C%E4%B8%BB/AppData/Roaming/Typora/typora-user-images/image-20210921082156120.png)

```
访问过程：使用  nginx 作为反向代理服务器，通过 nginx 去访问 tomcat
```

![image-20210921083648712](C:/Users/%E8%90%8C%E4%B8%BB/AppData/Roaming/Typora/typora-user-images/image-20210921083648712.png)

```
浏览器访问域名的原则：
	首先去 本地的 host 文件下去查找 有没有当前域名；如果没有。再去再去网络中寻找
	
	由于我们没有域名，所以我们可以在本地的 host 文件中去设置一下
```

![image-20210921084151361](C:/Users/%E8%90%8C%E4%B8%BB/AppData/Roaming/Typora/typora-user-images/image-20210921084151361.png)

```shell
加上自己的 IP 想要对应得域名
eg:
192.168.30.131 www.123.com

上述工作完成之后，就可以使用通过 www.123.com:8080 来访问 tomcat 了

我们还可以通过 nginx 配置, 通过 192.168.30.131:8080  跳转到  127.0.0.1:8080, ()

1: 前往 nginx 得配置文件下
cd usr/local/nginx/conf

2: 通过 vim 打开 nginx.conf
vi nginx.conf

3：在 nginx 进行请求转发的配置（反向代理配置）


server {
	listen 80
	server_name 192.168.30.131      #  将 localhost 改成我们得 IP:192.168.30.131
}



在 location / { }中修改; 会使 当我们访问的时 192.168.30.131:80 端口的话会将其转向：
http://127.0.0.1:8080


location / {
	root hmtl;
	proxy_pass http://127.0.0.1:8080;    # 需要添加的
	index index.html index.htm;
}
```

```
最后再启动 nginx ，进行测试，执行
cd usr/local/nginx/sbin
./nginx
```

![image-20210921090359794](C:/Users/%E8%90%8C%E4%B8%BB/AppData/Roaming/Typora/typora-user-images/image-20210921090359794.png)

### nginx 配置实例  -  反向代理2

```shell
1：需要实现的效果；
	使用 nginx 反向代理，根据访问的路径跳转到不同端口的服务中 nginx 监听端口为 9001，
	访问 http://192.168.17.129:9001/edu/ 直接跳转到 127.0.0.1:8080
	访问 http:// 192.168.17.129:9001/vod/ 直接跳转到 127.0.0.1:8081


2：准备工作：
	2.1：准备两个 tomcat 服务器，一个 8080 端口 ，一个 8081 端口；

2.1.1：首先找到之前的 tomcat 服务器 ，先停掉：
ps -ef | grep tomcat
kill -9 2982   # 这里的 2982 是其进程的 id 号,可以在执行完 ps -ef | grep tocmat 后看到


2.2.2：修改新安装的 tomcat 的端口号：
# 进入其解压后目录，进入 conf
cd conf

# 在进入 conf 的 server.xml 文件中修改：
vi server.xml

修改完成之后，启动即可
	
	
	2.2：准备 html 文件：
分别在 两个不同端口号的 tomcat 文件目录下的  webapps 中创建    edu ， vod　　文件夹；
在这两个文件夹下分别创建一个　hmtl 文件
```

```shell
# 前往 nginx 的中配置文件中进行修改：	
我们需要再添加一个 server 的块

server {
	listen      9001;
	server_name 192.168.30.131;
	
	location ~/edu/ {
		proxy_pass http://127.0.0.1:8080;
	}
	
	location ~/vod/ {
		proxy_pass http://127.0.0.1:8081;
	}
}

# 记得开放端口号
# 首先查看端口号有没有开放
firewall-cmd --list-all

# 开放对应端口号 (以 8081 为例)
firewall-cmd --add-service=8081/tcp --per

# 重启防火墙
firewall-cmd --reload


# 最后，重新启动 nginx
```

#### location  指令说明

```
该指令用于匹配 URL。

location [ =  | ~ | ~* | ^~ ] uri {
	
}

 1、= ：用于不含正则表达式的 uri 前，要求请求字符串与 uri 严格匹配，如果匹配
成功，就停止继续向下搜索并立即处理该请求。
 2、~：用于表示 uri 包含正则表达式，并且区分大小写。
 3、~*：用于表示 uri 包含正则表达式，并且不区分大小写。
 4、^~：用于不含正则表达式的 uri 前，要求 Nginx 服务器找到标识 uri 和请求字
符串匹配度最高的 location 后，立即使用此 location 处理请求，而不再使用 location 
块中的正则 uri 和请求字符串做匹配。


注意：如果 uri 包含正则表达式，则必须要有 ~ 或者 ~* 标识。
```

### nginx 配置实例 - 负载均衡

```shell
1: 实现效果：
	1：浏览器中输入  http://192.168.30.131/edu/a.hmtl ,负载均衡结果：
		平均到 8080 端口 和 8081 端口中。

2: 依然是准备 tomcat 服务器;
	在 webapps 中都有 edu文件夹，且其中有 a.html
	

3: 在 nginx 的配置文件中进行 负载均衡 的配置。
```

```shell
在 nginx.conf 中的配置。
在 http 块中进行添加。

upstream myserver {     # 其中包含需要将 负载 均衡的不同 tomcat 服务器反向代理后的网址
	server 192.168.30.131:8080;
	server 192.168.30.131:8081;
}

server {
	listen 80;
	server_name 192.168.30.131;
	
	location  / {
		proxy_pass http://myserver;    # myserver 就是 upstream 后的名字。
		root hmtl;
		index index.html index.htm;
	}
	
}

完成之后启动 nginx;
```

#### nginx 的负载均衡的一些默认带调度机制：

```shell
	1、轮询（默认）
每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器 down 掉，能自动剔除。



	2、weight
weight 代表权,重默认为 1,权重越高被分配的客户端越多
指定轮询几率，weight 和访问比率成正比，用于后端服务器性能不均的情况
eg:

upstream myserver {
	server 192.168.30.131 weight 10;
	server 192.168.30.131 weight 10;
}





	3、ip_hash
每个请求按访问 ip 的 hash 结果分配，这样每个访客固定访问一个后端服务器，
可以解决 session 的问题。 
eg:

upstream myserver {
	ip_hash;
	server 192.168.30.131;
	server 192.168.30.131;
}
	
	
	
	
	4：fair
按后端服务器的响应时间来分配请求，响应时间短的优先分配。
eg:

upstream myserver {
	server 192.168.30.131;
	server 192.168.30.131;
	fair;
}
```

### nginx 配置实例 - 动静分离

```
Nginx 动静分离简单来说就是把动态跟静态请求分开，不能理解成只是单纯的把动态页面和
静态页面物理分离。严格意义上说应该是动态请求跟静态请求分开，可以理解成使用 Nginx 
处理静态页面，Tomcat 处理动态页面。动静分离从目前实现角度来讲大致分为两种，

1：一种是纯粹把静态文件独立成单独的域名，放在独立的服务器上，也是目前主流推崇的方案；
2：另外一种方法就是动态跟静态文件混合在一起发布，通过 nginx 来分开。
```

<img src="C:/Users/%E8%90%8C%E4%B8%BB/AppData/Roaming/Typora/typora-user-images/image-20210921131924026.png" alt="image-20210921131924026" style="zoom:67%;" />

```
通过 location 指定不同的后缀名实现不同的请求转发。通过 expires 参数设置，可以使浏
览器缓存过期时间，减少与服务器之前的请求和流量。具体 Expires 定义：是给一个资源
设定一个过期时间，也就是说无需去服务端验证，直接通过浏览器自身确认是否过期即可，
所以不会产生额外的流量。此种方法非常适合不经常变动的资源。（如果经常更新的文件，
不建议使用 Expires 来缓存），我这里设置 3d，表示在这 3 天之内访问这个 URL，发送一
个请求，比对服务器该文件最后更新时间没有变化，则不会从服务器抓取，返回状态码 304，
如果有修改，则直接从服务器重新下载，返回状态码 200。
```

```shell
我们需要准备一些静态资源：
我们在   data 目录下新建的 image  和  www 文件夹，其中分别放着静态资源。 


然后进入 nginx.conf 中进行设置

我们需要配置的 location 

server {
	listen 80;
	server_name 192.168.30.131;     # 要注意 IP 的修改
	
	location /www/ {
		root /data/;
		index index.html index.htm;
	}
	location /image/ {
		root /data/;
		autoindex on;   # 表示列出当前文件夹中的内容
	}
	
	
}

# 测试：
http://192.168.30.131/image/01.jpg
```

<img src="C:/Users/%E8%90%8C%E4%B8%BB/AppData/Roaming/Typora/typora-user-images/image-20210921133756171.png" alt="image-20210921133756171" style="zoom:50%;" />



<img src="C:/Users/%E8%90%8C%E4%B8%BB/AppData/Roaming/Typora/typora-user-images/image-20210921133826918.png" alt="image-20210921133826918" style="zoom:50%;" />



### 通过 nginx 配置高可用的集群

```
因为我们使用的是 nginx 来做为中间层，但是 一旦 nginx 宕机，那么访问就会出现问题；
所以高可用就是为这一风险而存在；
```

![image-20210921160928829](C:/Users/%E8%90%8C%E4%B8%BB/AppData/Roaming/Typora/typora-user-images/image-20210921160928829.png)

```
使用 多台 nginx 作为 备用；
当 主 nginx 宕机之后，就会通过 备nginx 来进行反向代理等一系列操作；

需要使用   keepalived   软件进行检测；

由于两台  nginx 机 的 IP 不一致，所以我们必须对外保持一致的 IP ，即使用 虚拟 IP去访问 两台nginx 。

```

```shell
1：准备工作：
	1：两台服务器：  192.168.30.131    192.168.30.132  (两台虚拟机或着两台服务器)
	2：两台 服务器种都安装 nginx   和   keepalived
	
	
#  1.2 安装  keepalived
yum install keepalived -y

# 查看 keepalived 有没有安装成功
rpm -q -a keepalived


安装成功之后会在 etc 目录下有 keepalived 文件夹，其中会有 keepalived.conf 配置文件
```

```
1.3:
修改 etc/keepalived/keepalived.conf 配置文件
```

```shell
#  keepalived.conf  文件

global_defs {     					# 全局定义
	 notification_email {
	 	acassen@firewall.loc
 	 	failover@firewall.loc
		sysadmin@firewall.loc
 	 }
 	 notification_email_from Alexandre.Cassen@firewall.loc
	 smtp_server 192.168.17.129
  	 smtp_connect_timeout 30
  	 
 	 router_id LVS_DEVEL        # 是指可以访问到的 服务器 的名字
 	 
}


vrrp_script chk_http_port {						# 检测的脚本和一些参数
 	 script "/usr/local/src/nginx_check.sh"
 	 interval 2                                 #（检测脚本执行的间隔时间）
	 weight -20						# 表示当脚本中的条件成立，将主机的权重降低 20
}
vrrp_instance VI_1 {
 	 state BACKUP             # 备份服务器上将 MASTER 改为 BACKUP 
 	 
 	 interface ens33          #  网卡 -->  在 ifconfig 种查看
 	 
	 virtual_router_id 51     # 主、备机的 virtual_router_id 必须相同
	 
	 priority 90              # 主、备机取不同的优先级，主机值较大，备份机值较小
	 
	 advert_int 1             # 每个 1 秒检测当前主机是否还活着
	 
    authentication {		  # 校验方式 
         auth_type PASS
         auth_pass 1111
    }
    virtual_ipaddress {
         192.168.17.50         # VRRP H 虚拟地址   可以绑定多个  虚拟ip
    }
 
}
```

```
1.4:
在  /usr/local/src/   下添加  nginx_check.sh   检测脚本
```

```shell
# /usr/local/src/nginx_check.sh   脚本文件

#!/bin/bash
A=`ps -C nginx –no-header |wc -l`
if [ $A -eq 0 ];then
 	/usr/local/nginx/sbin/nginx
 	sleep 2
 	if [ `ps -C nginx --no-header |wc -l` -eq 0 ];then
 		killall keepalived
 	fi
fi
```

```shell
1.5
	分别启动  nginx   和   keepalived 

./nginx

systemctl start keepalived.service     #    开启  keepalived  服务

stop keepalived.service                #    关闭  keepalived  服务
```





```c++
global_defs {     	# 全局定义

 	 router_id LVS_DEVEL        # 是指可以访问到的 服务器 的名字	 
}
通过在  etc/host   文件中配置 服务器对应的名字
```

![image-20210921174922215](C:/Users/%E8%90%8C%E4%B8%BB/AppData/Roaming/Typora/typora-user-images/image-20210921174922215.png)







### nginx  原理解析

```
nginx 启动之后就会产生来两种进程；
	master:
	worker:  (不止一个)
```

<img src="C:/Users/%E8%90%8C%E4%B8%BB/AppData/Roaming/Typora/typora-user-images/image-20210921180600689.png" alt="image-20210921180600689" style="zoom:50%;" />

```
worker 是如何工作的？
	master  发送任务之后就会有  worker 进程 争抢 任务，然后通过 tomcat 反向代理工作



	master-workers 的机制的好处 

首先，对于每个 worker 进程来说，独立的进程，不需要加锁，所以省掉了锁带来的开销，
同时在编程以及问题查找时，也会方便很多。其次，采用独立的进程，可以让互相之间不会
影响，一个进程退出后，其它进程还在工作，服务不会中断，master 进程则很快启动新的
worker 进程。当然，worker 进程的异常退出，肯定是程序有 bug 了，异常退出，会导致当
前 worker 上的所有请求失败，不过不会影响到所有请求，所以降低了风险。
```

<img src="C:/Users/%E8%90%8C%E4%B8%BB/AppData/Roaming/Typora/typora-user-images/image-20210921180648029.png" alt="image-20210921180648029" style="zoom:50%;" />

```shell

	#   1.1   一个 master 和多个 woker 有好处
（1）可以使用 nginx –s reload 热部署，利用 nginx 进行热部署操作
（2）每个 woker 是独立的进程，如果有其中的一个 woker 出现问题，其他 woker 独立的，
继续进行争抢，实现请求过程，不会造成服务中断


	#  1.2   设置多少个 woker 合适
Nginx 同 redis 类似都采用了 io 多路复用机制，每个 worker 都是一个独立的进程，但每个进
程里只有一个主线程，通过异步非阻塞的方式来处理请求， 即使是千上万个请求也不在话
下。每个 worker 的线程可以把一个 cpu 的性能发挥到极致。所以 worker 数和服务器的 cpu
数相等是最为适宜的。设少了会浪费 cpu，设多了会造成 cpu 频繁切换上下文带来的损耗。


	#   1.3  连接数 worker_connection
这个值是表示每个 worker 进程所能建立连接的最大值，


第一个：发送请求，占用了 woker 的几个连接数？
	答案：2 或者 4 个
这里的 2 就是只到 nginx 中去去请求静态资源，4 个就是还要再通过  worker 去请求 tomcat 请求
d
	
第二个：nginx 有一个 master，有四个 woker，每个 woker 支持最大的连接数 1024，支持的
最大并发数是多少？

 普通的静态访问最大并发数是： worker_connections * worker_processes /2，
 而如果是 HTTP 作 为反向代理来说，最大并发数量应该是 worker_connections * 
worker_processes/4

```
























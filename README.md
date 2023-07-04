# GenchOJ部署指南
<br> 

[![Python](https://img.shields.io/badge/python-3.9.0-success.svg?style=flat-round)](https://www.python.org/downloads/release/python-372/)
[![Django Rest Framework](https://img.shields.io/badge/django_rest_framework-3.9.1-success.svg?style=flat-round)](http://www.django-rest-framework.org/)
[![vuex](https://img.shields.io/badge/vuex-3.1.0-success.svg?style=flat-round)](https://vuex.vuejs.org/)
<br> 

- OJ简介
    - GenchOJ是基于vue3+Django技术栈开发的online judge系统，系统由前端，后端，judger端，爬虫服务四部分组成。其中，judger端采用了Hust OJ的开源技术，其余部分由GenchOJ项目组自行完成或补充。

    - [项目地址](https://github.com/AlizeLiu/GenchOJ)


## 简述
<br> 
- 轻量级，易于部署和自定义定制
- 前后端分离，提高服务器性能
- 支持多机器多进程判题，判题更高效
- 支持 C/C++/Java/Python2/Python3和Swift5.1语言
- 支持 Special Judge和选择题判题
- 丰富的API，开放的源代码

<br> <br> 
## 环境说明

- 判题机必须部署在linux环境下，如需要在windows环境下部署需要自行解决可能出现的问题。

### 本项目的试验环境为：
<br> 

- 前端： Ubuntu 22.04 + Nginx
- 后端： Ubuntu 22.04 + Python 3.9
- 判题服务器： Ubuntu 22.04 + Python 3.9
- 判题机： Ubuntu 22.04 + Python 3.9 （必须Linux系统）
- 爬虫部分： Ubuntu 22.04 + Python 3.9

## 部署说明
<br> 
1. 首先从git上clone代码，准备进行部署。
```
    mkdir GenchOJ
    cd GenchOJ 
    git clone https://github.com/AlizeLiu/GenchOJ.git
```

<br> 

2. 前端部署 

```
cd Frontend
npm install
npm run build
```

<br> 

- 等待npm编译前端，编译成功后可以在前端中的dist文件夹找到编译成功后的静态文件。

- 生产环境使用的服务器建议为nginx，一下教程均基于nginx进行

- 如工具使用VSCode，需要自行解决自动下载最新版本依赖导致的依赖冲突

<br> 
<br> 
    

```
sudo apt-get install nginx
sudo nano /etc/nginx/nginx.conf
```
<br> 
- 然后需要修改以下配置：

1. 路由重定向
2. API重定向
<br>

```
server{
    listen 80;
    server_name ;  # 此处填写你的域名或IP
    root /var/www/html;   # 此处填写你的网页根目录
    location /api {  # 将API重定向到后台服务器
        rewrite ^.*api/?(.*)$ /$1 break;
        proxy_pass http://localhost:8000; # 填写你的后端地址和端口
    }
    location / {  # 路由重定向以适应Vue中的路由
        index index.html;
        try_files $uri $uri/ /index.html;
    }
}
```

⚠️ 部署时前端端口为8080，后端端口为8000，仅在第一次设置管理员时需要打开8000端口，使用时均为localhost：8080端口
 
 <br>

 随后重启服务 
 ```
 sudo systemctl restart nginx
 ``` 
 <br>

 以上部分完成后，应当可以在浏览器中看到渲染的前端页面，可以进入8080端口进行查看。

<br>

3. 后端和数据库部署

<br>

```
sudo apt-get install mysql-server
CREATE DATABASE LPOJ DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
```

以上操作建立mysql数据库。

<br>

```
pip install django
pip install djangorestframework
pip install django-filter
sudo apt-get install python-django
pip install django-cors-headers
pip install mysqlclient
```
<br>


安装完后端依赖后，进行后端部署 

```
cd Backend

cd Backend

sudo nano setting.py
```

在页面中修改数据库配置为自己服务器的IP端口和密码

<br>

### 添加管理员

- 安装成功后，先通过IP:8080访问OJ，注册一个用户

- 进入 IP:8000/admin 以用户名admin 密码admin 登录后台（请及时修改后台密码）

- 修改User表中，你注册的超级用户的type为3，使得你注册的用户变为超级管理员

<br>

4. Judger端部署

- 首先修改配置文件

```
cd JudgeServer
nano setting.json
```
修改对应的数据库IP和端口，用户名和密码保存退出

<br>

```
sudo apt-get install libmysqlclient-dev

pip install mysqlclient
```

最后运行
```
sudo python main.py
```
<br>


判题机支持多个且异地部署

#### 一般部署

- 修改配置文件

```
cd Judger
nano setting.json
```

- 修改对应的数据库IP和端口，用户名和密码

- 修改server_ip为你的判题服务器的IP地址

```
pip install mysqlclient      //安装依赖库

sudo apt-get install libseccomp-dev
mkdir build && cd build && cmake .. && make && sudo make install
cd ..
cd JudgerCore
sudo python setup.py install
cd ..
pip install paramiko
sudo apt install time
```


安装Java环境
```
sudo apt install openjdk-8-jdk
```

最后运行
```
sudo python main.py
```

<br>
 
 以上，所有的部署已经结束，在浏览器中应当可以使用完整的功能。












# Frps
Frp内网穿透服务端的简单配置

## 介绍
Frp是一款内网穿透的工具，这里我只记录Frps的简单配置，也就是服务端。

## 使用方法
1. 安装Frps。
~~~
wget https://github.com/fatedier/frp/releases/download/v0.35.1/frp_0.35.1_linux_amd64.tar.gz && tar -zxvf frp_0.35.1_linux_amd64.tar.gz
vi frps.ini
mv fprs.ini /root
~~~

2. 进入Frps的压缩后的目录下，赋予权限，并且放到/usr/bin目录下
~~~
cd frp_0.35.1_linux_amd64
chmod +x frps
mv frps /usr/bin
~~~

3. 编辑Frps的配置文件。
~~~
[common]
bind_port = 7000
dashboard_port = 7500
token = HelloWorld
dashboard_user = ***
dashboard_pwd = ***
vhost_http_port = 80
vhost_https_port = 443
subdomain_host = kyun.pro
~~~
修改为你自己的域名，并且记录下你设置的Token，在配置客户端的时候会用到。dashboard_user和dashboard_pwd修改为你常用的即可，在登陆dashboard的时候会要求你输入。

4. 配置Frps的进程守护。（我介绍的进程守护的方式）
~~~
cd /usr/lib/systemd/system
vi frps.service
~~~

在frps.service文件中填入以下内容
~~~
[Unit]
# 服务描述
Description=crontab master
# 在网络初始化之后启动
After=network.target

[Service]
# 服务类型
Type=simple
# 进程退出立即重启
Restart=always


# 工作目录
WoringDirectory=/usr/bin/frps
# 启动命令
ExecStart=/usr/bin/frps -c /root/frps.ini

[Install]
# 当系统以多用户方式启动时，这个服务需要被自动运行
WantedBy=multi-user.target
~~~

5. 开启Frps并设置为开机自启动
~~~
systemctl daemon-reload
systemctl start frps.service
systemctl enable frps.service
~~~

6. 检查Frps运行状态
~~~
systemctl status frps.service
~~~

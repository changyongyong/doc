# CentOS6.6安装ShadowSocks服务端

## 查看系统
```
cat /etc/issue
```
CentOS release 6.6 (Final)
```
uname -a
```
Linux localhost.localdomain 2.6.32-042stab106.6 #1 SMP Mon Apr 20 14:48:47 MSK 2015 x86_64 x86_64 x86_64 GNU/Linux

## 安装ShadowSocks  
``` bash
# yum install python-setuptools && easy_install pip
# pip install shadowsocks
```

## 创建配置文件/etc/shadowsocks.json
```
touch /etc/shadowsocks.json    
vi /etc/shadowsocks.json
```    
{    
"server":"159.203.180.134",    
"server_port":443,    
"local_address": "127.0.0.1",     
"local_port":1080,   
"password":"MDBkYjQ0OG",   
"timeout":600,    
"method":"rc4-md5"   
}    

备注：加密方式官方默认使用aes-256-cfb，推荐使用rc4-md5，因为 RC4比AES速度快好几倍。
各字段说明：
    server:服务器IP
    server_port:服务器端口
    local_port:本地端端口
    password:用来加密的密码
    timeout:超时时间（秒）
    method:加密方法，可选择 “bf-cfb”, “aes-256-cfb”, “des-cfb”, “rc4″等

## 使用配置文件在后台运行shadowsocks服务

```
ssserver -c /etc/shadowsocks.json -d start
```
 
备注：若无配置文件，在后台可以使用一下命令运行：
```
ssserver -p 443 -k MyPass -m rc4-md5 -d start
```
 
## 停止服务
 	
```
ssserver -c /etc/shadowsocks.json -d stop
```
参考：
https://github.com/shadowsocks/shadowsocks/wiki/Shadowsocks-使用说明
http://wuchong.me/blog/2015/02/02/shadowsocks-install-and-optimize/




# 在 CentOS 7 下安装配置 shadowsocks

在控制台执行以下命令安装 pip：    
```
curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"    
python get-pip.py
```

在控制台执行以下命令安装 shadowsocks：
```
pip install --upgrade pip     
pip install shadowsocks
```

安装完成后，需要创建配置文件/etc/shadowsocks.json，内容如下：
{
  "server": "0.0.0.0",
  "server_port": 8388,
  "password": "uzon57jd0v869t7w",
  "method": "aes-256-cfb"
}

新建启动脚本文件/etc/systemd/system/shadowsocks.service，内容如下：
[Unit]
Description=Shadowsocks

[Service]
TimeoutStartSec=0
ExecStart=/usr/bin/ssserver -c /etc/shadowsocks.json

[Install]
WantedBy=multi-user.target

执行以下命令启动 shadowsocks 服务：
systemctl enable shadowsocks
systemctl start shadowsocks

为了检查 shadowsocks 服务是否已成功启动，可以执行以下命令查看服务的状态：
systemctl status shadowsocks -l

一键安装脚本
新建文件install-shadowsocks.sh，内容如下：

#!/bin/bash
# Install Shadowsocks on CentOS 7

echo "Installing Shadowsocks..."

random-string()
{
    cat /dev/urandom | tr -dc 'a-zA-Z0-9' | fold -w ${1:-32} | head -n 1
}

CONFIG_FILE=/etc/shadowsocks.json
SERVICE_FILE=/etc/systemd/system/shadowsocks.service
SS_PASSWORD=$(random-string 32)
SS_PORT=8388
SS_METHOD=aes-256-cfb
SS_IP=`ip route get 1 | awk '{print $NF;exit}'`
GET_PIP_FILE=/tmp/get-pip.py

# install pip
curl "https://bootstrap.pypa.io/get-pip.py" -o "${GET_PIP_FILE}"
python ${GET_PIP_FILE}

# install shadowsocks
pip install --upgrade pip
pip install shadowsocks

# create shadowsocls config
cat <<EOF | sudo tee ${CONFIG_FILE}
{
  "server": "0.0.0.0",
  "server_port": ${SS_PORT},
  "password": "${SS_PASSWORD}",
  "method": "${SS_METHOD}"
}
EOF

# create service
cat <<EOF | sudo tee ${SERVICE_FILE}
[Unit]
Description=Shadowsocks

[Service]
TimeoutStartSec=0
ExecStart=/usr/bin/ssserver -c ${CONFIG_FILE}

[Install]
WantedBy=multi-user.target
EOF

# start service
systemctl enable shadowsocks
systemctl start shadowsocks

# view service status
sleep 5
systemctl status shadowsocks -l

echo "================================"
echo ""
echo "Congratulations! Shadowsocks has been installed on your system."
echo "You shadowsocks connection info:"
echo "--------------------------------"
echo "server:      ${SS_IP}"
echo "server_port: ${SS_PORT}"
echo "password:    ${SS_PASSWORD}"
echo "method:      ${SS_METHOD}"
echo "--------------------------------"

执行以下命令一键安装：

```
chmod +x install-shadowsocks.sh
./install-shadowsocks.sh
```

也可以直接执行以下命令从 GitHub 下载安装脚本并执行：
```
bash <(curl -s http://morning.work/examples/2015-12/install-shadowsocks.sh)
```




# nginx教程

mac pro m1环境下的教程，

## 1. 安装（可以用 brew 安装）
```
sudo brew install nginx    // 安装
brew info nginx    // 查看nginx信息
```

## 2. 查看 nginx 版本

```
nginx -v
```

## 3. 启动 

```
nginx
```

## 4.重启

```
nginx -s reload
```

## 5.常用命令

```
sudo nginx 启动服务
sudo nginx -s reload  重新加载
sudo nginx -s reopen 重新启动
sudo nginx -s quit 退出（处理完事情走）
open /opt/homebrew/etc/nginx/ 查看nginx安装目录

```

# 6.配置文件
```
1.网站存放路径
/opt/homebrew/var/www
2.Nginx配置文件路径
/opt/homebrew/etc/nginx/nginx.conf
3.服务配置文件
/opt/homebrew/etc/nginx/servers/
4.后台进程守护，如果您不想要/不需要后台服务，您可以运行：
/opt/homebrew/opt/nginx/bin/nginx -g守护进程关闭；
```





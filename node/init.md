# MAC 安装NODE、NPM，以及修改淘宝镜像

## 概述
### 安装node
http://nodejs.cn/download/

### 终端验证是否成功
```
node -v
npm -v
```

## 配置环境变量
```
vim .bash_profile  
PATH=$PATH:/usr/local/bin/
:wq  //保存并退出
source ～/.bash_profile //保存修改
echo $PATH //查看是否生效
```

## 将原始镜像为淘宝镜像
```
$ npm get registry #查看原本镜像
$ npm config set registry http://registry.npm.taobao.org/ #修改成淘宝镜像
$ npm config set registry https://registry.npmjs.org/ #镜像还原
```

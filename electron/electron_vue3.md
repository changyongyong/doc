# [ Vue | Electron ] 使用 vue-cli3.x 快速构建 electron 项目
重要

此方法仅用于 Vue-CLI 3 (vue create my-app) 创建的项目，不适用 Vue-CLI 2 (vue init webpack my-app)!

如果想使用 Vue-CLI 2 ，可以参考 electron-vue

## 使用 Vue CLI3 创建项目("my-project" 改为自己想要取是项目名称)
```
vue create my-project
```

## 进入项目文件夹，添加插件 vue-cli-plugin-electron-builder
```
vue add electron-builder
```

## 当自动化脚本执行完毕后，使用开发模式运行
```
npm run electron:serve
```

## 准备部署时，可以使用
```
npm run electron:build
```

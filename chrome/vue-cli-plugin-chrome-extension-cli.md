# vue-cli-plugin-chrome-extension-cli


使用 Vue-CLI 轻松建构 chrome 扩展插件

<p align="center"><a href="https://vuejs.org" target="_blank" rel="noopener noreferrer"><img width="100" src="https://github.com/sanyu1225/vue-cli-plugin-chrome-extension-cli/raw/main/logo.png" alt="Vue logo"></a></p>

<p align="center">
  <a href="https://www.npmjs.com/package/vue-cli-plugin-chrome-extension-cli"><img src="https://img.shields.io/github/package-json/v/sanyu1225/vue-cli-plugin-chrome-extension-cli" alt="Version"></a>
  <a href="https://www.npmjs.com/package/vue-cli-plugin-chrome-extension-cli"><img src="https://img.shields.io/github/license/sanyu1225/vue-cli-plugin-chrome-extension-cli" alt="License"></a>
</p>

支援 vue2 vue3 TypeScript 跟 JavaScript!
## 版本要求
vue-cli 5.0.1 或更高 
## 安裝

该插件用于将新项目用于 chrome 扩展。

![](https://sanyu1225.github.io/images/shell.gif)

## 使用方法?

```
vue create <project-name>
cd <project-name>
vue add chrome-extension-cli
```

## 资料夹结构

```
.
├── public
│   ├──  can set image.
├── src/
│   ├── assets
│   │   └── Static assets
│   ├── entry
│   │   ├── options.js
│   │   ├── popup.js
|   |   ├── devtools.js
│   │   ├── content.js
│   │   └── background.js
│   └── view
│   │   ├── popup.vue
│   │   ├── options.vue
|   |   └── devtools.vue
│   ├── manifest.development.json
│   └── manifest.production.json
└── vue.config.js
```

### 本地开发 跟 生产模式

- 使用` npm run build-watch`运行开发模式，将生成一个`dist`文件。 安装[Extension Reloader](https://chrome.google.com/webstore/detail/extensions-reloader/fimgfedafeadlieiabdeeaodndnlbhid)，以便在热更新。 （注意，当您更改 manifest.json 文件时，它不会自动加载，您需要点选 extension 页面中的更新）
- 生产模式 `npm run build`，并将其压缩成 zip 并部署到 chrome 商店中。

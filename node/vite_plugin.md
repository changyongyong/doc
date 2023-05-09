# vite插件

## 1、 插件钩子

开发时，vite dev server 创建一个插件容器，按照Rollup调用创建钩子的规则请求各个钩子函数

在服务启动时调用一次：
```
optipns  替换或操控rollup选项（只在打包时有用，开发时为空）
buildStart 开始创建（只是一个信号）
```

每次有模块请求时都会调用：
```
resolveId 创建自定义确认函数，常用语句定位第三方依赖
load 创建自定义加载函数，可用于返回自定义的内容
transform 可用于装换已加载的模块内容
```

在服务器关闭时调用一次：
```
buildEnd
closeBundle
```

vite 特有钩子
```
config：修改Vite配置（可以配置别名）
configResolved ：vite配置确认
configureServer：用于配置dev server
transformIndexHtml: 用于转换宿主页（可以注入或者删除内容）
handleHotUpdate：自定义HMR更新时调用
```

## 2、钩子调用顺序
```
config → configResolved → optipns → configureServer → buildStart → transform → load → resolveId → transformIndexHtml → vite dev server
```

## 3、插件顺序
```
别名处理 Alias
用户插件执行（如果设置 enforce : ‘pre’）
Vite 核心插件(plugin-vue)
用户插件执行（如果未设置 enforce）
Vite 构建插件
用户插件执行（如果设置 enforce : ‘post’）
Vite 构建后置插件
```

## 4、实现一个mock服务器—vite-plugin-mock

实现思路：

给开发服务器实例（concent）配置一个中间件，这个中间件可以存储用户配置接口映射信息，并提前处理输入请求，如果请求的url和路由表匹配则接管，按用户配置的handler返回结果

```
import path from "path";

let mockRouteMap = {};

function matchRoute(req) {
  let url = req.url;
  let method = req.method.toLowerCase();
  let routeList = mockRouteMap[method];

  return routeList && routeList.find((item) => item.path === url);
}

// 默认导出的插件工厂函数
export default function (options = {}) {
  // 获取mock文件入口，默认是index
  options.entry = options.entry || "./mock/index.js";
  // 转换为绝对路径
  if (!path.isAbsolute(options.entry)) {
    options.entry = path.resolve(process.cwd(), options.entry);
  }
  // 返回的插件
  return {
    configureServer: function ({ app }) {
      // 定义路由表
      const mockObj = require(options.entry);
      // 创建路由表
      createRoute(mockObj);

      // 定义中间件：路由匹配
      const middleware = (req, res, next) => {
        // 1.执行匹配过程
        let route = matchRoute(req);
        //2. 存在匹配，是一个mock请求
        if (route) {
          console.log("mock req", route.method, route.path);
          res.send = send;
          route.handler(req, res);
        } else {
          next();
        }
      };

      // 最终目标，给app注册一个中间件
      app.use(middleware);
    },
  };
}

function createRoute(mockConfList) {
  mockConfList.forEach((mockConf) => {
    let method = mockConf.method || "get";
    let path = mockConf.url;
    let handler = mockConf.response;
    // 路由对象
    let route = { path, method: method.toLowerCase(), handler };
    if (!mockRouteMap[method]) {
      mockRouteMap[method] = [];
    }
    console.log("create mock api");
    // 存入映射对象中
    mockRouteMap[method].push(route);
  });
}

// 实现一个send方法
function send(body) {
  let chunk = JSON.stringify(body);
  if (chunk) {
    chunk = Buffer.from(chunk, "utf-8");
    this.setHeader("Content-Length", chunk.length);
  }
  this.setHeader("Content-Type", "application/json");
  this.statusCode = 200;
  this.end(chunk, "utf8");
}
```


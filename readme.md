#### 埋点是啥

埋点就是 数据采集-数据处理-数据分析和挖掘，如用户停留时间，用户哪个按钮点的多，等

#### 技术架构

技术架构使用 ts + rollup
使用 ts 主要是在编译过程中发现问题，减少生产代码的错误，
使用 rollup 应为 rollup 打包干净，而 webpack 非常臃肿，可读性差，所以 rollup 非常适合开发 SDK 和一些框架，webpack 适合开发一些项目
:blush:

#### 依赖安装

npm install rollup -D
npm install rollup-plugin-dts -D
npm install rollup-plugin-typescript2 -D
npm install typescript -D

#### 配置 rollup config

```js
import ts from 'rollup-plugin-typescript2'
import path from 'path'
import dts from 'rollup-plugin-dts';
export default [{
//入口文件
input: "./src/core/index.ts",
output: [
//打包 esModule
{
file: path.resolve(**dirname, './dist/index.esm.js'),
format: "es"
},
//打包 common js
{
file: path.resolve(**dirname, './dist/index.cjs.js'),
format: "cjs"
},
//打包 AMD CMD UMD
{
input: "./src/core/index.ts",
file: path.resolve(\_\_dirname, './dist/index.js'),
format: "umd",
name: "tracker"
}

    ],
    //配置ts
    plugins: [
        ts(),
    ]

}, {
//打包声明文件
input: "./src/core/index.ts",
output:{
file: path.resolve(\_\_dirname, './dist/index.d.ts'),
format: "es",
},
plugins: [dts()]
}]
```

#### 核心功能

PV：页面访问量，即 PageView，用户每次对网站的访问均被记录
主要监听了 history 和 hash
history API go back forward pushState replaceState
history 无法通过 popstate 监听 pushState replaceState 只能重写其函数 在 utils/pv
hash 使用 hashchange 监听

UV(独立访客)：即 Unique Visitor，访问您网站的一台电脑客户端为一个访客
用户唯一表示 可以在登录之后通过接口返回的 id 进行设置值 提供了 setUserId
也可以使用 canvas 指纹追踪技术

使用 navigator.sendBeacon 这个去上报
这个上报的机制 跟 XMLHttrequest 对比 navigator.sendBeacon 即使页面关闭了 也会完成请求 而 XMLHTTPRequest 不一定

DOM 事件监听
主要是给需要监听的元素添加一个属性 用来区分是否需要监听 target-key

js 报错上报 error 事件 promise 报错 unhandledrejection

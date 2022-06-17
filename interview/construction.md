# 10 项目构建


webpack构建的原理

初始化配置参数，创建 compiler编译对象；

从入口文件递归的去加载依赖文件，通过对应不同文件的loader去解析编译转换，形成转换后的依赖加载关系块；

通过插件plugin在构建各个阶段的钩子，对chunk进行操作处理；

最后生成资源输出文件。

一文吃透 Webpack 核心原理 https://zhuanlan.zhihu.com/p/363928061


webpack的优化

构建速度

避免重复编译：exclude 配置去掉已编译过的模块、cache-loader 缓存编译的模块

区分环境处理：在开发环境下可以减少一些非必要的构建工作，比如 模块文件的压缩、计算 hash、提取模块

多进程打包：使用 thread-loader配置启用多进程loader编译执行

体积、访问速度优化

import 懒加载模块；使用文件哈希、版本好做缓存

js、css的文件压缩（代码格式、注释）；图片的压缩

拆分代码块，达到公共模块的缓存、运行时按需加载chunk

利用 esModule 的结构加载模块，做tree-shaking 减少体积


Vite

基于webpack的vue-cli启动需要，先加载编译入口及所有依赖，耗时比较长

vite启动，则是先启动了一个资源服务的service，通过esbuild预编译npm的依赖包；接收浏览器http请求依赖再去按需编译响应

基于esbuild编译，内置了html、css等预处理器，兼容 Rollup 的插件机制与 API


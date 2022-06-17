# 06 性能优化

2018 前端性能优化清单 译  https://juejin.cn/post/6844903568130965517
优化思维导图  https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2018/2/28/161db9df64a2f3c1~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp

https://juejin.cn/post/6911472693405548557



资源优化

从减小文件大小、缓存等方面提高资源访问速度

构建
* Js、Css文件压缩：删除源码中的代码格式、注释
* Js tree-shaking：利用es module的模块引用删除的无用
* Js、Css Code-splitting：对代码拆分成多个chunk块，可以按需加载、减少重复代码
* 根据文件内容生成对应的哈希值文件名

服务端
* 打开资源gzip压缩配置
* 使用CDN节点缓存资源
* 对没什么变动的资源强缓存，对其他启用协商缓存
* 减少cookie传输数据
* 使用 webp 格式的图片
* 使用svg字体图标减少请求

浏览器
* 懒加载一些可视范围外的资源
* 用preload 预加载下一个页面的资源
* dns-prefetch DNS预解析
* 做服务端渲染


运行时优化

* 减少首屏渲染js的执行阻塞
* 时间切片：利用渲染刷新频率，拆开分段执行大量的js任务，提高界面的渲染流畅度和用户响应速度
* 防止内存泄漏：及时释放闭包内存、合理复用变量内存、移除无用的监听器
* 通过最优算法减少js执行的时间复杂度
* 利用防抖、节流的方案控制连续触发


对webpack 做什么优化、webpack的构建原理

node 守护进程  node创建子进程
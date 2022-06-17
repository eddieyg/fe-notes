# 07 框架

Vue响应性原理

Vue3 通过 Proxy代理目标对象，对变更属性值时触发变更通知；在副作用中（计算属性、render函数等）收集依赖的属性，在触发变更后去再次执行副作用，以获得最新的结果。

Vue2 通过 直接重写对象的get、set 方法拦截监听更新。

重写get/set 在初始化时需遍历递归设置，Proxy则只在有读取时去处理 


Vue渲染机制

通过渲染函数对元素及子级创建对象，形成虚拟DOM树。

监测到数据变更后，把更新推入队列（对异步队列尝试 Promise.then、MutationObserver 和 setImmediate、setTimeout(fn, 0) ）；

在同步代码执行完后，对队列更新合并，与实际的DOM执行diff对比，批量的更新变更的元素。


虚拟DOM、diff算法
https://zhuanlan.zhihu.com/p/281031340
深度剖析：如何实现一个 Virtual DOM 算法 · Issue #13 · livoras/blog · GitHub
Diff算法核心原理  https://juejin.cn/post/6994959998283907102

背景：当状态变更的时候，更新到DOM上；如果 全部重新更新会造成性能浪费，如果 局部更新又会存在同时局部更新过多而阻塞。（跨平台）

1 用对象描述DOM树的结构（节点唯一标识），然后把对象树构建成DOM树，插入文档
2 当状态变更时，重新生成新的对象树，与旧对象树对比出差异
3 将差异根据1生成DOM树更新

diff https://github.com/livoras/list-diff/blob/master/lib/diff.js
精读《DOM diff 原理详解》

 O(n³)算法有跨层比较，但实际项目中很少跨层移动；所以降低时间复杂度为O(n) 只做同层的比较，同层要计算最小步数位移，达到最高效


Vue hash 路由和 history 路由的区别


Vue3的区别

Diff: 事件缓存、静态标记、静态提升

响应式的实现

生命周期 setup

全局API 按需加载

响应性、组合式API

Teleport 组件挂载到指定目标节点

具名插槽的写法改变

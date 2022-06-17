# 05 浏览器

进程线程

浏览器由主进程通过IPC通信协调其他渲染进程（每个Tab页）组成；
进程是独立运行的，系统会为进程分配内存，进程下的线程共享内存；
线程是同步执行的，处理各自负责的事情；
Tab页中的iframe用单独的进程渲染；

图解浏览器的基本工作原理  https://zhuanlan.zhihu.com/p/47407398


地址栏链接到渲染完成过程

* UI进程传给网络进程对域名DNS解析到IP，然后请求获取http报文数据；
* 根据http content-type 判断类型，文件类型交由其他进程下载，html类型则新建一个渲染进程渲染；
* 渲染进程解析html构建DOM树，遇到外链资源等标签则去网络请求；
* 解析到js会同步的加载并执行，async会异步的加载后执行，defer 会异步的加载在onload执行；
* Css加载会阻塞渲染、js代码执行
* 对解析的选择器样式CSSOM 树与DOM树计算合成渲染树；
* 布局：计算渲染树布局位置信息；
* 绘制：遍历布局树以创建绘制记录；
* 合成层后交由GPU 渲染；


事件模型

事件的传播：
* 捕获：从window对象 由外向内 到目标对象
* 目标：到达目标对象
* 冒泡：从目标对象 由内向外 到window对象
其中某个节点执行 event.stopPropagation() 则终止事件的传播

定义监听函数的方式：
* 在HTML标签上设置on-xx 属性，冒泡阶段触发，监听函数最多存在一个
* 在DOM节点对象 el.onxx 赋值监听函数，冒泡阶段触发，监听函数最多存在一个
* 在DOM节点对象 el.addEventListener 方法监听函数，通过第三参数默认false为冒泡、true为捕获触发，可设置多个监听函数

事件委托：
利用事件的冒泡阶段，在父节点监听函数对子节点做事件处理
应对子节点动态变化、减少重复事件监听


事件循环

* 执行栈上执行全局代码块，异步http请求响应后、异步事件监听、定时器结束 将回调推进宏任务队列
* promise resolve后将then回调推入微任务队列
* 当执行栈代码执行完成后，从微任务队列取一个到执行栈执行，直到微任务队列情况为止
* requestAnimationFrame回调函数在渲染之前执行
* GUI渲染
* 从宏任务队列取一个到执行栈执行，继续循环直到取完

Event loop 和 JS 引擎、渲染引擎的关系(精致版) https://zhuanlan.zhihu.com/p/371786505


Nodejs事件循环机制

* 执行栈运行输入代码
    * 执行 process.nextTick 回调
    * 执行 microtasks（promise）
* 进入 timers 阶段
    * 执行 timer 队列到期的 timer 回调
    * 检查是否有 process.nextTick 任务，如果有，全部执行
    * 检查是否有microtask，如果有，全部执行
    * 退出该阶段。
* 进入IO callbacks阶段
    * 执行 pending 的 I/O 回调
    * 检查是否有 process.nextTick 任务，如果有，全部执行
    * 检查是否有microtask，如果有，全部执行
    * 退出该阶段。
* 进入 idle，prepare 阶段
    * 仅系统内部使用
* 进入 poll 阶段
    * 如果有可用回调（可用回调包含到期的定时器还有一些IO事件等）
        * 执行所有可用回调
        * 检查是否有 process.nextTick 任务，如果有，全部执行
        * 检查是否有microtask，如果有，全部执行
        * 退出该阶段
    * 如果没有可用回调
        * 检查是否有 setImmediate 回调，如果有，退出 poll 阶段。如果没有，阻塞在此阶段，等待新的事件通知
        * 如果不存在尚未完成的回调，退出poll阶段
* 进入 check 阶段
    * 执行所有setImmediate回调
    * 检查是否有 process.nextTick 回调，如果有，全部执行
    * 检查是否有 microtaks，如果有，全部执行
    * 退出 check 阶段
* 进入 closing 阶段。
    * 执行所有close(socket.destroy())的回调
    * 检查是否有 process.nextTick 回调，如果有，全部执行
    * 检查是否有 microtaks，如果有，全部执行
    * 退出 closing 阶段
* 检查是否有活跃的 handles（定时器、IO等事件句柄）
    * 如果有，继续下一轮循环
    * 如果没有，结束事件循环，退出程序
Node文档：事件循环



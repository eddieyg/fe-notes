# Node
  basics...

## Nodejs事件循环机制

- 执行栈运行输入代码  
  - 执行 process.nextTick 回调
  - 执行 microtasks（promise）

- 进入 timers 阶段
  - 执行 timer 队列到期的 timer 回调
  - 检查是否有 process.nextTick 任务，如果有，全部执行
  - 检查是否有microtask，如果有，全部执行
  - 退出该阶段。

- 进入IO callbacks阶段
  - 执行 pending 的 I/O 回调
  - 检查是否有 process.nextTick 任务，如果有，全部执行
  - 检查是否有microtask，如果有，全部执行
  - 退出该阶段。

- 进入 idle，prepare 阶段
  - 仅系统内部使用

- 进入 poll 阶段
  - 如果有可用回调（可用回调包含到期的定时器还有一些IO事件等）
    - 执行所有可用回调
    - 检查是否有 process.nextTick 任务，如果有，全部执行
    - 检查是否有microtask，如果有，全部执行
    - 退出该阶段
  - 如果没有可用回调
    - 检查是否有 setImmediate 回调，如果有，退出 poll 阶段。如果没有，阻塞在此阶段，等待新的事件通知
    - 如果不存在尚未完成的回调，退出poll阶段

- 进入 check 阶段
  - 执行所有setImmediate回调
  - 检查是否有 process.nextTick 回调，如果有，全部执行
  - 检查是否有 microtaks，如果有，全部执行
  - 退出 check 阶段

- 进入 closing 阶段。
  - 执行所有close(socket.destroy())的回调
  - 检查是否有 process.nextTick 回调，如果有，全部执行
  - 检查是否有 microtaks，如果有，全部执行
  - 退出 closing 阶段

- 检查是否有活跃的 handles（定时器、IO等事件句柄）
  - 如果有，继续下一轮循环
  - 如果没有，结束事件循环，退出程序

[Node文档：事件循环](https://nodejs.org/zh-cn/docs/guides/event-loop-timers-and-nexttick/#what-is-the-event-loop)
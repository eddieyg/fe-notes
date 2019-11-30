# 模块化
  通过一定的规范将代码`按底层通用功能`或`业务功能`拆分为成独立的代码块，引用对应的代码块组合使用。代码块内部的数据是私有的，只向外部暴露一些接口（方法）通信。  
  减少了对全局变量的污染、避免了命名冲突，代码分离可以更好的按需加载，让代码有更高的复用性和可维护性。

  ## 原始的模块化实现  
  ## 函数封装
  将不同的功能封装在全局函数中；会污染全局的命名空间，容易引起命名冲突。
  ```
  function checkFormat() { ... }
  function getSomeList() { ... }
  ```
  ### namespace命名空间
  用对象的属性和方法实现封装；避免了命名冲突，但外部可以改变对象内部的数据，所以不安全。
  ```
  var count = {
    num: 1,
    addNum: function() { ... }
  }
  ```
  ### 匿名闭包IIFE模式
  通过匿名函数自调用的方式封装；内部的数据是私有的，保证了数据的安全。
  ```
  (function() {
    var num = 1;
    function addNum() {
      num++;
    }
    window.addNum = addNum;
    // 或者return
    return { addNum }
  })()
  ```
  
  ## 模块化的规范

  ### CommonJS
  在`commonJS`模块化规范中，模块通过 `module.exports` 暴露接口和 `require()` 引用其他模块。  
  `nodejs`使用的是`commonJS`模块化规范，每个js文件都是一个独立的模块。  
  在浏览器中使用`commonJS`模块化规范，需要使用 `browserify` 编译打包在一个js文件。  
  
  在代码运行时同步加载模块，依赖模块在第一次引入时加载，之后再引入会使用缓存（包括依赖模块的依赖）
  ```
  // m1.js
  console.log('load m1')
  const getDate = () => new Date()
  module.exports = { getDate }

  // m2.js
  const m1 = require('m1.js')
  const getYear = () => m1.getDate().getFullYear()
  module.exports = { getYear }

  // index.js
  const m2 = require('m2.js')    // 'load m1'
  console.log(m2.getYear())      // 2019
  const m1 = require('m1.js')    // 这里不会再重新加载m1模块，直接使用m2模块加载m1模块的缓存；所以不会再 console 'load m1'
  console.log(m1.getDate())      // Sat Nov 30 2019 20:40:30 GMT+0800 (中国标准时间)

  ```
  模块输出的值为值拷贝，模块内对值改变也不会影响已输出的值
  ```
  // m1.js
  let num = 0
  const addNum = () => num++
  module.exports = { num, addNum }

  // index.js
  const m1 = require('m1.js')
  console.log(m1.num)           // 0
  m1.addNum()
  console.log(m1.num)           // 0
  ```

  ### AMD
  模块文件映射
  异步加载模块
   特点：依赖前置、依赖提前执行

  ### CMD
  同步执行异步加载模块

  ### ES6 Module
  代码静态编译时输出接口
  引入模块为值引用
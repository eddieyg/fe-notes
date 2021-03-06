# JS基础
  basics ...

## 数据类型
  Js的数据类型有7种，分为基本数据类型和引用数据类型。  
  基本数据类型：null、undefined、boolean、symbol、string、number  
  引用数据类型：Object

  `typeof` 可以正确的返回大部分的数据类型，例外：null返回的是‘object’；函数返回的是‘function’
  ```
    typeof undefined              // 'undefined'
    typeof true                   // 'boolean'
    typeof Symbol()               // 'symbol'
    typeof 'abcde'                // 'string'
    typeof 28                     // 'number'
    typeof {}                     // 'object'
    typeof []                     // 'object'

    typeof function (){}          // 'function'
    typeof null                   // 'object'
  ```

## 原型与原型链
  每个函数自身属性都有一个 prototype 显式原型，该原型上的 constructor 属性指向函数本身
```
  function fn() {}
  fn.prototype.constructor === fn   // true
```

  通过 new（操作符）构造函数获得一个实例对象，实例对象有一个 `__proto__` 隐式原型，该原型指向构造函数的 prototype 显式原型，来获得它的属性
```
  function A() {}
  A.prototype.showSomething = function () { console.log('something') }
  var a = new A()
  a.__proto__ === A.prototype       // true
  a.showSomething()                 // 'something'

  // new 创建实例对象的实际操作
  var a = new Object()
  a.__proto__ = A.prototype
  A.call(a)
```

  访问一个实例对象的属性时，先从对象本身查找，再从 `__proto__` 隐式原型依次向上查找，直到顶端的 Object 的 `__proto__` 原型为null，`__proto__` 将对象连接起来形成了 **原型链**
```
  a实例对象的原型链
  a.__proto__  →  A.prototype  →  A.prototype.__proto__  →  Object.prototype  →  null
```

## 原型继承
  子类通过继承父类的原型，来获得父类的原型属性

  ```
    /* 寄生组合继承 */
    function Parent(name) {
      this.name = name
    }
    Parent.prototype.getName = function() {
      return this.name
    }

    function Child(name, age) {
      Parent.call(this, name)
      this.age = age
    }

    // 关键步骤. ps: 另一种组合继承方式 这里则是 new Parent() 实例对象来赋值
    Child.prototype = Object.create(Parent.prototype)

    Child.prototype.constructor = Child
    Child.prototype.getAge = function() {
      return this.age
    }

    var instance = new Child('edd', 18)
    instance.getName()                                      // 'edd'
    instance.getAge()                                       // 18
    instance instanceof Parent                              // true
    instance instanceof Child                               // true
  ```

  ## ES6

  ### Set 和 Map 数据结构
  - Set：不重复成员的集合，指基础数据类型`!==`不绝对相等 和 引用数据类型的引用地址不同
  - WeakSet：成员只能是引用数据类型，对其是弱引用，不增加引用计数；不可遍历
  - Map：可以用任何数据类型做键的键值对的集合；键的唯一（基础数据类型`===`绝对相等、引用数据类型的引用地址）
  - WeakMap：只能使用引用数据类型做键，对其键是弱引用，不增加引用计数；不可遍历

  ## 数组的遍历方法

  #### for循环

  #### find()
  - 参数的函数 return 为 `true` 中断遍历
  - 返回中断遍历的 `成员` 或 `undefind`
  ```
  [1,2,3].find((val, index, arr) => val === 2)    // 2
  ```
  #### findIndex()
  - 参数的函数 return 为 `true` 中断遍历
  - 返回中断遍历 `成员的索引值` 或 `-1`
  ```
  [1,2,3].findIndex((val, index, arr) => val === 2)    // 1
  ```
  #### flatMap()
  - 遍历所有数组成员
  - 返回 参数的函数 return 的数组展开一层 的新数组
  ```
  [1,2,3].flatMap((val, index, arr) => [[val, val * 2]])    // [1,2,2,4,3,6]
  ```
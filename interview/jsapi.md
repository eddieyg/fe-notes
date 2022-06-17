# 02 JsAPI / ES6


迭代器

while
for
for in   遍历 对象的key、数组的索引
for of   遍历 数组、Set、Map的值

以上可以 return、break、continue
数组、Set、Map 具有Symbol.iterator属性

forEach   遍历 iterator，无法终止遍历

some   遍历数组，如果遍历中return true终止，结果返回boolean

every   遍历数组，如果遍历每次都return true，才会结果返回true，否则 false

find   遍历数组，返回结果为 遍历中return true 的值，并终止遍历

findIndex   遍历数组，返回结果为 遍历中return true 的索引，并终止遍历

map   遍历数组，返回结果为 每个遍历return 值组成的新数组

reduce   正序遍历数组，将上一个遍历return 值，传递给下一个遍历参数，返回结果为累计后的值

reduceRight   逆序遍历数组，其他与 reduce 一致

sort   遍历数组，根据返回 -1、0、1调整排序，直接改变原数组

filter   遍历数组，返回一个筛选后的新数组


ES6 数据结构

Set：值的集合，值是唯一的 ===，有序迭代根据插入的顺序

WeakSet：成员只能是引用数据类型，对其是弱引用，不增加引用计数；不可遍历

Map：可用任何类型做键的键值对的集合；键是唯一的 ===，是有序的 根据插入的顺序

WeakMap：只能使用引用数据类型做键，对其键是弱引用，不增加引用计数；不可遍历


Symbol：是唯一的基本数据类型，Symbol.for()获取全局单一的数据值

Generator：封装内部状态的函数，返回一个遍历器对象。通过 function* 和 yield 表达式标识，next() 执行遍历


Class类

基于原型继承的类  https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes

constructor 构造函数：在 new 初始化执行，this 属性为实例属性，用super() 执行 extends父类的构造函数

对属性的读写拦截：set存值函数；get 取值函数

static 定义静态方法、字段；只能在静态方法之间访问，和通过类对象访问；实例对象上不能访问

私有域：用哈希符 # 为前缀定义私有，在类外部无法访问（实例、子类）

装饰器

https://es6.ruanyifeng.com/#docs/decorator

用于类、方法上，修改目标封装逻辑，提高代码可读性


模块化

通过规范将代码按功能逻辑拆分成为独立的代码块，组合使用。代码块内部的变量是私有的，只向外部暴露一些接口通信。  
减少了对全局变量的污染、避免了命名冲突，代码分离可以更好的按需加载，让代码有更高的复用性和可维护性。

CommonJS 模块是运行时加载，输出的是一个值的拷贝
ES6 模块是编译时输出接口，输出的是值的引用


Promise

https://blog.csdn.net/chaunceyw/article/details/78007842


# Vue
  about vue

  ## MVVM
  MVVM是Model-View-ViewModel缩写，也就是把MVC中的Controller演变成ViewModel。Model层代表数据模型，View代表UI组件，ViewModel是View和Model层的桥梁，数据会绑定到viewModel层并自动将数据渲染到视图中，视图事件响应通知viewModel层更新数据。

  ## 组件通信
  1. props + $emit
  2. $on + $emit (事件总线)
  3. $parent + $children
  4. $ref
  5. $attrs + $listeners (跨级组件)  
    子组件(b)中设`inheritAttrs: false`，父组件(a)传的数据没有在`props`接收的，被`$attrs`接收，可传递给孙子组件；孙子组件(c)可用`$listeners`直接调用父组件(a)的自定义事件方法。
  6. provide + inject (跨级深层子组件)  
  7. vuex
  8. localStorage or seesionStorage
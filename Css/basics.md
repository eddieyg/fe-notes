## Css基础知识点
  basics...

### BFC
  BFC（Block Formatting Context）：块级格式上下文；web页面中的一块级区域的渲染规则，决定了其子元素如何布局和相邻元素之间相互的关系，以及BFC区域元素和其他元素的关系和作用。
  #### BFC的规则
  - 块级子元素按垂直方向依次排放
  - BFC父元素的左边 border 与子元素的 margin 相接触
  - 垂直相邻的子元素的 margin 会发生重叠，实际距离为两者较大值
  - BFC计算高度的时候，会包含浮动子元素的高度
  - BFC区域是一个独立的容器，其子元素不会与外部元素相互发生作用
  - BFC元素不会与浮动元素重叠
  #### 创建BFC的方法
  - overflow 值不为 visible 
  - float 值不为 none
  - position 值为 absolute / fixed
  - display 值为 table-cell / table-caption / inline-flex / inline-block / flex
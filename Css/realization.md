## Css应用实现
  realization...

  ### 清除浮动
  浮动元素脱离了原来的文档流；父元素的计算高度没有包含浮动元素的高度，所以导致了父元素的高度坍塌；以下是清除浮动解决的几种方法：
  ```
    Html:
      <div class="box">
        <div class="item"></div>
      </div>
    
    1. 使用 clear: both + 伪类 (推荐)
      .box::after{
        display: table;
        content: '';
        height: 0;
        clear: both;
      }
    
    2. 父类 .box 触发BFC，设值为以下其中之一
      overflow 的值不为 visible
      float 的值不为 none
      position 的值为 absolute / fixed
      display 的值为 table-cell / table-caption / inline-block / inline-flex / flex
  ```
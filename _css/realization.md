# Css应用实现
  realization...

  ## 清除浮动
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

  ## 水平垂直居中
  ```
    Html:
      <div class="box">
        <div class="main"></div>
      </div>
    
    不定宽高

      1. 定位 + transform
        .box{
          position: relative;
        }
        .main{
          positiotn: absolute;
          left: 50%;
          top: 50%;
          transform: translate(-50%, -50%);
        }
      
      2. flex弹性布局
        .box{
          display: flex;
          justify-content: center;
          align-items: center;
        }

      3. line-height
        .box{
          height: 100px;
          line-height: 100px;
          font-size: 0;
          text-align: center;
        }
        .main{
          display: inline-block;
          line-height: initial;
          vertical-align: middle;
        }
      
      4. table-cell
        .box{
          display: table-cell;
          vertical-align: middle;
          text-align: center;
        }
        .main{
          display: inline-block;
        }

      5. writing-mode
        Html:
          <div class="box">
            <div class="inner">
              <div class="main"></div>
            </div>
          </div>
        Css:
          .box{
            writing-mode: vertical-lr;
            text-align: center;
          }
          .inner{
            display: inline-block;
            writing-mode: horizontal-tb;
          }
          .main{
            display: inline-block;
          }
  ```

  ## 两栏定宽自适应
  一栏固定宽度，另一栏宽度自适应
  ```
    Html：
      <div class="box">
          <div class="left"></div>
          <div class="right"></div>
      </div>

    1. 定位 + 内边距
      .box{
        position: relative;
        padding-left: 200px;
      }
      .left{
        position: absolute;
        left: 0;
        top: 0;
        width: 200px;
      }

    2. 浮动 + 外边距
      .left{
        float: left;
        width: 200px;
      }
      .right{
        margin-left: 200px;
      }

    3. 浮动 + 计算属性
      .left{
        float: left;
        width: 200px;
      }
      .right{
        float: right;
        width: calc( 100% - 200px );
      }
    
    4. flex弹性布局
      .box{
        display: flex;
      }
      .left{
        flex-basis: 200px;
      }
      .right{
        flex: 1;
      }
  ```

  ## 三栏定宽自适应
  左右两栏固定宽度，中间一栏宽度自适应
  ```
  1. 定位 + 内边距

    Html:
      <div class="box">
        <div class="left"></div>
        <div class="center"></div>
        <div class="right"></div>
      </div>

    Css:
      .box{
        position: relative;
        padding: 0 100px;
      }
      .left, .right{
        position: absolute;
        top: 0;
        width: 100px;
      }
      .left{
        left: 0;
      }
      .right{
        right: 0;
      }

  2. 浮动 + 外边距

    Html:
      <div class="box">
        <div class="left"></div>
        <div class="center"></div>
        <div class="right"></div>
      </div>

    Css:
      .left{
        float: left;
        width: 100px;
      }
      .right{
        float: right;
        width: 100px;
      }
      .center{
        margin: 0 100px;
      }
  
  3. 圣飞布局

    Html:
      <div class="box">
        <div class="center"></div>
        <div class="left"></div>
        <div class="right"></div>
      </div>

    Css:
      .box{
        padding: 0 100px;
      }
      .left, .right{
        position: relative;
        float: left;
        width: 100px;
      }
      .left{
        margin-left: -100%;
        left: -100px;
      }
      .right{
        margin-left: -100px;
        right: -100px;
      }
      .center{
        float: left;
        width: 100%;
      }
  
  4. flex弹性布局

    Html:
      <div class="box">
        <div class="left"></div>
        <div class="center"></div>
        <div class="right"></div>
      </div>

    Css:
      .box{
        display: flex;
      }
      .left, .right{
        flex-basis: 100px;
      }
      .center{
        flex: 1;
      }
  ```
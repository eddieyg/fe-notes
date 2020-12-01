# 开发规范
## 代码书写格式

### 命名

变量使用 `Camel` 命名法（第一个单词全小写，后面单词首字母大写）
```javascript
let goodsPayType = 'alipay'
```

boolean 类型的变量使用 is 或 has 开头
```javascript
let isNeedPay = true
let hasPayRecord = true
```

常量使用 `全大写字母，单词间下划线分隔` 命名方式
```javascript
const CURR_SITE_CODE = 'bcf10203'
```

普通函数使用 `Camel` 命名法
```javascript
function getSomeData() {}
```

类函数使用 `Pascal` 命名法（所有单词首字母大写）
```javascript
function ReadIdCard() {}
ReadIdCard.prototype.listen = function () {}

class ReadIdCard() {
  constructor() {}
}

new ReadIdCard()
```

前端对于Api接口返回的数据，去附加的字段，名称应以 `FE_` 作为开头
```javascript
querySome().then(res => {
  res.data.forEach((e, i) => e.FE_index = i)
})
```

### 空格 + 换行

```javascript
let str1, str2, str3
let num = 1 + 2
let obj1 = {name: 'aidi'}
let arr = [
  {fee: 8.88, num: 6},
  {
    code: 'DHUS123',
    fee: 8.88,
    num: 6,
  },
]

if (arr.length) // do...
if (num && arr.length) {
  // do...
} else if (str) {
  // do...
}
if (
  check.array(arr)
  && check.obj(obj1)
  && typeof str === 'string'
) {
  // do...
}

switch (type) {
  case 'A':
    // do...
    break
  default:
    // do...
}

function getSome(name = '', cb) {
  typeof cb === 'function' && cb()
}
function getSome(
  name = '',
  num = 1,
  arr = []
) {
  // do...
}
getSome('abc', 2, [1, 2, 3])
getSome(
  'abcabcabcabcabcabcabcabc', 
  2234234243234324453453453, 
  [1, 2, 3, 1, 2, 3]
)
```

### HTMl
整体 `2个空格` 缩进  
标签属性过长，换行并缩进，结束符合 `>` 在最后一个属性后面（同行）  
HTML结构按照页面排版 `从左到右`、`从上到下` 排放，给较大模块注释说明  
弹窗、遮罩等相关脱离文档流的放在最后  
```html
<div class="box">

  <!-- 头部 -->
  <div class="header">
    <div class="flex-table-th">
      <span>项目</span>
      <span>名称</span>
    </div>
    <div class="flex-table-th" @click="handleClick">操作</div>
  </div>

  <!-- 选择器 -->
  <el-select
    v-model="ai.selVal"
    remote
    filterable
    reserve-keyword
    placeholder="请输入关键词"
    :remote-method="query => queryOtherOption({query, ai})"
    @change="val => selToAddItem({val, ai})">
    <el-option
      v-for="item in options"
      :key="item.id"
      :label="item.name"
      :value="item.val">
    </el-option>
  </el-select>

  <!-- 编辑弹窗 -->
  <el-dialog 
    title="编辑"
    :visible.sync="visible">
  </el-dialog>

</div>
```

### 注释

文件头部标识文件说明
```javascript
/**
 * @file Describe the file
 */
```
公共函数方法
```javascript
/**
 * @description: 函数方法描述
 * @param {string} computeType 计算类型：加法'add' | 减法'subtr' | 乘法'mul' | 除法'div'
 * @param {number|string} num1 加数 | 被减数 | 乘数 | 被除数
 * @param {number|string} num2 加数 | 减数  | 乘数 | 除数
 * @return {number} 返回值
 */
const computeNum = (computeType, num1, num2) => {
  // do...
  return res
}
```

### 其他

- **引号：** js代码内全部使用 `'` 单引号，html的标签属性全部使用 `"` 双引号 

## 代码开发思想

### 可读性
简化逻辑：减少函数嵌套调用深度、  
逻辑关注点集中：相同的业务逻辑点代码集中在一起，不同的分离开；在函数内用`代码段落`分离，当逻辑点的代码较多时用`函数`分离，当逻辑点做的事件比较多、代码距离跨度大的时候用`文件`分离，页面组件业务逻辑点的代码较多，用`子组件`分离

### 可扩展性
编写代码时，应考虑多种可能性，为之后有可能出现的场景预留空间；使在开发新的需求时，减少写过多代码、甚至重写的负担

### 可复用性
在多处重复使用差不多相同的代码，应将代码抽出来封装，再引入使用。抽离封装代码的类型分为：基础公用、业务公用

### 性能
开发过程中，应通过一些编码技巧，减少js的执行，来规避性能开销  
**简化初始执行逻辑，减少串行执行方法、接口，提高首屏渲染效率**  
**减少遍历，使用索引查找**
```javascript
let payList = [{code: 'wx', name: '微信', appid: '...'}, ...]
let orderList = [{payCode: 'wx'}, ...]

// good  (n1 + n2)
let payListIndex = {}
payList.forEach((e, index) => payListIndex[e.code] = index)
orderList.forEach(order => {
  let index = payListIndex[order.payCode]
  if (typeof index === 'number') {
    order.payName = payList[index].name
    order.payAppid = payList[index].appid
  }
})

// bad  (n1 * n2)
orderList.forEach(order => {
  payList.forEach(pay => {
    if (order.payCode == pay.code) {
      order.payName = pay.name
      order.payAppid = pay.appid
    }
  })
})
```

**数量多、结构复杂且变化频率较小的数据；需要重复枚举变量；尽量不放在Vue.$data里**
```javascript
let list = null
const formInitData = () => ({
  name: '',
  num: 1,
  ids: [],
  ...
})

export default {
  data() {
    return {
      loadedListTime: null,
      form: formInitData()
    }
  },
  computed: {
    list() {
      return this.loadedListTime && list ? list : []
    }
  },
  destroyed() {
   list = null
  },
  methods: {
    getList() {
      queryList().then(res => {
        list = res.data
        this.loadedListTime = +new Date()
      })
    },
    clearForm() {
      this.form = formInitData()
    }
  }
}
```

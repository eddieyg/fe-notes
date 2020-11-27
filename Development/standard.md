# 开发规范
## 代码书写格式

### 命名

变量使用 `Camel驼峰` 命名法
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

普通函数使用 `Camel驼峰` 命名法
```javascript
function getSomeData() {}
```

类函数使用 `首字母大写` + `Camel驼峰` 命名法
```javascript
function ReadIdCard() {}
ReadIdCard.prototype.listen = function () {}

class ReadIdCard() {
  constructor() {}
}

new ReadIdCard()
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

### 其他

- **引号：** js代码内全部使用 `'` 单引号，html的标签属性全部使用 `"` 双引号 

## 代码开发思路


## 项目目录
# 字符串去空

> 实用、扩展题

**⚡题目**:

❓ 写一个方法去掉字符串中的空格，要求传入不同的类型分别能去掉前、后、前后、中间的空格

**💡思考**：

- 去掉字符串的空格（制表符和换行符）
- 传入类型不定
- 去掉前、后、前后、中间空格

## 尝试 ⌛

```js
/*
 * @param {*} str
 * @param {string} placement  不传去除所有；before/after/center/beforeAfter
*/
function trim (str, placement) {
  if (typeof str !== "string") return str;
  var trimStr = str;
  if (!placement) {
    trimStr = str.replace(/[\t\n\v\r\f\s]+/g, "");
  } else if (placement === "before") {
    trimStr = str.replace(/^[\t\n\v\r\f\s]+/g, "");
  } else if (placement === "after") {
    trimStr = str.replace(/[\t\n\v\r\f\s]+$/g, "");
  } else if (placement === "center") {
    /*
      str.replace(/^([\t\n\v\r\f\s]+)(.*?)([\t\n\v\r\f\s]+)$/g,function($0,$1,$2,$3){
        return $1+$2.replace(/([\t\n\v\r\f\s]+)/g,'')+$3
      })
    */
    var newStr = str.replace(/[\t\n\v\r\f\s]+/g, "");
    var RegLeft = str.match(/(^[\t\n\v\r\f\s]*)/g)[0]; // 保存右边空格
    var RegRight = str.match(/([\t\n\v\r\f\s]*$)/g)[0]; // 保存左边空格
    trimStr = RegLeft + newStr + RegRight; // 将空格加给清完全部空格后的字符串
  } else if (placement === "beforeAfter") {
    trimStr = str.replace(/^[\t\n\v\r\f\s]+|[\t\n\v\r\f\s]+$/g, "");
  }

  return trimStr;
}
```

## 优解 🚀

> 完善模式

```js
const str = '  s t  r  '

const POSITION = Object.freeze({
  left: "left",
  right: "right",
  both: "both",
  center: "center",
  all: "all",
})

function trim(str, position = POSITION.both) {
  if (typeof str !== "string") return str;
  if (!!POSITION[position]) {
    throw new Error('unexpected position value');
    return str;
  }
  
  switch(position) {
      case(POSITION.left):
        str = str.replace(/^[\t\n\v\r\f\s]+/, '')
        break;
      case(POSITION.right):
        str = str.replace(/[\t\n\v\r\f\s]+$/, '')
        break;
      case(POSITION.both):
        str = str.replace(/^[\t\n\v\r\f\s]+/, '').replace(/[\t\n\v\r\f\s]+$/, '')
        break;
      case(POSITION.center):
        while (str.match(/\w[\t\n\v\r\f\s]+\w/)) {
          str = str.replace(/(\w)([\t\n\v\r\f\s]+)(\w)/, `$1$3`)
        }
        break;
      case(POSITION.all):
        str = str.replace(/[\t\n\v\r\f\s]/g, '')
        break;
      default:
  }
  
  return str
}

const result = trim(str)

console.log(`|${result}|`) //  |s t  r|
```

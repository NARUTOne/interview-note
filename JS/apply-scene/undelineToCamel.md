# 下划线转成大驼峰

> 实用

**⚡题目**:

❓ 写一个方法把下划线命名转成大驼峰命名

## 优解 🔥

> 正则模式

```js
function undelineToCamel(str) {
  str = str.replace(/(\w)/, (match, $1) => `${$1.toUpperCase()}`)
  while(str.match(/\w_\w/)) {
    str = str.replace(/(\w)(_)(\w)/, (match, $1, $2, $3) => `${$1}${$3.toUpperCase()}`)
  }
  return str
}

console.log(undelineToCamel('a_c_def')) // ACDef

```

> api模式

```js
function changeStr(str){
   if(str.split('_').length==1)return;
   str.split('_').reduce((a,b)=>{
     return a+b.substr(0,1).toUpperCase() + b.substr(1)
   })
}
```

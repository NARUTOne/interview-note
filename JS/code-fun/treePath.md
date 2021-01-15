# 转换treePath对象

> 编程题

**⚡题目**:

❓ 模拟实现一个 函数

```js
var entry = {
a: {
 b: {
   c: {
     dd: 'abcdd'
   }
 },
 d: {
   xx: 'adxx'
 },
 e: 'ae'
}
}

// 要求转换成如下对象
var output = {
'a.b.c.dd': 'abcdd',
'a.d.xx': 'adxx',
'a.e': 'ae'
}
```

## 优解 🔥

**第一版**：

> 递归遍历处理，只处理对象

```js
function flatObj(obj, parentKey = "", result = {}) {
  for (const key in obj) {
    if (obj.hasOwnProperty(key)) {
      let keyName = `${parentKey}${key}`;
      if (Object.prototype.toString.call(obj[key]) === '[object Object]')
        flatObj(obj[key], keyName+".", result)
      else
        result[keyName] = obj[key];
    }
  }
  return result;
}
```

**逆向转换**：

```js
var entry = {
  'a.b.c.dd': 'abcdd',
  'a.d.xx': 'adxx',
  'a.e': 'ae'
}

function map(entry) {
  const obj = Object.create(null);
  for (const key in entry) {
    const keymap = key.split('.');
    set(obj, keymap, entry[key])
  }
  return obj;
}

function set(obj, map, val) {
  let tmp;
  if (!obj[map[0]]) obj[map[0]] = Object.create(null);
  tmp = obj[map[0]];
  for (let i = 1; i < map.length; i++) {
    if (!tmp[map[i]]) tmp[map[i]] = map.length - 1 === i ? val : Object.create(null);
    tmp = tmp[map[i]];
  }
}
console.log(map(entry));
```

> 支持数组

```js
function flatObj(obj, parentKey = "", result = {}) {
  for (const key in obj) {
    if (obj.hasOwnProperty(key)) {
      let keyName = `${parentKey}${key}`;
      let keyType = Object.prototype.toString.call(obj[key]);
      if (keyType === '[object Object]' || keyType === '[object Array]')
        flatObj(obj[key], keyName+".", result)
      else
        result[keyName] = obj[key];
    }
  }
  return result;
}
```

**逆向转换**：

```js
var entry = {
  'a.b.c.dd': 'abcdd',
  'a.d.xx': 'adxx',
  'a.e': 'ae'
}

function map(entry) {
  const obj = Object.create(null);
  for (const key in entry) {
    const keymap = key.split('.');
    set(obj, keymap, entry[key])
  }
  return obj;
}

function create (a) {
  if ((a - 0) > -1) return [];
  return Object.create(null);
}

function set(obj, map, val) {
  let tmp;
  tmp = obj[map[0]];

  if (!tmp) obj[map[0]] = create(tmp);
  for (let i = 1; i < map.length; i++) {
    if (!tmp[map[i]]) tmp[map[i]] = map.length - 1 === i ? val : create(tmp[map[i]]);
    tmp = tmp[map[i]];
  }
}
console.log(map(entry));

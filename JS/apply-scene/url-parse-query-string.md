# url参数解析

**⚡题目**:

❓ URL 参数解析为一个对象

## 优解 🔥

> 常规解析

```js
function parseQueryString (url) {
  const index = url.indexOf("?");
  if (!url || index < 0) return {};

  const paramStr = decodeURIComponent(url).slice(index + 1);
  const paramArr = paramStr.split("&");
  const params = paramArr.map(p => {
    const pa = p.split("=");
    return {
      [pa[0]]: [pa[1]]
    }
  })
  return params;
}
```

> 正则解析

```js
function parseQueryString (url) {
  const index = url.indexOf("?");
  if (!url || index < 0) return {};

  return JSON.parse('{"' + decodeURIComponent(url).replace(/"/g, '\\"').replace(/&/g, '","').replace(/=/g, '":"') + '"}');
}
```

> 应用

url有三种情况

```js
https://www.xx.cn/api?keyword=&level1=&local_batch_id=&elective=&local_province_id=33
https://www.xx.cn/api?keyword=&level1=&local_batch_id=&elective=800&local_province_id=33
https://www.xx.cn/api?keyword=&level1=&local_batch_id=&elective=800,700&local_province_id=33
```

匹配elective后的数字输出（写出你认为的最优解法）:

[] || ['800'] || ['800','700']

```js
function getUrlValue(url){
    if(!url) return;
    let res = url.match(/(?<=elective=)(\d+(,\d+)*)/);
    return res ?res[0].split(',') : [];
}
```

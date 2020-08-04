# jsonp

> 利用jsonp进行跨域请求

jsonp的缺点：

- 只能发送Get请求 不支持post put delete
- 不安全 xss攻击

## 基本实现

```js
function jsonp ({url, params, cb}) {
  const scriptEle = document.createElement('script');
  return new Promise((resolve, reject) => {
    window[cb] = (res) => {
      resolve(res);
      document.body.removeChild(scriptEle)
    }

    const newParams = {...params, cb};
    const arr = [];
    const keys = Object.keys(newParams);
    for (let i = 0; i < keys.length; i ++) {
      const key = keys[i];
      const p = `${key}=${newParams[key]}`
      arr.push(p);
    }

    scriptEle.src = `${url}?${arr.join("&")}`;
    document.body.appendChild(scriptEle);
  })
}
```

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

node实现的JSON服务器，调用callback

```js
var http = require('http');
http.createServer(function(req, res){
// req url  callback=?
console.log(req.url);
let data = {a: 1};
res.writeHead(200, {'Content-type' : 'text/json'})
  const reg = /callback=([\w]+)/
  if (reg.test(req.url)) {
    let padding = RegExp.$1
    res.end(`${padding}(${JSON.stringify(data)})`)
  } else {
    res.end(JSON.stringify(data));
  }
//  res.end('<p>Hello World</p>');
 res.end(JSON.stringify(data));
}).listen(3000);

```

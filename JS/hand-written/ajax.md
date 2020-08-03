# ajax

> 社区很多ajax库，有基于Promise的axios，基于Fetch的，JQuery版ajax等

## 原理

IE5、6不兼容`XMLHttpRequest`，所以要使用`ActiveXObject()`对象，并传入 `Microsoft.XMLHTTP`，达到兼容目的

readyState的五种状态详解：

- 0 － （未初始化）还没有调用send()方法
- 1 － （载入）已调用send()方法，正在发送请求
- 2 － （载入完成）send()方法执行完成，已经接收到全部响应内容
- 3 － （交互）正在解析响应内容
- 4 － （完成）响应内容解析完成，可以在客户端调用了

## 简单实现

```js
function ajax(options) {
  let method = options.method || 'GET', // 请求方式
      params = options.params, // get请求参数
      data = options.data, // get请求参数
      url = params ? `${options.url}?${Object.keys(params).map(key => `${key}=${params[key]}`).join('&')}` : `${options.url}`,
      async = options.async === false ? false : true,
      success = options.success,
      error = options.error,
      headers = options.headers,
  
  // 创建 xhr
  let xhr;

  // 兼容
  if (window.XMLHttpRequest) {
    xhr = new XMLHttpRequest();
  } else {
    xhr = new ActiveXObject('Microsoft.XMLHTTP');
  }

  // readystate 变化
  xhr.onreadystatechange = function() {
    if(xhr.readyState === 4 && xhr.status === 200) {
      success && success(xhr.responseText);
    }
  }

  xhr.open(method, url, async);
  
  if(headers) {
    Object.keys(Headers).forEach(key => xhr.setRequestHeader(key, headers[key]))
  }

  method === 'GET' ? xhr.send() : xhr.send(data)

}
```

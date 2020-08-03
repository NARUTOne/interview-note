# debounce

> 当持续触发事件时，一定时间段内没有再触发事件，事件处理函数才会执行一次，如果设定的时间到来之前，又一次触发了事件，就重新开始延时.

## 基础实现

```js
function debounce (fn, delay = 300) {
  let timer = null;
  return (...arg) => {
    clearTimeout(timer);
    timer = setTimeout(() => {
      fn.apply(this, args);
    }, delay);
  }
}
```

## 支持Promise

```js
function debounce (fn, delay = 300) {
  let timer = null;
  return (...arg) => {
    clearTimeout(timer);
    return new Promise(resolve => {
      timer = setTimeout(() => {
        resolve(fn.apply(this, args));
      }, delay);
    })
  }
}

// 使用

const debounceAjax = debounce((newParams)=>{
  return new Promise((resolve, reject) => {
    xhr({
      url: '/api',
      type: 'POST',
      data: newParams,
      success: res => {
        const {data} = res;
        const arr = isArray(data) ? data : [];
        resolve(arr);
      },
      error: err => {
        reject(err);
      }
    });
  });
}, 300);
export function apiExample (params) {
  const newParams = filterParams(params);
  return new Promise((resolve) => {
    const keys = Object.keys(newParams);
    if (!keys.length) {
      resolve([]);
    } else {
      debounceAjax(newParams).then(res => {
        resolve(res);
      });
    }
  });
}
```

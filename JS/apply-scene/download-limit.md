# 限制下载次数

> 编程题

**⚡题目**:

❓ 写一个限制下载个数的下载器

## 优解 🔥

```js
function createDownload(limit = 3) {
  const pending = [];
  let count = 0;

  const run = () => {
    if (!pending.length || count >= limit) return;

    const [url, resolve, reject] = pending.shift();

    count ++;
    fetch(url)
      .then(resolve)
      .catch(reject)
      .finally(() => {
        // 完成减一，再运行下一次
        count --;
        run();
      })
  };
  return function download(url) {
    return new Promise((resolve, reject) => {
      pending.push([url, resolve, reject]);
      run();
    });
  }
}

const download = createDownload();

download('1.jpg');
download('2.jpg');
download('3.jpg');
download('4.jpg');
download('5.jpg');

```

## 变形需求

> 传入一个 url 列表，控制并发，最终返回结果列表

```js
function downloadAll(list = [], limit = 3) {
  return new Promise((resolve, reject) => {
    const pending = [...list];
    let reqCount = 0;
    let resList = [];
    let resCount = 0;

    const run = () => {
      if (!pending.length || reqCount >= limit) return;

      const index = list.length - pending.length;
      const url = pending.shift();
      reqCount ++;

      fetch(url)
        .then(data => {
          reqCount --;
          resList[index] = data;
          resCount ++;
          resCount === list.length ? resolve(resList) : run();
        })
    }
    [...Array(limit)].forEach(() => run());
  });
}

downloadAll([1, 2, 3, 4, 5]).then(res => console.log(res));
```

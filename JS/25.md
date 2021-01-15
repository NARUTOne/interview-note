# sleep 函数

**⚡题目**:

❓ 实现一个 sleep 函数, 比如 sleep(1000) 意味着等待1000毫秒，可从 Promise、Generator、Async/Await 等角度实现

## 优解 🔥

```js
// promise

const sleep = time => {
  return new Promise((resolve, reject) => {
    if (typeof time == "number") {
      setTimeout(resolve, time);
    } else {
      reject();
    }
  })
}

sleep(1000).then(() => {console.log('wake')});

// Generator

function* sleep (time) {
  yield new Promise((resolve, reject) => {
    if (typeof time == "number") {
      setTimeout(resolve, time);
    } else {
      reject();
    }
  })
}

sleep(1000).next().value.then(() => {console.log('wake')});

// async

async function sleep (time) {
  const p = await new Promise((resolve, reject) => {
    if (typeof time == "number") {
      setTimeout(resolve, time);
    } else {
      reject();
    }
  })

  return p
}

sleep(1000).then(() => {console.log('wake')});

// callback

const sleep = (time, callback) => {
  if (typeof callback != "function") return;

  if (typeof time == "number") {
    setTimeout(callback, time);
  } else {
    callback(false)
  }
}

sleep(1000, () => {console.log("wake")});

```

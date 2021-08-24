# P51

> 编程题

**⚡题目**:

❓ 实现 `createFlow`

- flow 是指一系列 effects 组成的逻辑片段。
- flow 支持嵌套。
- effects 的执行只需要支持串行

```js
const delay = (ms) => new Promise((resolve) => setTimeout(resolve, ms));

const subFlow = createFlow([() => delay(1000).then(() => log("c"))]);

createFlow([
  () => log("a"),
  () => log("b"),
  subFlow,
  [() => delay(1000).then(() => log("d")), () => log("e")],
]).run(() => {
  console.log("done");
});

// 需要按照 a,b,延迟1秒,c,延迟1秒,d,e, done 的顺序打印

```

## 分析 ⌛

参数接受：

- 普通函数

```js
() => log("a");
```

- 延迟函数（Promise）

```js
() => delay(1000).then(() => log("d"));
```

- 另一个flow

```js
const subFlow = createFlow([() => delay(1000).then(() => log("c"))]);
```

- 用数组包裹的上述三项

```js
[() => delay(1000).then(() => log("d")), () => log("e")]
```

## 优解 🔥

- 先处理参数
- `run()` 后执行
- 参数 串行
- promise 结果 执行 then
- 判断 flow 执行

```js

function createFlow(effects = []) {
  let params = effects.slice().flat();
  
  function run(callback) {
    while(params.length) {
      const task = params.shift();
      if (typeof task === 'function') {
        const res = task();
        // 链判断运算符; 简易判断
        if (res?.then) {
          // 中断本次的 flow 执行，并且用剩下的 sources 去建立一个新的 flow; 便于延迟
          res.then(createFlow(sources).run);
          break;
        }
      } else if (task?.isFlow) {
        task.run(createFlow(sources).run);
        break;
      }
    }

    // callback?.();
    callback && callback();
  }

  return {
    run,
    isFlow: true
  }
}

```

## 其他解法

```js
const createFlow = (() => {
  const id = Symbol('flow');
  return (taskQueue) => {
    const run = (cb = () => {}) => {
      const _run = (task) => {
        if (typeof task === 'function') {
          return delay(0).then(() => task());
        }
        if (Array.isArray(task)) {
          return createFlow(task).run();
        }
        if (task[id]) {
          return task.run();
        }
        return task;
      }
      return taskQueue
        .reduce((prev, task) => prev.then(() => _run(task)), Promise.resolve())
        .then(cb);
    }
    return {[id]: true, run}
  }
})();
```

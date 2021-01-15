# 链式函数

**⚡题目**:

❓ 要求设计 LazyMan 类, 实现以下功能

```js
LazyMan('Tony');
// Hi I am Tony

LazyMan('Tony').sleep(10).eat('lunch');
// Hi I am Tony
// 等待了10秒...
// I am eating lunch

LazyMan('Tony').eat('lunch').sleep(10).eat('dinner');
// Hi I am Tony
// I am eating lunch
// 等待了10秒...
// I am eating diner

LazyMan('Tony').eat('lunch').eat('dinner').sleepFirst(5).sleep(10).eat('junk food');
// Hi I am Tony
// 等待了5秒...
// I am eating lunch
// I am eating dinner
// 等待了10秒...
// I am eating junk food
```

## 优解 🔥

> 常规解法

```js
/**
 * 普通 eat方法， 存放在立即执行队列中， 用settimeout来执行
 * sleep 方法  每次调用这个方法， 都单独开一个定时器， 把新的任务加入到这个队列
 * sleepFirst方法 把立即执行的任务队列的任务 塞到新的定时器任务队列
 */

class LazymanClass {
  constructor (_name = '') {
    this._immediateTask = [] // 立即执行定时器任务队列
    this._immediateTimer = null
    this._sleepTaskMap = {} // 候时执行定时器任务库
    this._curSleepTaskKey = null
    this._log(`Hi i am ${_name}`)
  }

  eat (meal) {
    // 添加新任务之前 清空之前的 立即定时器
    this._immediateTimer && clearTimeout(this._immediateTimer)

    const _eat = (meal) => {
      this._log(`i am eating ${meal}`)
    }

    // 判断当前是否有候时任务执行
    if (this._curSleepTaskKey === null) {
      this._immediateTask.push(_eat.bind(this, meal))
    } else {
      this._sleepTaskMap[this._curSleepTaskKey].push(_eat.bind(this, meal))
    }

    this._immediateTimer = setTimeout(() => {
      this._runImmeadiateTask()
    })
    return this
  }

  sleep (second) {
    const key = Math.random()
    this._curSleepTaskKey = key
    this._sleepTaskMap[this._curSleepTaskKey] = []
    setTimeout(() => {
      this._log(`等待了${second}秒`);
      this._runSleepTask(key)
    }, second * 1000)
    return this
  }

  sleepFirst (second) {
    const key = Math.random()
    this._curSleepTaskKey = key
    this._sleepTaskMap[key] = []
    this._immediateTask.map(task => {
      this._sleepTaskMap[key].push(task)
    })
    this._immediateTask = []

    setTimeout(() => {
      this._log(`等待了${second}秒`);
      this._runSleepTask(key)
    }, second * 1000)
    return this
  }

  _runImmeadiateTask () {
    this._immediateTask.map(task => {
      typeof task === 'function' && task()
    })
    this._immediateTask = []
  }

  _runSleepTask (key) {
    this._sleepTaskMap[key].map(task => {
      typeof task === 'function' && task()
    })
    this._sleepTaskMap[key] = []
  }

  _log(str) {
    console.log(str)
  }
}

function LazyMan(name) {
  return new LazymanClass(name)
}

// LazyMan('Tony')

// LazyMan('Tony').eat('lunch')

// LazyMan('Tony').sleep(2).eat('lunch')

// LazyMan('Tony').eat('lunch').sleep(2).eat('diner')

LazyMan('Tony').eat('lunch').eat('diner').sleepFirst(1).sleep(2).eat('junk food')
```

> next 执行 下一步操作；由于定时器，先观察所有的链式上函数，存入队列，接着一步步执行

```js
class LazyManClass {
    constructor(name) {
        this.taskList = [];
        this.name = name;
        console.log(`Hi I am ${this.name}`);
        setTimeout(() => {
            this.next();
        }, 0);
    }
    eat (name) {
        var that = this;
        var fn = (function (n) {
            return function () {
                console.log(`I am eating ${n}`)
                that.next();
            }
        })(name);
        this.taskList.push(fn);
        return this;
    }
    sleepFirst (time) {
        var that = this;
        var fn = (function (t) {
            return function () {
                setTimeout(() => {
                    console.log(`等待了${t}秒...`)
                    that.next();
                }, t * 1000);  
            }
        })(time);
        this.taskList.unshift(fn);
        return this;
    }
    sleep (time) {
        var that = this
        var fn = (function (t) {
            return function () {
                setTimeout(() => {
                    console.log(`等待了${t}秒...`)
                    that.next();
                }, t * 1000);
            }
        })(time);
        this.taskList.push(fn);
        return this;
    }
    next () {
        var fn = this.taskList.shift();
        fn && fn();
    }
}
function LazyMan(name) {
    return new LazyManClass(name);
}
LazyMan('Tony').eat('lunch').eat('dinner').sleepFirst(5).sleep(4).eat('junk food');
```

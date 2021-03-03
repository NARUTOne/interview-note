# Promise.all

**⚡题目**:

❓ 模拟实现一个 Promise.all

## 优解 🔥

> Promise.all()接受一个由promise任务组成的数组，可以同时处理多个promise任务，当所有的任务都执行完成时，Promise.all()返回resolve，但当有一个失败(reject)，则返回失败的信息，即使其他promise执行成功，也会返回失败

```js
function promiseAll(promises){
  return new Promise(function(resolve,reject){
          if(!Array.isArray(promises)){
            return reject(new TypeError("argument must be anarray"))
          }
    var countNum=0;
    var promiseNum=promises.length;
    var resolvedvalue=new Array(promiseNum);
    for(var i=0;i<promiseNum;i++){
      (function(i){
        Promise.resolve(promises[i]).then(function(value){
            countNum++;
          resolvedvalue[i]=value;
          if(countNum===promiseNum){
              return resolve(resolvedvalue)
          }
      },function(reason){
        return reject(reason)
      )
    })(i)
    }
  })
}
var p1=Promise.resolve(1),
p2=Promise.resolve(2),
p3=Promise.resolve(3);
promiseAll([p1,p2,p3]).then(function(value){
  console.log(value)
})
```

实现some方法，全失败才是失败

```js
Promise.some = function (promiseArrs) {
  return new Promise((resolve, reject) => {
      let arr = []; //定义一个空数组存放结果
      let i = 0;
      function handleErr(index, err) { //处理错误函数
          arr[index] = err;
          i++;
          if (i === promiseArrs.length) { //当i等于传递的数组的长度时 
            reject(err); //执行reject,并将结果放入
          }
      }
      for (let i = 0; i < promiseArrs.length; i++) { //循环遍历数组
          promiseArrs[i].then(resolve, (e) => handleErr(i, e))
      }
  })
}
```

# JavaScript

> JavaScript面书 📑

- [字符串去空](./1.md)
- [下划线命名转成大驼峰命名](./2.md)
- [大小写切换](./3.md)
- [传值传址的本质](./4.md)
- [加密、解密字符串](./5.md)
- [判断数据类型](./6.md)
- [数组去重](./7.md)
- [use strict](./8.md)
- [身份证号验证](./9.md)
- [new 操作符](./10.md)
- [bind, apply, call 自定义](./11.md)
- [arguments 理解](./12.md)
- [宏、微任务执行](./13.md)
- [数据类型转换下的对象属性](./14.md)
- [偶尔的全局变量](./15.md)
- [获取图片的原始高度](./16.md)
- [数组打平去重排序](./17.md)
- [深度，广度实现深拷贝](./18.md)
- [ES5/ES6继承](./19.md)
- [数组判断](./20.md)
- [url参数解析](./21.md)
- [变量声明提前](./22.md)
- [flattern 数组打平](./23.md)
- [== 隐式类型转换](./24.md)
- [sleep(1000)函数](./25.md)
- [sort 排序](./26.md)
- [伪数组代码求解](./27.md)
- [实现5.add(3).minus(2)](./28.md)
- [运算符优先级，赋值顺序](./29.md)
- [数据填充Array.from](./30.md)
- [链式函数 LazyMan类](./31.md)
- [模拟实现一个 Promise.finally](./32.md)
- [实现Promise.all](./33.md)
- [柯里化函数实现](./34.md)

## 参考

```js
for (var i = 0; i< 10; i++){
   setTimeout(() => {
    console.log(i);
   }, 1000)
}
```

- [数字正确输出 0 - 9](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/43#issuecomment-471960211)
- [深入探究 Function & Object 鸡蛋问题](https://mp.weixin.qq.com/s/4eBdJTGBIrB5JhvRrmmbaw)
![prototype](./imgs/prototype-layout.jpg)

## 额外阅读

那么 Babel 是如何把 ES6 转成 ES5 呢，其大致分为三步：

- 将代码字符串解析成抽象语法树，即所谓的 AST
- 对 AST 进行处理，在这个阶段可以对 ES6 代码进行相应转换，即转成 ES5 代码
- 根据处理后的 AST 再生成代码字符串

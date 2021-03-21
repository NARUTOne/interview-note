# 函数curry

> 把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数且返回结果的新函数的技术

```js
 /**
 * 函数式编程 柯里化
 * 完成类似lodash.curry功能，并进行扩展
 * 1、基本功能
 * 2、占位符
 * @example
var abc = function(a, b, c) {
  return [a, b, c];
};

var curried = useCurry(abc);

curried(1)(2)(3);
// => [1, 2, 3]

curried(1, 2)(3);
// => [1, 2, 3]

curried(1, 2, 3);
// => [1, 2, 3]

// Curried with placeholders.
curried(1)(_, 3)(2);
// => [1, 2, 3]
 */

const placeholder = '_'; // 占位符

/**
 *
 * @param {*} targetfn
 * @param {*} args
 * @returns
 */
function useCurry(targetfn, ...args) {
  let nextPos = 0; // 下一个有效参数索引
  const preset = [...args]; // 预设参数
  const argsNum = targetfn.length; // 目标函数参数个数
  const fps = preset.filter((p) => p !== placeholder);

  if (fps.length === argsNum) {
    return targetfn(...fps);
  }

  return (...added) => {
    // 循环并将added参数添加到preset参数
    while (added.length > 0) {
      const a = added.shift();
      // 获取下一个占位符的位置，可以是'_'也可以是preset的末尾
      while (preset[nextPos] !== placeholder && nextPos < preset.length) {
        nextPos += 1;
      }
      // 更新preset
      preset[nextPos] = a;
      nextPos += 1;
    }
    return useCurry.call(null, targetfn, ...preset);
  };
}

export default useCurry;

```

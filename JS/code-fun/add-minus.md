# 5.add(3).minus(2)

> 链式操作+给Number/Object对象添加方法

**⚡题目**:

❓ 实现5.add(3).minus(2)

## 优解 🔥

> 扩展支持浮点数;
【大数加减：直接通过 Number 原生的安全极值来进行判断，超出则直接取安全极值】
【超级多位数的小数加减：取JS安全极值位数-2作为最高兼容小数位数

```js
Number.MAX_SAFE_DIGITS = Number.MAX_SAFE_INTEGER.toString().length-2
Number.prototype.digits = function(){
  let result = (this.valueOf().toString().split('.')[1] || '').length
  return result > Number.MAX_SAFE_DIGITS ? Number.MAX_SAFE_DIGITS : result
}
Number.prototype.add = function(i=0){
  if (typeof i !== 'number') {
          throw new Error('请输入正确的数字');
      }
  const v = this.valueOf();
  const thisDigits = this.digits();
  const iDigits = i.digits();
  const baseNum = Math.pow(10, Math.max(thisDigits, iDigits));
  const result = (v * baseNum + i * baseNum) / baseNum;
  if(result>0){ return result > Number.MAX_SAFE_INTEGER ? Number.MAX_SAFE_INTEGER : result }
  else{ return result < Number.MIN_SAFE_INTEGER ? Number.MIN_SAFE_INTEGER : result }
}
Number.prototype.minus = function(i=0){
  if (typeof i !== 'number') {
          throw new Error('请输入正确的数字');
      }
  const v = this.valueOf();
  const thisDigits = this.digits();
  const iDigits = i.digits();
  const baseNum = Math.pow(10, Math.max(thisDigits, iDigits));
  const result = (v * baseNum - i * baseNum) / baseNum;
  if(result>0){ return result > Number.MAX_SAFE_INTEGER ? Number.MAX_SAFE_INTEGER : result }
  else{ return result < Number.MIN_SAFE_INTEGER ? Number.MIN_SAFE_INTEGER : result }
}
```

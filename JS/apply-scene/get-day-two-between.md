# 求两个日期中间的有效日期

> 编程题

**⚡题目**:

❓ 求两个日期中间的有效日期

如 2015-2-8 到 2015-3-3，返回[2015-2-8 2015-2-9...]

## 优解 🔥

```js
function toDateList(startTime, endTime) {
  // 2019-1-1 这种格式在ios是不兼容的Invalid Date。
  let startTimeStamp = new Date(startTime.replace(/-/g, '/')).getTime(),
    endTimeStamp = new Date(endTime.replace(/-/g, '/')).getTime(),
    dateList = []

  if (startTimeStamp > endTimeStamp) {
    return false
  }

  while (startTimeStamp !== endTimeStamp) {
    startTimeStamp += 24 * 60 * 60 * 1000

    let date = new Date(startTimeStamp)
      .toLocaleDateString()
      .replace(/\//g, '-')

    dateList.push(date)
  }

  return dateList
}
console.log(toDateList('2015-2-8', '2015-3-9'))
```

变种解法

```js
function getDays(start,end){
  let startTime = new Date(start)
  let endTime = new Date(end)
  let days = []
  while(startTime<=endTime){
    days.push(startTime.getFullYear()+'-'+(startTime.getMonth()+1)+'-'+startTime.getDate())
    startTime.setDate(startTime.getDate()+1)
  }
  return days
}

getDays('2015-2-8','2015-3-3')
```

# 两节点最近公共父节点

> Dom找节点

**⚡题目**:

❓ oNode1 和 oNode2 在同一文档中，且不会为相同的节点，寻找这两个节点最近的一个共同父节点，可以包括节点本身。

## 优解 🔥

1、递归版

```js
function findCommonParent(oNode1, oNode2) {
  // 判断情况1和2 
  if (oNode1.contains(oNode2)) {
    return oNode1
    // 判断情况1和2
  } else if (oNode2.contains(oNode1)) {
    return oNode2
  } else {
    // 判断情况3，从其中一个节点往上查找，会找到一个共同的祖先节点
    return findCommonParent(oNode1.parentNode, oNode2)
  }
}
```

2、遍历版

```js
function findCommonParent (oNode1, oNode2) {
  // 这里用oNode2是一样的
  // 如果某个节点包含另一个节点，直接返回，否则不断往上查找
  while (!oNode1.contains(oNode2)) {
    oNode1 = oNode1.parentNode 
  }

  return oNode1
}
```

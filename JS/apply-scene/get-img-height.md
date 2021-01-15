# 图片的原始高度

> JS 获取图片的原始高度

**⚡题目**:

❓ 获取图片的原始高度

## 优解 🔥

```js
function loadImageAsync(url) {
    return new Promise(function(resolve, reject) {
        var image = new Image();

        image.onload = function() {
            var obj = {
                w: image.naturalWidth,
                h: image.naturalHeight
            }
            resolve(obj);
        };

        image.onerror = function() {
            reject(new Error('Could not load image at ' + url));
        };
        image.src = url;
    });
}
```

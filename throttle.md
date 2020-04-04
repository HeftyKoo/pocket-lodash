# lodash源码分析之throttle

本文为读 lodash 源码的第一百七十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import debounce from './debounce.js'
import isObject from './isObject.js'
```
[《lodash源码分析之debounce》](debounce.md)
[《lodash源码分析之isObject》](isObject.md)

## 源码分析

`throttle` 就是平时据说的节流，也就是设置一个时间，每隔一段时间调用一次 `func` 。

这个实现好像有点熟悉，`throttle` 中的 `maxWait` 好像就是干这个事情的。

但是 `maxWait` 和 `wait` 有个竞争的关系，如果 `wait` 和 `maxWait` 设置成一样，那是不是就是节流所需要的效果了呢？

是的，来看看源码的实现：

```javascript
function throttle(func, wait, options) {
  let leading = true
  let trailing = true

  if (typeof func !== 'function') {
    throw new TypeError('Expected a function')
  }
  if (isObject(options)) {
    leading = 'leading' in options ? !!options.leading : leading
    trailing = 'trailing' in options ? !!options.trailing : trailing
  }
  return debounce(func, wait, {
    leading,
    trailing,
    'maxWait': wait
  })
}
```

`throttle` 调用的其实就是 `debounce` ，只不过将 `maxWait` 设置成了和 `wait` 一样。

`throttle` 也支持 `leading` 和 `trailling` 的设置，并且 `leading` 和 `trailing` 默认为 `true` ，也即默认在 `timer` 启动前和完成后都执行 `func` 。

## 参考资料

[探究防抖(debounce)和节流(throttle)](https://github.com/Bowen7/Blog/issues/5)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 
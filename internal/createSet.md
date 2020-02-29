# lodash源码分析之createSet

本文为读 lodash 源码的第九十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import setToArray from './setToArray.js'
```

[《lodash源码分析之setToArray》](internal/setToArray.md)

## 源码分析

`createSet` 是一个用来将 `array` 转换为 `set` 的函数。但是会有一些兼容判断。

```javascript
const INFINITY = 1 / 0
const createSet = (Set && (1 / setToArray(new Set([,-0]))[1]) == INFINITY)
  ? (values) => new Set(values)
  : () => {}
```

可以看到，如果支持 `Set` 则 `createSet` 为

```javascript
const createSet = (values) => new Set(values)
```

否则为空函数：

```javascript
const createSet = () => {}
```

第一个条件判断 `Set` 浏览器是否支持很容易理解，但是这个条件是什么意思呢？

```javascript
setToArray(new Set([,-0]))[1]) == INFINITY
```

原来规范规定 `Set` 中的键正负 `0` 和 `0` 是没有区分的，都为 `0` ，但是在 `IE` 中，是将正负 `0` 和 `0` 是分开的，在这种情况下，也是直接使用空函数。

## 参考资料

[MDN: Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set#Browser_compatibility)

[lodash.js createSet函数的疑问](https://segmentfault.com/q/1010000016627460)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 
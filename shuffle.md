# lodash源码分析之shuffle

本文为读 lodash 源码的第一百六十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import copyArray from './.internal/copyArray.js'
```

[《lodash源码分析之copyArray》](internal/copyArray.md)

## 源码分析

其实 `shuffle` 是 `sampleSize` 的一个特例，`sampleSize` 可以随机取 `n` 个值，如果 `n` 和 `array.length` 一样，即将 `array` 随机取出和 `array` 长度一样的值，就达到了 `shuffle` 的目的。

源码如下：

```javascript
function shuffle(array) {
  const length = array == null ? 0 : array.length
  if (!length) {
    return []
  }
  let index = -1
  const lastIndex = length - 1
  const result = copyArray(array)
  while (++index < length) {
    const rand = index + Math.floor(Math.random() * (lastIndex - index + 1))
    const value = result[rand]
    result[rand] = result[index]
    result[index] = value
  }
  return result
}
```

可以看到源码基本一样，只不过 `n` 默认就是 `length` 。

其实可以直接改成这样：

```javascript
function shuffle (array) {
  const length = array == null ? 0 : array.length
  return sampleSize(array, length)
}
```

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 
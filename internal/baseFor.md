# lodash源码分析之baseFor

本文为读 lodash 源码的第一百二十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`baseFor` 是用来实现对象属性和值遍历的基础方法。在遍历时，会将属性和值传给 `iteratee` 函数，如果 `iteratee` 函数返回 `false` ，则会中止遍历。

源码如下：

```javascript
function baseFor(object, iteratee, keysFunc) {
  const iterable = Object(object)
  const props = keysFunc(object)
  let { length } = props
  let index = -1

  while (length--) {
    const key = props[++index]
    if (iteratee(iterable[key], key, iterable) === false) {
      break
    }
  }
  return object
}
```

使用 `Object` 构造方法将传入的参数 `object` 转换成对象。

调用传入的 `keysFunc` 方法，获取 `object` 上的属性，这时得到的是一个属性数组 `props` 。获取属性的行为是由 `keysFunc` 来定义的。

接着就是遍历属性列表 `props` 了。每次遍历时，将属性取出，存在变量 `key` 中，然后调用 `iteratee` 将值 `iterable[jey]` 和属性 `key` 及对象 `iterable` 传入，如果返回 `false`，则中止遍历。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 
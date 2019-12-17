# lodash源码分析之isSymbol

本文为读 lodash 源码的第五十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import getTag from './.internal/getTag.js'
```

[《lodash源码分析之数据类型获取的兼容性》](internal/getTag.md)

## 源码分析

`isSymbol` 用来判断一个值是否为 `symbol` 类型。

看下源码：

```javascript
function isSymbol(value) {
  const type = typeof value
  return type == 'symbol' || (type === 'object' && value != null && getTag(value) == '[object Symbol]')
}
```

第一个条件很好理解，只要用 `typeof` 判断，返回的值为 `symbol` 即可。

在看第二个条件之前，先看下这个 `lodash` 的测试用例：

```javascript
assert.strictEqual(isSymbol(Object(symbol)), true)
```

可以看到这里先将一个 `symbol` 值转换为 `object` 类型，再传给 `isSymbol` 时，返回依旧为 `true` 。

看下以下代码：

```javascript
const symbol = Symbol()
typeof Object(symbol) // 'object'
Object.prototype.toString.call(Object(symbol)) // '[object Symbol]'
```

可以看到，用 `typeof` 来检测时，返回的是 `object`， 因此需要第二个条件来判断，即再通过 `getTag(value) == '[object Symbol]'`  来判断。

## 相关链接

[lodash源码解释：isSymbol](https://www.yuque.com/lanchengtie/rbtkp2/goho6n)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 
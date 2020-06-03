# lodash源码分析之matchesProperty

本文为读 lodash 源码的第三百五十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseClone from './.internal/baseClone.js'
import baseMatchesProperty from './.internal/baseMatchesProperty.js'
```

[《lodash源码分析之baseClone》](./internal/baseClone.md)

[《lodash源码分析之baseMatchesProperty》](./internal/baseMatchesProperty.md)


## 源码分析

`matchesProperty` 接收属性路径 `path` 和比较值 `srcValue` ，返回一个函数，这个函数接收一个 `object` ，用来判断 `object` 上对应属性路径上的值是否和 `srcValue` 相等。

源码如下：

```javascript
const CLONE_DEEP_FLAG = 1

function matchesProperty(path, srcValue) {
  return baseMatchesProperty(path, baseClone(srcValue, CLONE_DEEP_FLAG))
}
```

其实就是调用 `baseMatchesProperty` 来实现，只不过在将 `srcValue` 传入时，会进行一次深度复制。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 


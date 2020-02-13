# lodash源码分析之parent

本文为读 lodash 源码的第七十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseGet from './baseGet.js'
import slice from '../slice.js'
```

[《lodash源码分析之baseGet》](./baseGet.md)

[读lodash源码之从slice看稀疏数组与密集数组](../slice.md)

## 源码分析

`parent` 的作用是取指定属性路径上一层在 `object` 中的值。

例如有 `object` 如下：

```javascript
const object = {
  a: {
    b: {
      c: 'test'
    }
  }
}
```

指定的属性路径为 `['a', 'b', 'c']`，则取的是属性路径[`['a', 'b']` 的值，即最后取出的值为:

```javascript
{
  c: 'test'
}
```

源码如下：

```javascript
function parent(object, path) {
  return path.length < 2 ? object : baseGet(object, slice(path, 0, -1))
}
```

如果 `path.length` 小于 `2` ，即为 `1` 或者 `0` ，这里的父级只能为 `object` 本身。

否则调用 `baseGet` 取值，传入的第二个参数使用 `slice` 方法，将最后一项移除。这时取出来的值即为父级的值。 

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 
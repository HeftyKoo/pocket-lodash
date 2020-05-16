# lodash源码分析之createAssigner

本文为读 lodash 源码的第二百八十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import isIterateeCall from './isIterateeCall.js'
```

[lodash源码分析之isIterateeCall](./isIterateeCall.md)


## 源码分析

`createAssigner` 用来创建一个跟 `assign` 差不多的方法，其实就是用来创建 `merge` 和 `mergeWith` 的方法。

因此它会返回一个函数，并且接收的参数和 `mergeWith` 一致。

源码如下：

```javascript
function createAssigner(assigner) {
  return (object, ...sources) => {
    let index = -1
    let length = sources.length
    let customizer = length > 1 ? sources[length - 1] : undefined
    const guard = length > 2 ? sources[2] : undefined

    customizer = (assigner.length > 3 && typeof customizer === 'function')
      ? (length--, customizer)
      : undefined

    if (guard && isIterateeCall(sources[0], sources[1], guard)) {
      customizer = length < 3 ? undefined : customizer
      length = 1
    }
    object = Object(object)
    while (++index < length) {
      const source = sources[index]
      if (source) {
        assigner(object, source, index, customizer)
      }
    }
    return object
  }
}
```

`createAssigner` 接收一个 `assigner` 函数，这个方法的作用和 `Object.assgin` 类似。

### 自定义合并函数customizer的确定

#### 必定是最后一个参数

```javascript
let index = -1
let length = sources.length
let customizer = length > 1 ? sources[length - 1] : undefined
```

如果有传自定义 `customizer` 参数，则 `sources` 的个数必须要大于 `1` 个，因为必须至少要有一个 `source` 和 `object` 来合并，又因为 `source` 可以为多个，因此 `customizer` 函数要放在最后一个参数传入。

所以有传 `customizer` ，则必定在最后一个参数。

#### assigner 要接受customizer函数

```javascript
const guard = length > 2 ? sources[2] : undefined
customizer = (assigner.length > 3 && typeof customizer === 'function')
  ? (length--, customizer)
: undefined
```

取出可能的 `customizer` 后，还需要判断 `assigner` 是否接收 `customizer` 函数。

如果创建 `merge` 函数时，是不需要 `customizer` 函数的，但是 `mergeWith` 是可以接收 `customizer` 函数的，不接收 `customizer` 函数时， `assinger` 接收的参数个数为 `3` 个，接收时 `assinger` 的参数为 `4` 个。

因此 `assigner` 大于 `3`  个时，即 `assigner` 接收 `customizer` 函数，且  `customizer` 又为函数时， `customizer` 才可能是自定义合并函数。

注意这里有 `customizer` 时， `length` 会减少 `1` ，因为 `customizer` 不是需要合并的对象 `source` ，因此要将它排除。

#### 迭代过程中的 customizer

```javascript
if (guard && isIterateeCall(sources[0], sources[1], guard)) {
  customizer = length < 3 ? undefined : customizer
  length = 1
}
```

注意 `guard` 是 `sources[2]` ，这里调用 `isIterateeCall` 来判断 `sources[0]` 、 `sources[1]` 和 `sources[2]` 是否对应迭代过程中的 `value` 、`key` 和 `object` 。

如果是迭代的过程中使用 `merge` 或者 `mergeWith` ，其实只有第一个参数 `value` 才是要真正需要合并的对象。

第二个参数 `key` 和第三个参数 `object` 是需要丢弃的，第四个参数才是自定义函数 `csutomizer` 。

因为如果存在 `customizer` 时，上面已经 `length -1` ，因此在 `length < 3` 时，肯定不会有 `customizer` 函数，置为 `undefined` 。

在这种情况下，只有一个参数是有效的 `source` ，需要将 `length` 设置为 `1` 。

### 调用assigner

```javascript
object = Object(object)
while (++index < length) {
  const source = sources[index]
  if (source) {
    assigner(object, source, index, customizer)
  }
}
```

接下来的过程就简单了，遍历有效的 `sources` ，在遍历的过程中，调用 `assigner` 来合并 `object` 和 `source` 。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 
# lodash源码分析之baseWhile

本文为读 lodash 源码的第三十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import slice from './slice.js'
```

[读lodash源码之从slice看稀疏数组与密集数组](./slice.md)

## 源码分析

`baseWhile` 是实现 `dropWhile` 和 `takeWhile` 的工具函数。

源码如下：

```javascript
function baseWhile(array, predicate, isDrop, fromRight) {
  const { length } = array
  let index = fromRight ? length : -1

  while ((fromRight ? index-- : ++index < length) &&
    predicate(array[index], index, array)) {}

  return isDrop
    ? slice(array, (fromRight ? 0 : index), (fromRight ? index + 1 : length))
    : slice(array, (fromRight ? index + 1 : 0), (fromRight ? length : index))
}
```

`baseWhile` 接收四个参数，第一个参数很明显是需要处理的数组，其它参数后面分析的时候再看。

我们先来看这段：

```javascript
let index = fromRight ? length : -1

while ((fromRight ? index-- : ++index < length) &&
       predicate(array[index], index, array)) {}
```

如果 `fromRight` 为 `true`，则 `index` 为数组的长度，否则为 `-1`。

接下来是一段遍历，先看这个条件: `fromRight ? index-- : ++index < length` ，这时 `fromRight` 的意思就很显示了，表示数组从后往前遍历，因为 `fromRight` 为 `true` 时，`index` 为 `length` ，因此使用 `index—` 迭代。

接下来再看这个条件: `predicate(array[index], index, array)` ，每次迭代的时候，都会将当前迭代到的数组项、索引、和数组传给 `predicate` 函数，当 `predicate` 返回假值时，循环中止，此时索引 `index` 为中止时的索引。

`isDrop` 的是指是否将遍历过的项给移除掉。

先来看移除掉的情况：

`slice(array, (fromRight ? 0 : index), (fromRight ? index + 1 : length))`  

这样看就一目了然了，如果是从后向前遍历，相当于调用 `slice(0, index+1)` ，保留 `0` 到 `index` 的所有项。从前向后遍历，则相当于调用 `slice(index, length)` ，保留 `index` 到最后一项。

保留的情况则正好相反：

`slice(array, (fromRight ? index + 1 : 0), (fromRight ? length : index))`

如果从后向前遍历，其实是保留后半截，即相当于 `slice(index+ 1, length)`

如果是从前向后遍历，其实是保留前半截，即相当于 `slice(0, index)`

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 
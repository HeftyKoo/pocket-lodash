# lodash源码分析之baseSortedUniq

本文为读 lodash 源码的第八十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import eq from '../eq.js'
```

[《lodash源码分析之NaN不是NaN》](../eq.md)

## 源码分析

`baseSortedUniq` 是用来实现 `sortedUniq` 和 `sortedUniqBy` 的内部方法，它的作用和 `uniq` 函数的作用差不多，传入一个数组 `array` ，然后返回这个数组去重后的结果。

但是 `baseSortedUniq` 和 `uniq` 不同的是，`baseSortedUniq` 只接受已经排好序的数组。

因为传入的数组是已经排好序的了，因此对数组进行迭代遍历时，只需要将前一个值保存起来，和当前值比较，如果和当前值不同，那这个值肯定是没有在这之前出现过的，就可以将这个值放入到结果集里了，如果相同，那肯定是出现过了，而且已经放过结果集里了，在这次迭代中不能再放到结果集里了。

来看源码：

```javascript
function baseSortedUniq(array, iteratee) {
  let seen
  let index = -1
  let resIndex = 0

  const { length } = array
  const result = []

  while (++index < length) {
    const value = array[index], computed = iteratee ? iteratee(value) : value
    if (!index || !eq(computed, seen)) {
      seen = computed
      result[resIndex++] = value === 0 ? 0 : value
    }
  }
  return result
}
```

这里用 `seen` 来保存上次出现的不重复值。用 `index` 来作为迭代的指针，用 `resIndex` 作为结果数组的指针。

因为 `baseSortedUniq` 要支持 `baseSortedBy`，因此也支持传入 `iteratee` 来返回一个新值，再使用 `iteratee` 函数返回的值来作比较。

```javascript
if (!index || !eq(computed, seen))
```

可以看到，这是符合条件的判断式，`!index` 是指 `index` 为 `0` 的情况，也即数组的第一个值，因为数组的第一个值，也即肯定没有在之前出现过，所以要在结果集中。

如果不是第一个值，则调用 `eq` 来判断两个值是否相等，如果不相等，表示之前还没有出现过，要存入结果集中。

在存入结果集中之前，也将当前值保存在 `seen` 中，方便下一个值来比较。

注意到，在将 `value` 保存到结果集之前，有这样的判断：

```javascript
value === 0 ? 0 : value
```

因为 `javascript` 有 `+0` 、`-0` 和 `0` ，即 `0` 也是带符号的，但是值都为 `0` 。在返回的结果中，统一作为 `0` 来处理。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 
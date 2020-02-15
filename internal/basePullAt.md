# lodash源码分析之basePullAt

本文为读 lodash 源码的第七十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseUnset from './baseUnset.js'
import isIndex from './isIndex.js'
```

[《lodash源码分析之baseUnset》](./baseUnset.md)

[《lodash源码分析之isIndex》](./isIndex.md)

## 源码分析

`basePullAt` 是用来实现 `pullAt` 的内部方法，它的作用是将一个数组 `array` 指定的所有索引 `indexes` 的值从数组中删除，并且返回修改后的数组。

如果看 `lodash` 文档，可以知道，`pullAt` 在修改数组 `array` 的同时返回的是对应索引所删除的值，这一点是和 `basePullAt` 所不同的地方。

还有 `basePullAt` 要求索引数组 `indexes` 是按照升序排列好的，但是 `pullAt` 是没有这种要求的，下面读源码的时候会看到为什么会有这种要求。

这也是 `lodash` 源码好读的原因，它的 `api` 所完成的功能并不会只在一个函数内就全部完成，而是会拆分成不同的功能函数，每个功能函数只负责很小部分的功能，精简而且好复用。

源码如下：

```javascript
function basePullAt(array, indexes) {
  let length = array ? indexes.length : 0
  const lastIndex = length - 1

  let previous
  while (length--) {
    const index = indexes[length]
    if (length === lastIndex || index !== previous) {
      previous = index
      if (isIndex(index)) {
        array.splice(index, 1)
      } else {
        baseUnset(array, index)
      }
    }
  }
  return array
}
```

首先判断 `array` 如果为真值，则取 `indexes` 的长度，后面遍历 `indexes` 。

然后将最后的索引 `lastIndex` 存起来，后面会用到。

### indexes为什么要按升序排列

因为要删除指定的索引，如果不考虑特殊情况，其实核心代码如下：

```javascript
while (length--) {
  const index = indexes[length]
  array.splice(index, 1)  
}
```

两行代码就搞掂。

这时，就很清楚为什么传入的 `indexes` 一定要按升序来排列了，因为遍历的时候，`indexes` 是从后往前取的，也就是说，先将大的索引取出来，再做删除的操作，在做删除的操作时，是会改动到原数组的，因为按照升序排列了，因此即使原数组改动了，也不会影响到下一个索引的位置。

### 处理index有重复的情况

在传入 `indexes` 的时候，有可能会有传重复的情况，如传入了 `[1, 2,2,3]` 的情况，如果直接 `splice` ，在删除完第一个索引是 `2` 的值时，原数组会发生变动，原来索引 `2` 后面的值会递补到这个位置，再在下一次循环时，会将这个递补过来的值删除，这不是想要的结果。

要避免这种情况的发生，因为 `indexes` 是排序好了的，只需要将前一次删除的索引保存起来，如果下次循环进来，前一次的索引和当前的索引相等，即表示已经进行过删除，那直接跳过这次循环即可。

因此代码改成这样：

```javascript
let previous
while (length--) {
    const index = indexes[length]
    if (index !== previous) {
      previous = index
      array.splice(index, 1)
    }
  }
```

要注意，`previous` 应该定义在 `while` 的外面，`lodash` 的源码是错误的，它是定义在 `while` 的里面，因为 `let` 有 `block scope` ，这样每次循环时，都会重新被赋值为 `undefined`，达不到保存前一次 `index` 的目的。

### 处理非正常索引的情况

在 `js` 中，数组其实也是对象，也是可以添加属性的，如果 `indexes` 包含属性，那就需要调用 `baseUnset` 将对应的属性删除了。因此在调用 `splice` 时，需要判断一下，当前的值究竟是索引还是属性。

代码改成如下：

```javascript
let previous
  while (length--) {
    const index = indexes[length]
    if (index !== previous) {
      previous = index
      if (isIndex(index)) {
        array.splice(index, 1)
      } else {
        baseUnset(array, index)
      }
    }
  }
```

看到这里，你会发现，`lastIndex` 已经定义了，但是从来没有用过。那这个有什么用呢？

这个是用来处理一个非常特殊的情况的，假设传入的 `indexes` 如下：`[1,2,3, undefined]`，它最后的一个值为`undefined`，而数组中刚好有一个键为 `undeinfed` 的属性，此时应该要将它删除掉的。

但是，此时 `previous` 刚定义，还没有赋值，因此取出来的 `index` 和 `previous` 都为 `undefined`，并不会执行删除操作，因此还需要加上一个判断条件 `length === lastIndex` 的时候，即第一次循环时，会执行删除操作。

最终的代码就如源码所示了。 

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 
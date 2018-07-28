# lodash源码分析之baseFlatten

> 一颗沙里看出一个世界，
>
> 一朵野花里一座天堂，
>
> 把无限放在你的手掌上，
>
> 永恒在一刹那里隐藏。
>
> ——布莱克《天真的预示》

本文为读 lodash 源码的第二十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import isFlattenable from './isFlattenable.js'
```

《[lodash源码分析之isFlattenable](./isFlattenable.md)》

## 源码分析

`baseFlatten` 用来展平数组，可以指定展平的深度。
```javascript
function baseFlatten(array, depth, predicate, isStrict, result) {
  predicate || (predicate = isFlattenable)
  result || (result = [])

  if (array == null) {
    return result
  }

  for (const value of array) {
    if (depth > 0 && predicate(value)) {
      if (depth > 1) {
        baseFlatten(value, depth - 1, predicate, isStrict, result)
      } else {
        result.push(...value)
      }
    } else if (!isStrict) {
      result[result.length] = value
    }
  }
  return result
}
```

### 参数含义

* `@param {Array} array` : 需要展平的数组
* `@param {Number} depth`: 展平的深度
* `@param {Function} predicate`: 每次迭代时都会调用的检查函数，回调参数为每次迭代的值，默认为 `isFlattenable` 函数
* `@param {Boolean} isStrict`: 是否严格模式，在严格模式下，迭代的值必须要经过 `predicate` 函数的检查才存入结果数组中
* `@param {Array} result`： 结果数组

### 两个分支

`baseFlatten` 的源码很简单，先是使用 `for...of` 来迭代数组，接下来分两个分支。

第一个分支是递归调用，每次递归的时候，都递减 `depth` ，直至 `depth` 变为 `0` 为止，每次递归都将结果数组的引用 `result` 传递进去，将展平的结果存入 `result` 中。

第二个分支的判断条件使用到了 `isStrict` 这个参数。在我看来，这个参数的作用是用来在展平的过程中，舍弃一些值。从源码中可以看到，满足第一个分支的条件是深度大于 `0`，并且值要通过 `predicate` 的检测。如果此时 `isStrict` 设置为 `true` 时，那只有满足第一个分支条件的值才会存入结果数组中。

这个是 `lodash` 中的内部函数，`lodash` 在处理数组的函数中，有很多参数个数是不定的，但是最后一个参数有特殊用途的，这时 `isStrict` 就可以派上用场了。

例如如下的函数（仅作例子使用，没有任何用途）：

```javascript
function test (...values) {
    return baseFlatten(values, 1, undefined, true)
}
test([1,2,3],[4,5,6] function(){})
```

`test` 在调用的时候，`values` 的值为 `[[1,2,3],[4,5,6],function(){}]`，在 `test` 调用后，返回的值会是 `[1,2,3,4,5,6]`，后面的因为通不过 `isFlattenable` 的检测，因此被舍弃了。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面
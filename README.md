# pocket-lodash

> 能做事的做事，能发声的发声。有一分热，发一分光，就令萤火一般，也可以在黑暗里发一点光，不必等候炬火。
>
> ​																					——鲁迅

很喜欢鲁迅的这段话，在很多社区也会引用作为签名。鲁迅这段话有反抗的意味，用于单纯的技术分享似乎不太合适，可是管它呢。

前一段时间，从 `Zepto` 开始，初尝了阅读源码，以前觉得很难，现在有点上了瘾，这次阅读的是 `lodash` ，平时也会用到这个库，希望可以借此对它有更深入的了解。

这系列的每篇文章应该不会很长，依 `lodash` 的组织，每个文件都会有一篇对应的文章，因此命名为 `pocket-lodash`。

## 源码版本

[lodash master 分支](https://github.com/lodash/lodash)

## GitBook

《[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)》

## 目录

* [internal]()
  * [Hash](internal/Hash.md)
  * [assocIndexOf](internal/assocIndexOf.md)
  * [ListCache](internal/ListCache.md)
  * [MapCache](internal/MapCache.md)
  * [SetCache](internal/SetCache.md)
  * [baseFindIndex](internal/baseFindIndex.md)
  * [baseIsNaN](eq.md)
  * [strictIndexOf](internal/strictIndexOf.md)
  * [baseIndexOf](internal/baseIndexOf.md)
  * [arrayIncludes](internal/arrayIncludes.md)
  * [arrayIncludesWith](internal/arrayIncludesWith.md)
  * [cacheHas](internal/cacheHas.md)
  * [baseDifference](internal/baseDifference.md)
  * [baseGetTag](internal/baseGetTag.md)
  * [getTag](internal/getTag.md)
  * [isFlattenable](internal/isFlattenable.md)
  * [baseFlatten](internal/baseFlatten.md)
  * [baseWhile](internal/baseWhile.md)
  * [baseIntersection](internal/baseIntersection.md)
  * [castArrayLikeObject](internal/castArrayLikeObject.md)
  * [strictLastIndexOf](internal/strictLastIndexOf.md)
  * [isIndex](internal/isIndex.md)
  * [baseIndexOfWith](internal/baseIndexOfWith.md)
  * [copyArray](internal/copyArray.md)
  * [basePullAll](internal/basePullAll.md)
  * [isKey](internal/isKey.md)
  * [memoizeCapped](internal/memoizeCapped.md)
  * [stringToPath](internal/stringToPath.md)
  * [castPath](internal/castPath.md)
  * [toKey](internal/toKey.md)
  * [baseGet](internal/baseGet.md)
  * [baseAt](internal/baseAt.md)
  * [parent](internal/parent.md)
  * [baseUnset](internal/baseUnset.md)
  * [basePullAt](internal/basePullAt.md)
  * [compareAscending](internal/compareAscending.md)
  * [baseSortedIndexBy](internal/baseSortedIndexBy.md)
  * [baseSortedIndex](internal/baseSortedIndex.md)
  * [baseSortedUniq](internal/baseSortedUniq.md)
  * [setToArray](internal/setToArray.md)
  * [createSet](internal/createSet.md)
  * [baseUniq](internal/baseUniq.md)
  * [baseProperty](internal/baseProperty.md)
  * [baseXor](internal/baseXor.md)
  * [baseAssignValue](internal/baseAssignValue.md)
  * [assignValue](internal/assignValue.md)
  * [baseZipObject](internal/baseZipObject.md)
  * [baseSet](internal/baseSet.md)
  * [arrayReduce](internal/arrayReduce.md)
  * [baseFor](internal/baseFor.md)
  * [freeGlobal](internal/freeGlobal.md)
  * [root](internal/root.md)
  * [nodeTypes](internal/nodeTypes.md)
  * [arrayLikeKeys](internal/arrayLikeKeys.md)
  * [baseForOwn](internal/baseForOwn.md)
  * [baseEach](internal/baseEach.md)
  * [baseReduce](internal/baseReduce.md)
  * [arrayEach](internal/arrayEach.md)
  * [arrayEachRight](internal/arrayEachRight.md)
  * [baseForRight](internal/baseForRight.md)
  * [baseForOwnRight](internal/baseForOwnRight.md)
  * [baseEachRight](internal/baseEachRight.md)
  * [baseSortBy](internal/baseSortBy.md)
  * [compareMultiple](internal/compareMultiple.md)
  * [baseOrderBy](internal/baseOrderBy.md)
  * [arrayReduceRight](internal/arrayReduceRight.md)
  * [asciiSize](internal/asciiSize.md)
  * [hasUnicode](internal/hasUnicode.md)
  * [unicodeSize](internal/unicodeSize.md)
  * [stringSize](internal/stringSize.md)
  * [Stack](internal/Stack.md)
  * [cloneBuffer](internal/cloneBuffer.md)
  * [copyObject](internal/copyObject.md)
  * [copyArrayBuffer](internal/copyArrayBuffer.md)
  * [cloneDataView](internal/cloneDataView.md)
  * [cloneTypedArray](internal/cloneTypedArray.md)
  * [cloneRegExp](internal/cloneRegExp.md)
  * [cloneSymbol](internal/cloneSymbol.md)
  * [getSymbols](internal/getSymbols.md)
  * [copySymbols](internal/copySymbols.md)
  * [getSymbolsIn](internal/getSymbolsIn.md)
  * [copySymbolsIn](internal/copySymbolsIn.md)
  * [getAllKeys](internal/getAllKeys.md)
  * [getAllKeysIn](internal/getAllKeysIn.md)
  * [isPrototype](internal/isPrototype.md)
  * [initCloneObject](internal/initCloneObject.md)
  * [baseClone](internal/baseClone.md)
  * [baseConformsTo](internal/baseConformsTo.md)
  * [equalArrays](internal/equalArrays.md)
  * [mapToArray](internal/mapToArray.md)
  * [equalByTag](internal/equalByTag.md)
  * [equalObjects](internal/equalObjects.md)
  * [baseIsEqualDeep](internal/baseIsEqualDeep.md)
  * [baseIsEqual](internal/baseIsEqual.md)
  * [isStrictComparable](internal/isStrictComparable.md)
  * [getMatchData](internal/getMatchData.md)
  * [baseIsMatch](internal/baseIsMatch.md)


* [slice](slice.md)
* [chunk](chunk.md)
* [compact](compact.md)
* [eq](eq.md)
* [map](map.md)
* [isObjectLike](isObjectLike.md)
* [isArguments](isArguments.md)
* [isLength](isLength.md)
* [isArrayLike](isArrayLike.md)
* [isArrayLikeObject](isArrayLikeObject.md)
* [difference](difference.md)
* [last](last.md)
* [differenceBy](differenceBy.md)
* [differenceWith](differenceWith.md)
* [drop](drop.md)
* [dropRight](dropRight.md)
* [dropRightWhile](dropRightWhile.md)
* [dropWhile](dropWhile.md)
* [findLastIndex](findLastIndex.md)
* [head](head.md)
* [flatten](flatten.md)
* [flattenDeep](flattenDeep.md)
* [flattenDepth](flattenDepth.md)
* [fromPairs](fromPairs.md)
* [indexOf](indexOf.md)
* [initial](initial.md)
* [intersection](intersection.md)
* [intersectionBy](intersectionBy.md)
* [intersectionWith](intersectionWith.md)
* [isObject](isObject.md)
* [isSymbol](isSymbol.md)
* [toNumber](toNumber.md)
* [toFinite](toFinite.md)
* [toInteger](toInteger.md)
* [lastIndexOf](lastIndexOf.md)
* [nth](nth.md)
* [pullAll](pullAll.md)
* [pull](pull.md)
* [pullAllBy](pullAllBy.md)
* [pullAllWith](pullAllWith.md)
* [memoize](memoize.md)
* [get](get.md)
* [pullAt](pullAt.md)
* [remove](remove.md)
* [sortedIndex](sortedIndex.md)
* [sortedIndexBy](sortedIndexBy.md)
* [sortedIndexOf](sortedIndexOf.md)
* [sortedLastIndex](sortedLastIndex.md)
* [sortedLastIndexBy](sortedLastIndexBy.md)
* [sortedLastIndexOf](sortedLastIndexOf.md)
* [sortedUniq](sortedUniq.md)
* [sortedUniqBy](sortedUniqBy.md)
* [tail](tail.md)
* [take](take.md)
* [takeRight](takeRight.md)
* [takeRightWhile](takeRightWhile.md)
* [takeWhile](takeWhile.md)
* [union](union.md)
* [unionBy](unionBy.md)
* [unionWith](unionWith.md)
* [uniq](uniq.md)
* [uniqBy](uniqBy.md)
* [uniqWith](uniqWith.md)
* [filter](filter.md)
* [zip](zip.md)
* [unzipWith](unzipWith.md)
* [without](without.md)
* [xor](xor.md)
* [xorBy](xorBy.md)
* [xorWith](xorWith.md)
* [zip](zip.md)
* [zipObject](zipObject.md)
* [zipObjectDeep](zipObjectDeep.md)
* [zipWith](zipWith.md)
* [isBuffer](isBuffer.md)
* [isTypedArray](isTypedArray.md)
* [keys](keys.md)
* [reduce](reduce.md)
* [countBy](countBy.md)
* [forEach](forEach.md)
* [forEachRight](forEachRight.md)
* [every](every.md)
* [findLast](findLast.md)
* [flatMap](flatMap.md)
* [flatMapDeep](flatMapDeep.md)
* [flatMapDepth](flatMapDepth.md)
* [groupBy](groupBy.md)
* [invoke](invoke.md)
* [invokeMap](invokeMap.md)
* [keyBy](keyBy.md)
* [orderBy](orderBy.md)
* [partition](partition.md)
* [reduceRight](reduceRight.md)
* [filterObject](filterObject.md)
* [negate](negate.md)
* [reject](reject.md)
* [sample](sample.md)
* [sampleSize](sampleSize.md)
* [shuffle](shuffle.md)
* [isString](isString.md)
* [size](size.md)
* [after](after.md)
* [before](before.md)
* [debounce](debounce.md)
* [throttle](throttle.md)
* [defer](defer.md)
* [delay](delay.md)
* [flip](flip.md)
* [once](once.md)
* [overArgs](overArgs.md)
* [castArray](castArray.md)
* [keysIn](keysIn.md)
* [clone](clone.md)
* [cloneDeep](cloneDeep.md)
* [cloneDeepWith](cloneDeepWith.md)
* [cloneWith](cloneWith.md)
* [conformsTo](conformsTo.md)
* [gt](gt.md)
* [gte](gte.md)
* [isArrayBuffer](isArrayBuffer.md)
* [isBoolean](isBoolean.md)
* [isDate](isDate.md)
* [isPlainObject](isPlainObject.md)
* [isElement](isElement.md)
* [isEmpty](isEmpty.md)
* [some](some.md)
* [eqDeep](eqDeep.md)
* [isEqualWith](isEqualWith.md)
* [isError](isError.md)
* [isFunction](isFunction.md)
* [isMap](isMap.md)
* [isMatch](isMatch.md)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：

  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 
```flow
start=>start: key
type=>condition: string/number/
symbol/boolean?
proto=>condition: 等于'__proto__'?
null=>condition: 为null?
string=>condition: string类型?
support=>condition: 支持Map?
keyable=>operation: 可用作对象的key
notKeyable=>operation: 不可用作对象的key
hashString=>operation: Hash
hash=>operation: Hash
map=>operation: Map
listCache=>operation: ListCache
end=>end: MapCache

start->type(yes)->proto(yes)->notKeyable
type(yes)->proto(no)->keyable
type(no)->null(yes)->keyable
type(no)->null(no)->notKeyable
keyable->string(yes)->hashString->end
keyable->string(no)->hash->end
notKeyable->support(yes)->map->end
notKeyable->support(no)->listCache->end
```


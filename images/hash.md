```mermaid
graph LR
	Hash-.->size
	size-.->缓存数量
	Hash-.->get
	get-.->获取缓存
	Hash-.->set
	set-.->设置缓存
	Hash-.->has
	has-.->检测缓存是否存在
	Hash-.->delete
	delete-.->删除缓存
	Hash-.->clear
	clear-.->清空缓存
```
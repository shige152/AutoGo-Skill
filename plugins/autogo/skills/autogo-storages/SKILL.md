---
name: "storages - 本地存储"
description: "提供了保存简单数据、用户配置等的支持。保存的数据除非应用被卸载或者被主动删除,否则会一直保留。当用户需要存储、检索或管理持久数据时调用。"
---

# Storages 模块

提供了保存简单数据、用户配置等的支持。保存的数据除非应用被卸载或者被主动删除,否则会一直保留。

## API 函数

### Get

从本地存储中取出键值为 key 的数据。

**参数：**
- `table` {string} 要操作的表。
- `key` {string} 要查询的键。

**示例：**
```go
value := storages.Get("data", "username")
fmt.Println("获取到的值:", value)
```

### Put

把值 value 保存到本地存储中。

**参数：**
- `table` {string} 要操作的表。
- `key` {string} 要保存的键。
- `value` {string} 要保存的值。

**示例：**
```go
storages.Put("data", "username", "JohnDoe")
```

### Remove

移除键值为 key 的数据。

**参数：**
- `table` {string} 要操作的表。
- `key` {string} 要移除的键。

**示例：**
```go
storages.Remove("data", "username")
```

### Contains

返回该本地存储是否包含键值为 key 的数据。

**参数：**
- `table` {string} 要操作的表。
- `key` {string} 要检查的键。

**示例：**
```go
exists := storages.Contains("data", "username")
if exists {
    fmt.Println("键存在!")
} else {
    fmt.Println("键不存在!")
}
```

### GetAll

获取该本地存储所有键值对。

**参数：**
- `table` {string} 要操作的表。

**示例：**
```go
data := storages.GetAll("data")
for k, v := range data {
    fmt.Printf("%s = %s\n", k, v)
}
```

### Clear

移除该本地存储的所有数据。

**参数：**
- `table` {string} 要操作的表。

**示例：**
```go
storages.Clear("data")
fmt.Println("所有存储数据已被清除!")
```

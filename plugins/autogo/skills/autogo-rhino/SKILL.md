---
name: "rhino - JS脚本引擎"
description: "提供 JavaScript 脚本执行能力,基于 Rhino 引擎实现。当用户需要执行 JavaScript 代码或集成 JavaScript 功能时调用。"
---

# Rhino 模块

提供 JavaScript 脚本执行能力,基于 Rhino 引擎实现。

## API 函数

### Eval

执行指定的 JavaScript 脚本,并返回执行结果。

**参数：**
- `contextId` {string} 执行上下文的标识符,用于区分不同的脚本运行环境(可用于隔离变量作用域或缓存)。
- `script` {string} 要执行的 JavaScript 代码字符串。

**示例：**
```go
fmt.Println(rhino.Eval("script", `importClass(android.os.Build);Build.MODEL`))
```

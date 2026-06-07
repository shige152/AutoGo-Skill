---
name: "utils - 工具函数"
description: "提供一组常用的工具函数,包括日志记录、字符串与数据类型的转换、随机数生成等功能。当用户需要执行shell命令、显示消息、转换数据类型或执行常用工具操作时调用。"
---

# Utils 模块

提供一组常用的工具函数,包括日志记录、字符串与数据类型的转换、随机数生成等功能。

## 函数速查

| 分类 | 函数 | 简述 |
|------|------|------|
| 系统 | Shell | 执行shell命令 |
| 提示 | Toast / Alert | Toast提示/弹窗确认 |
| 等待 | Sleep | 暂停执行（毫秒） |
| 随机 | Random | 生成随机整数 |
| 日志 | LogI / LogE | INFO/ERROR级别日志 |
| 类型转换 | I2s / S2i | int↔string |
| 类型转换 | F2s / S2f | float64↔string |
| 类型转换 | B2s / S2b | bool↔string |

## API 函数

### Shell

执行 shell 命令并返回输出。

**参数：**
- `cmd` {string} 要执行的命令。

**示例：**
```go
output := utils.Shell("ls -l")
fmt.Println("Command Output:", output)
```

### Toast

显示 Toast 提示信息。

**参数：**
- `message` {string} 要显示的提示信息。
- `x` {int} 在界面上显示的 X 坐标（传递 -1 使用默认坐标）。
- `y` {int} 在界面上显示的 Y 坐标（传递 -1 使用默认坐标）。
- `duration` {int} 提示显示的持续时间,单位为毫秒（传递 -1 使用默认 2000 毫秒）。

**示例：**
```go
utils.Toast("Hello AutoGo", -1, -1, -1)
```

### Alert

显示带标题、内容和按钮的弹窗,阻塞等待用户点击后返回按钮索引,安卓 15 及以上只有在 APK 模式下才能正常弹出。

**参数：**
- `title` {string} 弹窗标题。
- `content` {string} 弹窗内容。
- `btn1Text` {string} 第一个按钮的文字(通常为"取消"),输入空字符串默认不显示该按钮。
- `btn2Text` {string} 第二个按钮的文字(通常为"确定")。

**返回值：**
- {int} 用户点击的按钮索引（第一个按钮返回 0,第二个按钮返回 1）。

**示例：**
```go
btnIndex := utils.Alert("确认操作", "重置脚本数据？", "取消", "确认")
if btnIndex == 1 {
    fmt.Println("用户点击了确认")
} else {
    fmt.Println("用户点击了取消")
}
```

### Sleep

让当前线程暂停执行指定的时间。

**参数：**
- `i` {int} 暂停时间（毫秒）。

**示例：**
```go
utils.Sleep(500) // 暂停 500 毫秒
```

### Random

返回指定范围内的真随机整数,包含最小值和最大值。

**参数：**
- `min` {int} 最小值。
- `max` {int} 最大值。

**示例：**
```go
randNum := utils.Random(1, 10)
fmt.Println("Random Number:", randNum)
```

### LogI

记录一条 INFO 级别的日志。

**参数：**
- `label` {string} 日志标签,用于标识日志类别。
- `message` {...interface{}} 日志消息,描述具体的日志内容。

**示例：**
```go
utils.LogI("AppStart", "Application has started successfully.")
```

### LogE

记录一条 ERROR 级别的日志。

**参数：**
- `label` {string} 日志标签,用于标识日志类别。
- `message` {...interface{}} 日志消息,描述具体的日志内容。

**示例：**
```go
utils.LogE("AppCrash", "Application encountered an unexpected error.")
```

### I2s

将整数转换为字符串。

**参数：**
- `i` {int} 要转换的整数。

**示例：**
```go
str := utils.I2s(123)
fmt.Println("String Value:", str)
```

### S2i

将字符串转换为整数。

**参数：**
- `s` {string} 要转换的字符串。

**示例：**
```go
num := utils.S2i("123")
fmt.Println("Integer Value:", num)
```

### F2s

将浮点数转换为字符串。

**参数：**
- `f` {float64} 要转换的浮点数。

**示例：**
```go
str := utils.F2s(123.45)
fmt.Println("String Value:", str)
```

### S2f

将字符串转换为浮点数。如果转换失败返回 0.0。

**参数：**
- `s` {string} 要转换的字符串。

**示例：**
```go
num := utils.S2f("123.45")
fmt.Println("Float Value:", num)
```

### B2s

将布尔值转换为字符串 ("true" 或 "false")。

**参数：**
- `b` {bool} 要转换的布尔值。

**示例：**
```go
str := utils.B2s(true)
fmt.Println("Boolean as String:", str)
```

### S2b

将字符串转换为布尔值。如果无法转换则返回 false。

**参数：**
- `s` {string} 要转换的字符串。

**示例：**
```go
boolVal := utils.S2b("true")
fmt.Println("Boolean Value:", boolVal)
```

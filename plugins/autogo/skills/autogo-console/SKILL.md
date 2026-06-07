---
name: "console - 控制台"
description: "提供控制台悬浮窗控制接口，支持多实例创建、窗口位置大小设置、背景颜色、文字颜色大小配置、日志打印输出等。当用户需要显示调试信息、打印日志、查看运行输出时调用。"
---

# Console 模块

提供用于控制台悬浮窗的控制接口，支持多实例、位置、大小、颜色设置以及内容打印等功能。

## API 函数

### New

创建一个新的控制台实例。

**返回值：**
- `*Console` 控制台实例指针

**示例：**

```go
c := console.New()
```

### SetWindowSize

设置控制台窗口的宽高。

**参数：**
- `width` {int} 控制台窗口的宽度
- `height` {int} 控制台窗口的高度

**返回值：**
- `*Console` 控制台实例指针

**示例：**

```go
c.SetWindowSize(800, 600)
```

### SetWindowPosition

设置控制台窗口的位置。

**参数：**
- `x` {int} 控制台窗口左上角的横坐标
- `y` {int} 控制台窗口左上角的纵坐标

**返回值：**
- `*Console` 控制台实例指针

**示例：**

```go
c.SetWindowPosition(100, 200)
```

### SetWindowColor

设置控制台窗口的背景颜色。

**参数：**
- `color` {string} 背景颜色的十六进制字符串，格式如 "#1E1F22"

**返回值：**
- `*Console` 控制台实例指针

**示例：**

```go
c.SetWindowColor("#1E1F22")
```

### SetTextColor

设置控制台文字颜色。

**参数：**
- `color` {string} 文字颜色的十六进制字符串，格式如 "#FFFFFF"

**返回值：**
- `*Console` 控制台实例指针

**示例：**

```go
c.SetTextColor("#FFFFFF")
```

### SetTextSize

设置控制台文字大小。

**参数：**
- `size` {int} 文字大小

**示例：**

```go
c.SetTextSize(50)
```

### Println

打印文本到控制台。

**参数：**
- `a` {any} 要打印的参数，支持多个参数，行为类似 fmt.Println

**示例：**

```go
c.Println("Hello, world!")
c.Println("用户ID:", 123, "状态:", "在线")
```

### Clear

清空控制台内容。

**示例：**

```go
c.Clear()
```

### Show

显示控制台窗口。

**示例：**

```go
c.Show()
```

### Hide

隐藏控制台窗口。

**示例：**

```go
c.Hide()
```

### IsVisible

检查控制台是否可见。

**返回值：**
- `bool` true 表示可见，false 表示隐藏

**示例：**

```go
if c.IsVisible() {
    // 控制台当前可见
}
```

### Destroy

销毁控制台实例，释放资源。

**示例：**

```go
c.Destroy()
```

## 完整示例

```go
// 创建并配置控制台
c := console.New()
c.SetWindowPosition(50, 50)
c.SetWindowSize(700, 500)
c.SetWindowColor("#1E1F22")
c.SetTextColor("#00FF00")
c.SetTextSize(45)

// 打印日志
c.Println("控制台已就绪")

for {
    c.Println("当前时间:", time.Now().Format("2006-01-02 15:04:05"))
    utils.Sleep(1000)
}
```

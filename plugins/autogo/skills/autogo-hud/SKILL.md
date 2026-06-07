---
name: "hud - 悬浮显示"
description: "提供悬浮显示功能，支持多实例、多色文本、背景颜色、文字大小设置、显示隐藏控制等。当用户需要在屏幕上悬浮显示文字信息、状态提示、实时数据展示时调用。"
---

# Hud 模块

提供悬浮显示功能，支持多实例、彩色文本显示等功能。

## TextItem 结构体

TextItem 结构体用于定义HUD中的文本项，包含以下字段：

| 字段名 | 类型 | 说明 |
|--------|------|------|
| TextColor | color.Color | 文字颜色，格式如 "#FFFFFF" |
| Text | string | 显示的文本内容 |

## API 函数

### New

创建一个新的 HUD 实例。

**返回值：**
- `*HUD` HUD 实例指针

**示例：**

```go
h := hud.New()
```

### SetPosition

设置 HUD 的位置和大小。

**参数：**
- `x1` {int} 左上角横坐标
- `y1` {int} 左上角纵坐标
- `x2` {int} 右下角横坐标
- `y2` {int} 右下角纵坐标

**返回值：**
- `*HUD` HUD 实例指针

**示例：**

```go
h.SetPosition(100, 100, 400, 150)
```

### SetBackgroundColor

设置 HUD 的背景颜色。

**参数：**
- `color` {string} 背景颜色的十六进制字符串，格式如 "#2D2D30" 或 "#2D2D3080"（带透明度）

**返回值：**
- `*HUD` HUD 实例指针

**示例：**

```go
h.SetBackgroundColor("#2D2D30")
h.SetBackgroundColor("#00000080")  // 半透明黑色
```

### SetTextSize

设置 HUD 的字体大小。

**参数：**
- `size` {int} 字体大小（推荐范围：30-60）

**返回值：**
- `*HUD` HUD 实例指针

**示例：**

```go
h.SetTextSize(45)
```

### SetText

设置 HUD 显示的文本内容（支持多色文本）。

**参数：**
- `items` {[]TextItem} 文本项数组，每个元素包含颜色和文本

**返回值：**
- `*HUD` HUD 实例指针

**示例：**

```go
h.SetText([]hud.TextItem{
    {TextColor: "#00FF00", Text: "HP: "},
    {TextColor: "#FFFFFF", Text: "100/100"},
})
```

### Show

显示 HUD。

**返回值：**
- `*HUD` HUD 实例指针

**示例：**

```go
h.Show()
```

### Hide

隐藏 HUD。

**返回值：**
- `*HUD` HUD 实例指针

**示例：**

```go
h.Hide()
```

### IsVisible

检查 HUD 是否可见。

**返回值：**
- `bool` true 表示可见，false 表示隐藏

**示例：**

```go
if h.IsVisible() {
    // HUD 当前可见
}
```

### Destroy

销毁 HUD 实例，释放资源。

**示例：**

```go
h.Destroy()
```

## 完整示例

```go
// 创建并配置 HUD
h := hud.New()
h.SetPosition(50, 700, 450, 750)
h.SetBackgroundColor("#3D000080")
h.SetTextSize(45)

for {
    // 设置多色文本
    h.SetText([]hud.TextItem{
        {TextColor: "#00FF00", Text: "当前时间: "},
        {TextColor: "#FFFFFF", Text: time.Now().Format("2006-01-02 15:04:05")},
    })
    utils.Sleep(1000)
}
```

---
name: "vdisplay - 虚拟屏幕"
description: "提供虚拟屏幕创建和管理功能，需Android 10+。支持创建虚拟屏幕、启动应用、预览窗口、触摸回调、多应用并行操作、全分辨率等。当用户需要创建虚拟屏幕、后台运行应用、多开应用、自动化测试时调用。"
---

# Vdisplay 模块

vdisplay模块提供了虚拟屏幕的创建和管理功能，需要Android 10及以上版本。通过该模块可以创建虚拟屏幕，用于在虚拟屏幕中启动和操作应用程序，实现多应用并行操作和完美全分辨率。

## API 函数

### Create

创建一个虚拟屏幕。

**参数：**
- `width` {int} 屏幕宽度
- `height` {int} 屏幕高度
- `dpi` {int} 屏幕的像素密度

**返回值：**
- `*Vdisplay` 虚拟屏幕对象，如果创建失败则返回 nil

**注意：**
- 可以通过Scrcpy执行命令查看虚拟屏幕的实时画面：scrcpy --display-id=虚拟屏幕ID

**示例：**

```go
v := vdisplay.Create(720, 1280, 320)
if v != nil {
    fmt.Printf("创建成功,屏幕ID:%d\n", v.GetDisplayId())
}
```

### GetDisplayId

获取虚拟屏幕的ID。

**返回值：**
- `int` 虚拟屏幕的ID

**示例：**

```go
displayId := v.GetDisplayId()
fmt.Printf("虚拟屏幕ID: %d\n", displayId)
```

### LaunchApp

在虚拟屏幕中启动指定的应用程序。

**参数：**
- `packageName` {string} 要启动的应用包名

**返回值：**
- `bool` 成功返回 true，失败返回 false

**示例：**

```go
success := v.LaunchApp("com.example.app")
if success {
    fmt.Println("应用启动成功")
}
```

### SetTitle

设置虚拟屏预览窗口的标题。

**参数：**
- `title` {string} 标题

**示例：**

```go
v.SetTitle("MT管理器")
```

### SetTouchCallback

设置一个点击回调。

**参数：**
- `callback` {function} 当用户点击虚拟屏预览窗口时调用的函数，格式为 func(x, y, action, displayId int)，如果传入 nil，则会移除当前设置的回调

**示例：**

```go
v.SetTouchCallback(func(x, y, action, displayId int) {
    // 处理用户点击事件
})
```

### ShowPreviewWindow

显示虚拟屏幕的预览窗口。预览窗口可以在ImgUI界面中显示虚拟屏幕的实时画面，并支持触摸操作。

**参数：**
- `rotated` {bool} 是否将预览窗口旋转90度显示(显示横屏游戏画面时可能需要)

**示例：**

```go
v.ShowPreviewWindow(false)
```

### HidePreviewWindow

隐藏虚拟屏幕的预览窗口。

**示例：**

```go
v.HidePreviewWindow()
```

### SetPreviewWindowSize

设置预览窗口的大小。

**参数：**
- `width` {int} 预览窗口宽度（像素）
- `height` {int} 预览窗口高度（像素）

**示例：**

```go
v.SetPreviewWindowSize(800, 600)
```

### SetPreviewWindowPos

设置预览窗口的位置。

**参数：**
- `x` {int} 预览窗口X坐标
- `y` {int} 预览窗口Y坐标

**示例：**

```go
v.SetPreviewWindowPos(100, 100)
```

### Destroy

销毁虚拟屏幕，释放相关资源。

**示例：**

```go
v.Destroy()
```

## 使用场景

虚拟屏幕功能适用于以下场景：

1. **多应用并行操作**：可以在虚拟屏幕中运行另一个应用，同时主屏幕运行当前应用
2. **完美全分辨率**：虚拟屏幕可以设置为任意分辨率，不受物理屏幕限制
3. **后台操作**：在虚拟屏幕中运行应用，不需要在主屏幕显示
4. **自动化测试**：可以同时控制多个应用实例进行测试

## 完整示例

```go
// 创建虚拟屏幕
v := vdisplay.Create(720, 1280, 320)
if v == nil {
    fmt.Println("创建虚拟屏幕失败")
    return
}
defer v.Destroy()

// 设置预览窗口标题
v.SetTitle("游戏助手")

// 显示预览窗口
v.ShowPreviewWindow(false)
v.SetPreviewWindowSize(400, 700)
v.SetPreviewWindowPos(10, 10)

// 设置触摸回调
v.SetTouchCallback(func(x, y, action, displayId int) {
    fmt.Printf("点击位置: (%d, %d), 动作: %d, 屏幕: %d\n", x, y, action, displayId)
})

// 在虚拟屏幕中启动应用
success := v.LaunchApp("com.example.game")
if success {
    fmt.Println("游戏启动成功")
}

// 使用虚拟屏幕进行操作
// 可以使用其他模块的 displayId 参数来指定在虚拟屏幕上操作
// 例如：motion.Click(100, 200, v.GetDisplayId())
```

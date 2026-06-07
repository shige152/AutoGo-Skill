---
name: "imgui - 即时模式图形用户界面"
description: "提供基于Dear ImGui的图形用户界面功能，支持窗口创建、按钮、文本、样式设置、颜色主题等。方法数量众多，完整方法列表参照Dear ImGui官方文档。当用户需要创建图形界面、自定义UI窗口、制作工具面板时调用。"
---

# ImGui 模块

提供基于 Dear ImGui 的图形用户界面功能。由于 ImGui 方法数量众多，完整的方法列表请参照 Dear ImGui 官方文档。

## 基础示例

```go
package main

import (
    "fmt"
    "github.com/Dasongzi1366/AutoGo/imgui"
)

func main() {
    // 初始化
    imgui.Init()
    
    // 状态变量
    counter := 0
    showWindow := true
    
    // 主循环
    imgui.Run(func() {
        // 设置窗口
        imgui.SetNextWindowSizeV(imgui.Vec2{X: 500, Y: 400}, imgui.CondOnce)
        imgui.SetNextWindowPosV(imgui.Vec2{X: 100, Y: 100}, imgui.CondOnce, imgui.Vec2{X: 0, Y: 0})
        
        // 创建窗口
        imgui.BeginV("示例窗口", &showWindow, 0)
        
        // 标题
        imgui.Text("ImGui 示例程序")
        imgui.Separator()
        imgui.Spacing()
        
        // 计数器
        imgui.Text(fmt.Sprintf("计数器: %d", counter))
        
        // 按钮
        if imgui.Button("增加") {
            counter++
        }
        imgui.SameLine()
        if imgui.Button("减少") {
            counter--
        }
        imgui.SameLine()
        if imgui.Button("重置") {
            counter = 0
        }
        
        imgui.Spacing()
        imgui.Separator()
        imgui.Spacing()
        
        // 样式化按钮
        imgui.Text("样式化按钮：")
        imgui.PushStyleColorVec4(imgui.ColButton, imgui.Vec4{X: 0.2, Y: 0.8, Z: 0.2, W: 1.0})
        if imgui.Button("绿色按钮") {
            // 绿色按钮的操作
        }
        imgui.PopStyleColor()
        
        imgui.SameLine()
        imgui.PushStyleColorVec4(imgui.ColButton, imgui.Vec4{X: 0.8, Y: 0.2, Z: 0.2, W: 1.0})
        if imgui.Button("红色按钮") {
            // 红色按钮的操作
        }
        imgui.PopStyleColor()
        
        // 结束窗口
        imgui.End()
    })

    // 阻塞主进程防止程序退出
    select {}
}
```

## 常用方法

### 初始化和运行

- `Init()` - 初始化 ImGui
- `Run(updateFunc func()) - 运行主循环

### 窗口管理

- `Begin(name string, p_open *bool, flags int)` - 开始一个窗口
- `BeginV(name string, p_open *bool, flags int)` - 带变参版本的 Begin
- `End()` - 结束当前窗口
- `SetNextWindowSize(size Vec2, cond Cond)` - 设置下一个窗口的大小
- `SetNextWindowPos(pos Vec2, cond Cond, pivot Vec2)` - 设置下一个窗口的位置

### 基础控件

- `Text(fmt string, args ...interface{})` - 显示文本
- `Button(label string)` - 创建按钮，返回是否被点击
- `SameLine(offsetFromStart float, spacing float)` - 在同一行继续
- `NewLine()` - 换行
- `Spacing()` - 添加垂直间距
- `Separator()` - 添加分隔线

### 样式设置

- `PushStyleColorVec4(idx Col, col Vec4)` - 推入颜色样式
- `PopStyleColor(count int)` - 弹出颜色样式
- `PushStyleVarVec2(idx StyleVar, val Vec2)` - 推入样式变量
- `PopStyleVar(count int)` - 弹出样式变量

### 常用常量

**窗口条件 (Cond):**
- `CondOnce` - 仅设置一次
- `CondFirstUseEver` - 首次使用时设置
- `CondAppearing` - 窗口出现时设置
- `CondAlways` - 总是设置

**颜色索引 (Col):**
- `ColText` - 文本颜色
- `ColWindowBg` - 窗口背景
- `ColButton` - 按钮颜色
- `ColButtonHovered` - 按钮悬停颜色
- `ColButtonActive` - 按钮激活颜色

**样式变量 (StyleVar):**
- `StyleVarWindowPadding` - 窗口内边距
- `StyleVarItemSpacing` - 控件间距
- `StyleVarFramePadding` - 控件内边距

### 数据类型

**Vec2 结构体：**
```go
type Vec2 struct {
    X float32
    Y float32
}
```

**Vec4 结构体：**
```go
type Vec4 struct {
    X float32
    Y float32
    Z float32
    W float32
}
```

## 更多资源

由于 ImGui 提供了大量控件和方法，完整的 API 文档请参考：
- Dear ImGui 官方文档：https://github.com/ocornut/imgui
- ImGui 使用指南和示例

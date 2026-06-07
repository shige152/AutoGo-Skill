---
name: "autogo-imgui"
description: "提供基于Dear ImGui的图形用户界面功能，支持窗口创建、按钮、文本、样式设置、颜色主题、移动端窗口适配、选项卡状态管理和截图反馈修正。当用户需要创建图形界面、自定义UI窗口、制作工具面板时调用。"
---

# ImGui 模块

提供基于 Dear ImGui 的图形用户界面功能。由于 ImGui 方法数量众多，完整的方法列表请参照 Dear ImGui 官方文档。

在 AutoGo 项目中，ImGui 界面运行在 Android 设备上。设计窗口、布局、选项卡和视觉样式时，必须以设备实际 viewport 和截图反馈为准。

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
        // 设置窗口：使用设备可用区域，避免固定尺寸在窄屏设备上被裁切
        windowPos, windowSize := nextWindowPlacement()
        imgui.SetNextWindowSizeV(windowSize, imgui.CondAlways)
        imgui.SetNextWindowPosV(windowPos, imgui.CondAlways, imgui.Vec2{X: 0, Y: 0})
        imgui.SetNextWindowBgAlpha(1.0)
        
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

func nextWindowPlacement() (imgui.Vec2, imgui.Vec2) {
    viewport := imgui.MainViewport()
    workPos := viewport.WorkPos()
    workSize := viewport.WorkSize()

    if workSize.X <= 0 || workSize.Y <= 0 {
        return imgui.Vec2{X: 8, Y: 8}, imgui.Vec2{X: 360, Y: 420}
    }

    return imgui.Vec2{X: workPos.X + 8, Y: workPos.Y + 8}, imgui.Vec2{
        X: clampFloat(workSize.X-16, 360, 720),
        Y: clampFloat(workSize.Y-16, 420, 900),
    }
}

func clampFloat(value, minValue, maxValue float32) float32 {
    if value < minValue {
        return minValue
    }
    if value > maxValue {
        return maxValue
    }
    return value
}
```

## 移动端布局规则

- 不要在 Android 设备上固定使用 `760x620`、`500x400` 这类桌面窗口尺寸；优先用 `imgui.MainViewport().WorkPos()` 和 `imgui.MainViewport().WorkSize()` 计算窗口位置与大小
- `SetNextWindowSizeV` 和 `SetNextWindowPosV` 在需要跟随设备尺寸变化时使用 `CondAlways`；只适合一次性初始化的尺寸才使用 `CondOnce`
- 窄屏设备优先保证宽度不超过 `WorkSize.X`，高度按用户要求选择全屏、固定上限或可用高度比例
- 表单、进度条、输入框等宽度使用 `imgui.ContentRegionAvail().X` 计算，不要写死右侧列宽
- 窗口内容超出时，先压缩文案和字段布局；再考虑滚动区域，不要让右侧控件被裁切

## 选项卡状态管理

ImGui 是即时模式 GUI。选项卡（Tab）选择应由持久状态驱动，但 `TabItemFlagsSetSelected` 只能用于一次性程序跳转，不能每帧强制设置。

推荐模式：

```go
forceTabSelect := s.forceTabSelect
targetView := s.activeView

if imgui.BeginTabBarV("main-navigation", imgui.TabBarFlagsFittingPolicyScroll) {
    if imgui.BeginTabItemV("登录", nil, selectedTabFlag(forceTabSelect && targetView == viewLogin)) {
        s.activeView = viewLogin
        s.renderLoginPanel()
        imgui.EndTabItem()
    }
    if imgui.BeginTabItemV("设置", nil, selectedTabFlag(forceTabSelect && targetView == viewSettings)) {
        s.activeView = viewSettings
        s.renderSettingsPanel()
        imgui.EndTabItem()
    }
    imgui.EndTabBar()
}

if forceTabSelect {
    s.forceTabSelect = false
}

func (s *uiState) selectView(view appView) {
    s.activeView = view
    s.forceTabSelect = true
}

func selectedTabFlag(selected bool) imgui.TabItemFlags {
    if selected {
        return imgui.TabItemFlagsSetSelected
    }
    return imgui.TabItemFlagsNone
}
```

执行规则：

- 用户点击选项卡时，在 `BeginTabItemV` 返回 true 的分支里更新 `activeView`
- 登录成功、菜单跳转等程序切换时，调用 `selectView`，只让下一帧带 `TabItemFlagsSetSelected`
- 一帧提交完选项卡后立刻清空 `forceTabSelect`，避免覆盖用户后续点击
- 不要在遍历每个 Tab 时直接按业务状态每帧传入 `TabItemFlagsSetSelected`

## 文案、主题与截图核验

- 移动端 ImGui 文案要短，优先使用字段式说明和值展示，避免长段说明自然换行后出现标点在行首
- 标题、Tab 名称和按钮文案要短；长说明放到状态区或分多行字段，不要挤在控件同一行
- 工具类界面优先使用不透明窗口背景，减少默认半透明效果导致的可读性问题
- 视觉层级用有限强调色区分主要操作、激活状态和错误状态；不要只保留默认 ImGui 灰色层级
- 修改布局或视觉后，必须通过 AutoGo MCP 运行到设备，并用截图确认没有裁切、重叠、标点行首和控件不可见问题

## 常用方法

### 初始化和运行

- `Init()` - 初始化 ImGui
- `Run(updateFunc func())` - 运行主循环

### 窗口管理

- `Begin(name string, p_open *bool, flags int)` - 开始一个窗口
- `BeginV(name string, p_open *bool, flags int)` - 带变参版本的 Begin
- `End()` - 结束当前窗口
- `SetNextWindowSize(size Vec2, cond Cond)` - 设置下一个窗口的大小
- `SetNextWindowPos(pos Vec2, cond Cond, pivot Vec2)` - 设置下一个窗口的位置
- `SetNextWindowSizeV(size Vec2, cond Cond)` - 设置下一个窗口的大小（变参版本）
- `SetNextWindowPosV(pos Vec2, cond Cond, pivot Vec2)` - 设置下一个窗口的位置（变参版本）
- `SetNextWindowBgAlpha(alpha float32)` - 设置下一个窗口背景透明度
- `MainViewport()` - 获取主 viewport，用于读取 `WorkPos()` 和 `WorkSize()`

### 基础控件

- `Text(fmt string, args ...interface{})` - 显示文本
- `Button(label string)` - 创建按钮，返回是否被点击
- `SameLine(offsetFromStart float, spacing float)` - 在同一行继续
- `NewLine()` - 换行
- `Spacing()` - 添加垂直间距
- `Separator()` - 添加分隔线
- `ContentRegionAvail()` - 获取当前可用内容区域尺寸

### 选项卡

- `BeginTabBar(str_id string)` - 开始一个选项卡栏
- `BeginTabBarV(str_id string, flags TabBarFlags)` - 开始一个带 flags 的选项卡栏
- `BeginTabItem(label string)` - 开始一个选项卡
- `BeginTabItemV(label string, p_open *bool, flags TabItemFlags)` - 开始一个带 flags 的选项卡
- `EndTabItem()` - 结束当前选项卡
- `EndTabBar()` - 结束当前选项卡栏

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

**选项卡标志 (TabItemFlags):**
- `TabItemFlagsNone` - 无特殊行为
- `TabItemFlagsSetSelected` - 本帧请求选中该选项卡，仅用于一次性程序跳转

**选项卡栏标志 (TabBarFlags):**
- `TabBarFlagsFittingPolicyScroll` - 选项卡过多或窄屏时允许滚动适配

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

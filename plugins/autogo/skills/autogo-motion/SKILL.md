---
name: "motion - 操作"
description: "提供触摸模拟和按键操作API，包括点击、长按、滑动、贝塞尔曲线滑动、手势操作、Home键、返回键、音量键、通知栏、快速设置等。当用户需要模拟点击、滑动、按键或手势操作时调用。"
---

# Motion 模块 API 文档

motion 模块提供了一系列模拟用户操作的函数，如点击、滑动、按键等。

## 函数速查

| 分类 | 函数 | 简述 |
|------|------|------|
| 触摸 | TouchDown / TouchMove / TouchUp | 触摸按下/移动/抬起 |
| 点击 | Click | 模拟单击 |
| 长按 | LongClick | 模拟长按（指定持续时间） |
| 滑动 | Swipe | 直线滑动 |
| 滑动 | Swipe2 | 贝塞尔曲线滑动（轨迹更自然） |
| 按键 | Home / Back / Recents | Home键/返回键/最近任务 |
| 按键 | VolumeUp / VolumeDown | 音量上/下键 |
| 按键 | Camera / PowerDialog | 照相键/电源菜单 |
| 系统 | Notifications / QuickSettings | 拉出通知栏/快速设置 |
| 通用 | KeyAction | 模拟指定按键（KEYCODE） |

## API 函数

### TouchDown

模拟触摸屏按下操作。

**参数：**
- x {int} 触摸点的 X 坐标
- y {int} 触摸点的 Y 坐标
- fingerID {int} 触摸点的指针 ID（0-9）
- displayId {int} 屏幕 ID，0 表示主屏幕，其他值表示虚拟屏幕

```go
motion.TouchDown(500, 600, 0, 0)
```

### TouchMove

模拟触摸屏移动操作。

**参数：**
- x {int} 移动到的 X 坐标
- y {int} 移动到的 Y 坐标
- fingerID {int} 触摸点的指针 ID（0-9）
- displayId {int} 屏幕 ID，0 表示主屏幕，其他值表示虚拟屏幕

```go
motion.TouchMove(550, 650, 0, 0)
```

### TouchUp

模拟触摸屏抬起操作。

**参数：**
- x {int} 抬起点的 X 坐标
- y {int} 抬起点的 Y 坐标
- fingerID {int} 触摸点的指针 ID（0-9）
- displayId {int} 屏幕 ID，0 表示主屏幕，其他值表示虚拟屏幕

```go
motion.TouchUp(550, 650, 0, 0)
```

### Click

模拟单击操作。

**参数：**
- x {int} 单击点的 X 坐标
- y {int} 单击点的 Y 坐标
- fingerID {int} 触摸点的指针 ID（0-9）
- displayId {int} 屏幕 ID，0 表示主屏幕，其他值表示虚拟屏幕

```go
motion.Click(500, 600, 0, 0)
```

### LongClick

模拟长按操作。

**参数：**
- x {int} 长按点的 X 坐标
- y {int} 长按点的 Y 坐标
- duration {int} 长按持续时间（毫秒）
- fingerID {int} 触摸点的指针 ID（0-9）
- displayId {int} 屏幕 ID，0 表示主屏幕，其他值表示虚拟屏幕

```go
motion.LongClick(500, 600, 1000, 0, 0)  // 长按 1 秒
```

### Swipe

模拟滑动操作。

**参数：**
- x1 {int} 起始点的 X 坐标
- y1 {int} 起始点的 Y 坐标
- x2 {int} 结束点的 X 坐标
- y2 {int} 结束点的 Y 坐标
- duration {int} 滑动持续时间（毫秒）
- fingerID {int} 触摸点的指针 ID（0-9）
- displayId {int} 屏幕 ID，0 表示主屏幕，其他值表示虚拟屏幕

```go
motion.Swipe(300, 800, 300, 200, 500, 0, 0)  // 从下往上滑动
```

### Swipe2

使用贝塞尔曲线方式进行滑动（轨迹更加自然）。

**参数：**
- x1 {int} 起始点的 X 坐标
- y1 {int} 起始点的 Y 坐标
- x2 {int} 结束点的 X 坐标
- y2 {int} 结束点的 Y 坐标
- duration {int} 滑动持续时间（毫秒）
- fingerID {int} 触摸点的指针 ID（0-9）
- displayId {int} 屏幕 ID，0 表示主屏幕，其他值表示虚拟屏幕

```go
motion.Swipe2(300, 800, 300, 200, 500, 0, 0)  // 从下往上滑动，轨迹更自然
```

### Home

模拟按下 Home 键。

**参数：**
- displayId {int} 屏幕 ID，0 表示主屏幕，其他值表示虚拟屏幕

```go
motion.Home(0)
```

### Back

模拟按下返回键。

**参数：**
- displayId {int} 屏幕 ID，0 表示主屏幕，其他值表示虚拟屏幕

```go
motion.Back(0)
```

### Recents

显示最近任务。

**参数：**
- displayId {int} 屏幕 ID，0 表示主屏幕，其他值表示虚拟屏幕

```go
motion.Recents(0)
```

### PowerDialog

弹出电源键菜单。

```go
motion.PowerDialog()
```

### Notifications

拉出通知栏。

```go
motion.Notifications()
```

### QuickSettings

显示快速设置（下拉通知栏到底）。

```go
motion.QuickSettings()
```

### VolumeUp

按下音量上键。

**参数：**
- displayId {int} 屏幕 ID，0 表示主屏幕，其他值表示虚拟屏幕

```go
motion.VolumeUp(0)
```

### VolumeDown

按下音量下键。

**参数：**
- displayId {int} 屏幕 ID，0 表示主屏幕，其他值表示虚拟屏幕

```go
motion.VolumeDown(0)
```

### Camera

模拟按下照相键。

```go
motion.Camera()
```

### KeyAction

模拟按下指定按键。

**参数：**
- code {int} 按键代码，参考 KEYCODE_常量
- displayId {int} 屏幕 ID，0 表示主屏幕，其他值表示虚拟屏幕

```go
motion.KeyAction(motion.KEYCODE_ENTER, 0)  // 按下回车键
```

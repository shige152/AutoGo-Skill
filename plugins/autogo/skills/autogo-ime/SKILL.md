---
name: "ime - 输入法"
description: "提供输入法控制API，包括文本输入、剪贴板操作（复制/粘贴）、输入法切换等。当用户需要输入文字、管理剪贴板内容或控制输入法行为时调用。"
---

# Ime 模块 API 文档

ime 模块提供了一系列函数，用于控制输入法行为，实现文本输入等功能。

## API 函数

### InputText

使用输入法输入文本。

**参数：**
- text {string} 需要输入的文本
- displayId {int} 屏幕 ID，0 表示主屏幕，其他值表示虚拟屏幕

```go
ime.InputText("Hello, World!", 0)
```

### GetClipText

获取剪贴板文本内容。

**返回值：**
- text {string} 剪贴板文本内容

```go
text := ime.GetClipText()
fmt.Println("剪贴板内容:", text)
```

### SetClipText

设置剪贴板文本内容。

**参数：**
- text {string} 要设置的文本

```go
ime.SetClipText("这是要复制到剪贴板的内容")
```

### GetIMEList

获取输入法列表。

**返回值：**
- []string 输入法列表

```go
fmt.Println("输入法列表:", ime.GetIMEList())
```

### SetCurrentIME

设置系统当前输入法。

**参数：**
- packageName {string} 要设置为当前输入法的应用包名

```go
ime.SetCurrentIME("com.android.inputmethod.latin")
```

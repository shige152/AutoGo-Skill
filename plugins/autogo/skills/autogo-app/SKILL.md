---
name: "app - 应用"
description: "提供应用交互与管理API，包括启动应用、打开链接、发送广播、Intent意图、获取应用列表、打开文件等。当用户需要启动或切换应用、发送Intent、与其他应用交互时调用。"
---

# App 模块 API 文档

app 模块提供一系列函数，用于使用其他应用、与其他应用交互。例如启动应用、打开文件、发送意图等。

同时提供了方便的进阶函数 startActivity 和 sendBroadcast，用他们可完成 app 模块没有内置的和其他应用的交互。

## 函数速查

| 分类 | 函数 | 简述 |
|------|------|------|
| 当前应用 | CurrentPackage / CurrentActivity | 获取当前包名/Activity类名 |
| 启动 | Launch | 通过包名启动应用 |
| 应用信息 | GetList / GetName / GetVersion / GetIcon | 应用列表/名称/版本/图标 |
| 安装管理 | Install / Uninstall / IsInstalled | 安装/卸载/判断已安装 |
| 应用控制 | ForceStop / Clear / Disable / Enable | 强停/清数据/禁用/启用 |
| 无障碍 | EnableAccessibility / DisableAccessibility | 启用/关闭无障碍服务 |
| 设置 | OpenSetting / IgnoreBattOpt | 打开设置页/忽略电池优化 |
| 文件 | ViewFile / EditFile | 用其他应用查看/编辑文件 |
| 浏览器 | GetBrowserPackage / OpenUrl | 获取浏览器包名/打开网址 |
| Intent | StartActivity / SendBroadcast / StartService | 启动Activity/发送广播/启动服务 |

## IntentOptions 结构体

以下是 app 包中定义的 IntentOptions 结构体及其字段说明：

| 字段名 | 类型 | 说明 |
|--------|------|------|
| Action | string | Intent 的动作，例如 android.intent.action.VIEW |
| Type | string | Intent 的数据类型，例如 text/plain 或 image/* |
| Data | string | Intent 的数据，例如文件路径或 URI |
| Category | []string | Intent 的类别 |
| PackageName | string | 应用包名，用于指定目标应用 |
| Extras | map[string]string | Intent 的额外参数（键值对） |
| Flags | []string | Intent 的标志，例如 FLAG_ACTIVITY_NEW_TASK |

## API 函数

### CurrentPackage

获取当前页面的应用包名。

```go
packageName := app.CurrentPackage()
```

### CurrentActivity

获取当前页面的应用类名。

```go
activityName := app.CurrentActivity()
```

### Launch

通过应用包名启动应用。

**参数：**
- packageName {string} 应用包名，也支持"包名/类名"格式
- displayId {int} 屏幕 ID

```go
success := app.Launch("com.tencent.mm", 0)
```

### GetList

获取手机中所有应用列表。

**参数：**
- includeSystemApps {bool} 是否需要包含系统应用

```go
list := app.GetList(true)
```

### GetName

获取指定包名应用的应用名称。

**参数：**
- packageName {string} 应用包名

```go
name := app.GetName("bin.mt.plus")
```

### GetIcon

获取应用图标。

**参数：**
- packageName {string} 应用包名

```go
data := app.GetIcon("com.tencent.mm")
```

### GetVersion

获取应用版本号。

**参数：**
- packageName {string} 应用包名

```go
version := app.GetVersion("com.tencent.mm")
```

### OpenSetting

打开应用的详情页（设置页）。

**参数：**
- packageName {string} 应用包名

```go
success := app.OpenSetting("com.tencent.mm")
```

### ViewFile

用其他应用查看文件。文件不存在的情况由查看文件的应用处理。

**参数：**
- path {string} 文件路径

```go
app.ViewFile("/sdcard/example.txt")
```

### EditFile

用其他应用编辑文件。文件不存在的情况由编辑文件的应用处理。

**参数：**
- path {string} 文件路径

```go
app.EditFile("/sdcard/example.txt")
```

### Uninstall

卸载应用。

**参数：**
- packageName {string} 应用包名

```go
app.Uninstall("com.tencent.mm")
```

### Install

安装应用（也支持 xapk）。

**参数：**
- path {string} APK 文件路径

```go
app.Install("/sdcard/app.apk")
```

### IsInstalled

判断是否已经安装某个应用。

**参数：**
- packageName {string} 应用包名

```go
installed := app.IsInstalled("com.tencent.mm")
```

### Clear

清除应用数据。

**参数：**
- packageName {string} 应用包名

```go
app.Clear("com.tencent.mm")
```

### ForceStop

强制停止应用。

**参数：**
- packageName {string} 应用包名

```go
app.ForceStop("com.tencent.mm")
```

### Disable

禁用应用。

**参数：**
- packageName {string} 应用包名

```go
app.Disable("com.tencent.mm")
```

### Enable

启用应用。

**参数：**
- packageName {string} 应用包名

```go
app.Enable("com.tencent.mm")
```

### EnableAccessibility

启用无障碍服务。

**参数：**
- packageName {string} 应用包名

```go
app.EnableAccessibility("org.autojs.autoxjs.v6")
```

### DisableAccessibility

关闭无障碍服务。

**参数：**
- packageName {string} 应用包名

```go
app.DisableAccessibility("org.autojs.autoxjs.v6")
```

### IgnoreBattOpt

忽略应用电池优化。

**参数：**
- packageName {string} 应用包名

```go
app.IgnoreBattOpt("com.tencent.mm")
```

### GetBrowserPackage

获取系统默认浏览器包名。

```go
packageName := app.GetBrowserPackage()
```

### OpenUrl

用浏览器打开指定的网址。

**参数：**
- url {string} 网站地址

```go
app.OpenUrl("https://example.com")
```

### StartActivity

根据选项构造一个 Intent，并启动该 Activity。

**参数：**
- options {IntentOptions} Intent 选项

```go
app.StartActivity(app.IntentOptions{
    Action: "SEND",
    Type:   "text/plain",
    Data:   "file:///sdcard/1.txt",
})
```

### SendBroadcast

根据选项构造一个 Intent，并发送广播。

**参数：**
- options {IntentOptions} Intent 选项

```go
app.SendBroadcast(options)
```

### StartService

根据选项构造一个 Intent，并启动服务。

**参数：**
- options {IntentOptions} Intent 选项

```go
app.StartService(options)
```

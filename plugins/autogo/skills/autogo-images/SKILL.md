---
name: "images - 图像处理"
description: "提供截图和图像处理API，包括屏幕截图、区域截图、像素颜色获取、颜色查找、图片比较、图片保存、灰度转换、二值化、图像裁剪等。当用户需要截屏、找色、找图或处理图像时调用。"
---

# Images 模块 API 文档

images 模块提供了截图、图像处理、颜色查找等功能。

## 函数速查

| 分类 | 函数 | 简述 |
|------|------|------|
| 截图 | CaptureScreen | 截取屏幕指定区域 |
| 回调 | SetCallback | 设置新图像到达回调 |
| 像素 | Pixel | 获取指定坐标颜色值 |
| 比色 | CmpColor | 比较坐标点颜色 |
| 找色 | FindColor | 在区域内查找目标颜色 |
| 找色 | GetColorCountInRegion | 计算区域内颜色数量 |
| 多点比色 | DetectsMultiColors | 多点颜色比对 |
| 多点找色 | FindMultiColors / FindMultiColorsAll | 多点找色（单个/全部） |
| 读取 | ReadFromPath / ReadFromBase64 / ReadFromBytes | 从路径/Base64/字节读取图片 |
| 保存 | Save | 保存图片到路径 |
| 编码 | EncodeToBase64 / EncodeToBytes | 编码为Base64/字节数组 |
| 转换 | ToNrgba | 转换为NRGBA格式 |
| 处理 | Clip / Resize / Rotate | 裁剪/缩放/旋转 |
| 处理 | Grayscale / ApplyBinarization | 灰度/二值化 |
| 处理 | ApplyThreshold / ApplyAdaptiveThreshold | 阈值/自适应阈值 |

## API 函数

### SetCallback

设置一个新图像数据到达的回调。

**参数：**
- callback {function} 当新图像数据到达时调用的函数，格式为 `func(img *image.NRGBA, displayId int)`，如果传入 nil，则会移除当前设置的回调

```go
images.SetCallback(func(img *image.NRGBA, displayId int) {
    // 处理新图像数据
})
```

**注意事项：**
- 回调函数应避免执行耗时操作，否则可能导致后续图像数据处理延迟
- 回调函数内部如需进行耗时操作（如文件写入或网络请求），建议启动新的 goroutine 处理，避免阻塞回调执行

### CaptureScreen

截取屏幕的指定区域。

**参数：**
- x1 {int} 区域左上角的 x 坐标
- y1 {int} 区域左上角的 y 坐标
- x2 {int} 区域右下角的 x 坐标，当为 0 时表示使用屏幕最大宽度
- y2 {int} 区域右下角的 y 坐标，当为 0 时表示使用屏幕最大高度
- displayId {int} 屏幕 ID，0 表示主屏幕，其他值表示虚拟屏幕

```go
img := images.CaptureScreen(0, 0, 0, 0, 0) // 截取主屏幕全屏
```

### Pixel

获取指定坐标点的颜色值。

**参数：**
- x {int} 坐标点的 x 位置
- y {int} 坐标点的 y 位置
- displayId {int} 屏幕 ID，0 表示主屏幕，其他值表示虚拟屏幕

```go
color := images.Pixel(100, 200, 0)
```

### CmpColor

比较指定坐标点 (x, y) 的颜色。

**参数：**
- x {int} 坐标点的 x 位置
- y {int} 坐标点的 y 位置
- colorStr {string} 颜色字符串，格式如 "FFFFFF|CCCCCC-101010"，每种颜色用 "|" 分割，"-" 后表示偏色
- sim {float32} 相似度，取值范围 0.1-1.0
- displayId {int} 屏幕 ID，0 表示主屏幕，其他值表示虚拟屏幕

```go
matched := images.CmpColor(100, 200, "FFFFFF|CCCCCC-101010", 0.9, 0)
```

### FindColor

在指定区域内查找目标颜色。

**参数：**
- x1 {int} 区域左上角的 x 坐标
- y1 {int} 区域左上角的 y 坐标
- x2 {int} 区域右下角的 x 坐标，当为 0 时表示使用屏幕最大宽度
- y2 {int} 区域右下角的 y 坐标，当为 0 时表示使用屏幕最大高度
- colorStr {string} 颜色格式串，例如 "FFFFFF|CCCCCC-101010"
- sim {float32} 相似度，取值范围 0.1-1.0
- dir {int} 查找方向，0-3 分别表示不同的查找方向
- displayId {int} 屏幕 ID，0 表示主屏幕，其他值表示虚拟屏幕

**查找方向说明：**
- 0 - 从左到右，从上到下
- 1 - 从右到左，从上到下
- 2 - 从左到右，从下到上
- 3 - 从右到左，从下到上

```go
x, y := images.FindColor(0, 0, 0, 0, "FFFFFF", 0.9, 0, 0)
```

### GetColorCountInRegion

计算指定区域内符合颜色条件的像素数量。

**参数：**
- x1 {int} 区域左上角的 x 坐标
- y1 {int} 区域左上角的 y 坐标
- x2 {int} 区域右下角的 x 坐标，当为 0 时表示使用屏幕最大宽度
- y2 {int} 区域右下角的 y 坐标，当为 0 时表示使用屏幕最大高度
- colorStr {string} 要查找的颜色字符串，例如 "FFFFFF|CCCCCC-101010"
- sim {float32} 相似度，取值范围 0.1-1.0
- displayId {int} 屏幕 ID，0 表示主屏幕，其他值表示虚拟屏幕

```go
count := images.GetColorCountInRegion(0, 0, 0, 0, "FFFFFF", 0.9, 0)
```

### DetectsMultiColors

根据指定的颜色串信息在屏幕进行多点颜色比对（多点比色）。

**参数：**
- colors {string} 颜色模板字符串，例如 "369,1220,ffab2d-101010,370,1221,24b1ff-101010,380,390,907efd-101010"
- sim {float32} 相似度，取值范围 0.1-1.0
- displayId {int} 屏幕 ID，0 表示主屏幕，其他值表示虚拟屏幕

```go
matched := images.DetectsMultiColors("369,1220,ffab2d-101010,370,1221,24b1ff-101010", 0.9, 0)
```

### FindMultiColors

在指定区域内查找匹配的多点颜色序列（多点找色）。

**参数：**
- x1 {int} 区域左上角的 x 坐标
- y1 {int} 区域左上角的 y 坐标
- x2 {int} 区域右下角的 x 坐标，当为 0 时表示使用屏幕最大宽度
- y2 {int} 区域右下角的 y 坐标，当为 0 时表示使用屏幕最大高度
- colors {string} 颜色模板字符串，例如 "ffccff-151515,635,978,ffab2d-101010,6,29,24b1ff-101010"
- sim {float32} 相似度，取值范围 0.1-1.0
- dir {int} 查找方向，0-3 分别表示不同的查找方向
- displayId {int} 屏幕 ID，0 表示主屏幕，其他值表示虚拟屏幕

```go
x, y := images.FindMultiColors(0, 0, 0, 0, "ffccff-151515,635,978,ffab2d-101010", 0.9, 0, 0)
```

### FindMultiColorsAll

在指定区域内查找匹配的多点颜色序列并返回所有符合条件的坐标。

**参数：**
- x1 {int} 区域左上角的 x 坐标
- y1 {int} 区域左上角的 y 坐标
- x2 {int} 区域右下角的 x 坐标，当为 0 时表示使用屏幕最大宽度
- y2 {int} 区域右下角的 y 坐标，当为 0 时表示使用屏幕最大高度
- colors {string} 颜色模板字符串，例如 "ffccff-151515,635,978,ffab2d-101010,6,29,24b1ff-101010"
- sim {float32} 相似度，取值范围 0.1-1.0
- dir {int} 查找方向，0-3 分别表示不同的查找方向
- displayId {int} 屏幕 ID，0 表示主屏幕，其他值表示虚拟屏幕

```go
points := images.FindMultiColorsAll(0, 0, 0, 0, "ffccff-151515,635,978,ffab2d-101010", 0.9, 0, 0)
```

### ReadFromPath

读取路径指定的图片文件并返回图像对象。

**参数：**
- path {string} 要读取的图片文件路径

```go
img := images.ReadFromPath("/sdcard/image.png")
```

### ReadFromBase64

解码 Base64 数据并返回解码后的图片对象。

**参数：**
- base64Str {string} 要解码的 Base64 字符串

```go
img := images.ReadFromBase64("iVBORw0KGgoAAAANSUhEUgAAAAUA...")
```

### ReadFromBytes

解码字节数组并返回解码后的图片对象。

**参数：**
- data {[]byte} 要解码的字节数组

```go
img := images.ReadFromBytes(bytes)
```

### Save

把图片保存到指定路径。

**参数：**
- img {*image.NRGBA} 要保存的图像对象
- path {string} 保存图片的文件路径
- quality {int} 保存图片的质量（如果适用）

```go
success := images.Save(img, "/sdcard/saved.png", 100)
```

### EncodeToBase64

把图像对象编码为 Base64 数据。

**参数：**
- img {*image.NRGBA} 要编码的图像对象
- format {string} 编码的图片格式（如 "png", "jpg" 等）
- quality {int} 编码的图片质量

```go
base64Str := images.EncodeToBase64(img, "png", 100)
```

### EncodeToBytes

把图片编码为字节数组。

**参数：**
- img {*image.NRGBA} 要编码的图像对象
- format {string} 编码的图片格式（如 "png", "jpg" 等）
- quality {int} 编码的图片质量

```go
bytes := images.EncodeToBytes(img, "png", 100)
```

### ToNrgba

将任意 image.Image 对象转换为 *image.NRGBA。

**参数：**
- img {image.Image} 要转换的图像对象

```go
nrgbaImg := images.ToNrgba(anyImg)
```

### Clip

从源图像中裁剪指定区域并返回新图像。

**参数：**
- img {*image.NRGBA} 要裁剪的图像
- x1 {int} 裁剪区域左上角 x 坐标
- y1 {int} 裁剪区域左上角 y 坐标
- x2 {int} 裁剪区域右下角 x 坐标
- y2 {int} 裁剪区域右下角 y 坐标

```go
clippedImg := images.Clip(img, 100, 100, 300, 300)
```

### Resize

调整图像大小。

**参数：**
- img {*image.NRGBA} 要调整的图像
- width {int} 目标宽度
- height {int} 目标高度

```go
resizedImg := images.Resize(img, 800, 600)
```

### Rotate

旋转图像。

**参数：**
- img {*image.NRGBA} 要旋转的图像
- degree {int} 旋转角度（顺时针方向）

```go
rotatedImg := images.Rotate(img, 90)
```

### Grayscale

将彩色图像转换为灰度图像。

**参数：**
- img {*image.NRGBA} 要转换的彩色图像

```go
grayImg := images.Grayscale(img)
```

### ApplyThreshold

对图像应用阈值处理。

**参数：**
- img {*image.NRGBA} 要处理的图像
- threshold {int} 阈值
- maxVal {int} 超过阈值后应用的值
- typ {string} 阈值类型，如 "BINARY", "BINARY_INV" 等

```go
thresholdImg := images.ApplyThreshold(img, 128, 255, "BINARY")
```

### ApplyAdaptiveThreshold

应用自适应阈值处理。

**参数：**
- img {*image.NRGBA} 要处理的图像
- maxValue {float64} 最大值
- adaptiveMethod {string} 自适应方法，如 "MEAN_C", "GAUSSIAN_C"
- thresholdType {string} 阈值类型，如 "BINARY", "BINARY_INV"
- blockSize {int} 用于计算阈值的像素邻域大小
- C {float64} 从平均值或加权平均值中减去的常量

```go
adaptiveImg := images.ApplyAdaptiveThreshold(img, 255, "MEAN_C", "BINARY", 11, 2)
```

### ApplyBinarization

应用二值化处理。

**参数：**
- img {*image.NRGBA} 要处理的图像
- threshold {int} 阈值

```go
binaryImg := images.ApplyBinarization(img, 128)
```

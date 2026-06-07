---
name: "dotocr - 点阵 OCR"
description: "提供基于模板匹配的点阵OCR文字识别API，包括文字检测、文字位置查找、字符识别等。当用户需要进行点阵字体识别、验证码识别、查找文字位置时调用。"
---

# Dotocr 模块 API 文档

提供基于模板匹配的文字识别和查找功能。

## 函数速查

| 分类 | 函数 | 简述 |
|------|------|------|
| 字库 | SetDict | 设置字库 |
| OCR识别 | Ocr | 屏幕区域OCR识别 |
| OCR识别 | OcrFromImage / OcrFromBase64 / OcrFromPath | 从图像/Base64/路径识别 |
| 查找文字 | FindStr | 屏幕区域查找文字位置 |
| 查找文字 | FindStrFromImage / FindStrFromBase64 / FindStrFromPath | 从图像/Base64/路径查找文字 |

## API 函数

### SetDict

设置字库。字库内容按行分割，每行一条模板记录。

**参数：**
- name {string} 字库名称，为空字符串时使用 "default"
- dict {string} 字库内容字符串，按行分割，每行一条模板记录

```go
dotocr.SetDict("字库 1", dictContent)
```

### Ocr

在屏幕指定区域进行 OCR 文字识别。

**参数：**
- x1 {int} 区域左上角的 x 坐标
- y1 {int} 区域左上角的 y 坐标
- x2 {int} 区域右下角的 x 坐标，当为 0 时表示使用屏幕最大宽度
- y2 {int} 区域右下角的 y 坐标，当为 0 时表示使用屏幕最大高度
- threshold {string} 阈值字符串，例如 "ffffff-101010"
- colGap {int} 列方向允许的连通间隙（用于合并近邻矩形）
- rowGap {int} 行方向允许的连通间隙
- sim {float32} 匹配相似度阈值，取值范围 0.0-1.0，例如 0.8
- mode {int} 返回模式，0 返回纯文本，1 返回 JSON 格式
- dictName {string} 使用的字库名称，为空字符串时使用 "default"
- displayId {int} 屏幕 ID，0 表示主屏幕，其他值表示虚拟屏幕

**返回值：**
- 当 mode == 0 时，返回纯文本字符串，按检测顺序拼接所有识别到的字符
- 当 mode == 1 时，返回 JSON 数组字符串，每个元素包含 {"x":坐标 x, "y":坐标 y, "width":宽度，"height":高度，"text":文字内容，"sim":相似度}

```go
// 返回纯文本
text := dotocr.Ocr(0, 0, 100, 50, "ffffff-101010", 1, 1, 0.8, 0, "字库 1", 0)

// 返回 JSON 格式
jsonStr := dotocr.Ocr(0, 0, 100, 50, "ffffff-101010", 1, 1, 0.8, 1, "字库 1", 0)
```

### OcrFromImage

从图像对象进行 OCR 文字识别。

**参数：**
- img {*image.NRGBA} 要识别的图像对象
- threshold {string} 阈值字符串
- colGap {int} 列方向允许的连通间隙
- rowGap {int} 行方向允许的连通间隙
- sim {float32} 匹配相似度阈值
- mode {int} 返回模式，0 返回纯文本，1 返回 JSON 格式
- dictName {string} 使用的字库名称

**返回值：**
- 识别结果字符串（纯文本或 JSON 格式）

```go
img := images.ReadFromPath("/sdcard/screenshot.png")
text := dotocr.OcrFromImage(img, "ffffff-101010", 1, 1, 0.8, 0, "字库 1")
```

### OcrFromBase64

从 Base64 编码的图像数据进行 OCR 文字识别。

**参数：**
- b64 {string} Base64 编码的图像数据
- threshold {string} 阈值字符串
- colGap {int} 列方向允许的连通间隙
- rowGap {int} 行方向允许的连通间隙
- sim {float32} 匹配相似度阈值
- mode {int} 返回模式，0 返回纯文本，1 返回 JSON 格式
- dictName {string} 使用的字库名称

**返回值：**
- 识别结果字符串（纯文本或 JSON 格式）

```go
text := dotocr.OcrFromBase64(base64String, "ffffff-101010", 1, 1, 0.8, 0, "字库 1")
```

### OcrFromPath

从图像文件路径进行 OCR 文字识别。

**参数：**
- path {string} 图像文件路径
- threshold {string} 阈值字符串
- colGap {int} 列方向允许的连通间隙
- rowGap {int} 行方向允许的连通间隙
- sim {float32} 匹配相似度阈值
- mode {int} 返回模式，0 返回纯文本，1 返回 JSON 格式
- dictName {string} 使用的字库名称

**返回值：**
- 识别结果字符串（纯文本或 JSON 格式）

```go
text := dotocr.OcrFromPath("/sdcard/screenshot.png", "ffffff-101010", 1, 1, 0.8, 0, "字库 1")
```

### FindStr

在屏幕指定区域中查找指定字符串的位置。

**参数：**
- x1 {int} 查找区域的左上角 x 坐标
- y1 {int} 查找区域的左上角 y 坐标
- x2 {int} 查找区域的右下角 x 坐标，当为 0 时表示使用屏幕最大宽度
- y2 {int} 查找区域的右下角 y 坐标，当为 0 时表示使用屏幕最大高度
- text {string} 要查找的字符串
- threshold {string} 阈值字符串
- colGap {int} 列方向允许的连通间隙
- rowGap {int} 行方向允许的连通间隙
- sim {float32} 匹配相似度阈值
- dictName {string} 使用的字库名称
- displayId {int} 屏幕 ID，0 表示主屏幕，其他值表示虚拟屏幕

**返回值：**
- x {int} 找到时返回字符串第一个字符的 x 坐标，未找到返回 -1
- y {int} 找到时返回字符串第一个字符的 y 坐标，未找到返回 -1

```go
x, y := dotocr.FindStr(0, 0, 0, 0, "商店", "ffffff-101010", 1, 1, 0.8, "字库 1", 0)
if x != -1 && y != -1 {
    fmt.Printf("找到字符串，坐标：%d, %d\n", x, y)
}
```

### FindStrFromImage

在图像对象中查找指定字符串的位置。

**参数：**
- img {*image.NRGBA} 要查找的图像对象
- text {string} 要查找的字符串
- threshold {string} 阈值字符串
- colGap {int} 列方向允许的连通间隙
- rowGap {int} 行方向允许的连通间隙
- sim {float32} 匹配相似度阈值
- dictName {string} 使用的字库名称

**返回值：**
- x {int} 找到时返回字符串第一个字符的 x 坐标，未找到返回 -1
- y {int} 找到时返回字符串第一个字符的 y 坐标，未找到返回 -1

```go
img := images.ReadFromPath("/sdcard/screenshot.png")
x, y := dotocr.FindStrFromImage(img, "商店", "ffffff-101010", 1, 1, 0.8, "字库 1")
```

### FindStrFromBase64

在 Base64 编码的图像中查找指定字符串的位置。

**参数：**
- b64 {string} Base64 编码的图像数据
- text {string} 要查找的字符串
- threshold {string} 阈值字符串
- colGap {int} 列方向允许的连通间隙
- rowGap {int} 行方向允许的连通间隙
- sim {float32} 匹配相似度阈值
- dictName {string} 使用的字库名称

**返回值：**
- x {int} 找到时返回字符串第一个字符的 x 坐标，未找到返回 -1
- y {int} 找到时返回字符串第一个字符的 y 坐标，未找到返回 -1

```go
x, y := dotocr.FindStrFromBase64(base64String, "商店", "ffffff-101010", 1, 1, 0.8, "字库 1")
```

### FindStrFromPath

在图像文件中查找指定字符串的位置。

**参数：**
- path {string} 图像文件路径
- text {string} 要查找的字符串
- threshold {string} 阈值字符串
- colGap {int} 列方向允许的连通间隙
- rowGap {int} 行方向允许的连通间隙
- sim {float32} 匹配相似度阈值
- dictName {string} 使用的字库名称

**返回值：**
- x {int} 找到时返回字符串第一个字符的 x 坐标，未找到返回 -1
- y {int} 找到时返回字符串第一个字符的 y 坐标，未找到返回 -1

```go
x, y := dotocr.FindStrFromPath("/sdcard/screenshot.png", "商店", "ffffff-101010", 1, 1, 0.8, "字库 1")
```

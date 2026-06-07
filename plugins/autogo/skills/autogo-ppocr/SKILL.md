---
name: "ppocr - 飞桨OCR"
description: "提供基于PaddleOCR的文字检测和识别功能，支持屏幕截图OCR、图片OCR、Base64图片OCR、文件路径OCR等。当用户需要文字识别、OCR提取文字、读取图片中的文字时调用。"
---

# Ppocr 模块

提供基于 PaddleOCR 的文字检测和识别功能。

## Result 结构体

Result 结构体用于存储OCR识别结果，包含以下字段：

| 字段名 | 类型 | 说明 |
|--------|------|------|
| X | int | 检测结果的左上角 X 坐标 |
| Y | int | 检测结果的左上角 Y 坐标 |
| Width | int | 检测结果的宽度 |
| Height | int | 检测结果的高度 |
| Label | string | 检测到的文字内容或标签 |
| Score | float64 | 检测结果的置信度，取值范围为 0-1 |
| CenterX | int | 检测结果的中心 X 坐标 |
| CenterY | int | 检测结果的中心 Y 坐标 |

## API 函数

### New

创建一个 Ppocr 实例对象。成功返回实例对象*Ppocr，如果加载失败则返回 nil。

**参数：**
- `version` {string} 模型版本，目前仅支持v2和v5

**返回值：**
- `*Ppocr` 实例对象，加载失败返回 nil

**示例：**

```go
ocr := ppocr.New("v5")
if ocr == nil {
    fmt.Println("初始化失败")
    return
}
fmt.Println("初始化成功")
```

### Ocr

在屏幕指定区域进行OCR文字识别。

**参数：**
- `x1` {int} 区域左上角的 x 坐标
- `y1` {int} 区域左上角的 y 坐标
- `x2` {int} 区域右下角的 x 坐标，当为 0 时表示使用屏幕最大宽度
- `y2` {int} 区域右下角的 y 坐标，当为 0 时表示使用屏幕最大高度
- `colorStr` {string} 文字颜色范围，格式如 "FFFFFF" 或 "FFFFFF-101010"，"-" 后表示偏色范围，如果不需要指定则直接传入空字符串""
- `displayId` {int} 屏幕ID，0表示主屏幕，其他值表示虚拟屏幕

**返回值：**
- `[]Result` 识别结果数组

**示例：**

```go
results := ocr.Ocr(0, 0, 0, 0, "000000", 0) // 识别主屏幕全屏的黑色文字
for _, result := range results {
    fmt.Println(result.Label) // 打印识别到的文字
}
```

### OcrFromImage

从图像对象进行OCR文字识别。

**参数：**
- `img` {*image.NRGBA} 要识别的图像对象
- `colorStr` {string} 文字颜色范围，格式如 "FFFFFF" 或 "FFFFFF-101010"

**返回值：**
- `[]Result` 识别结果数组

**示例：**

```go
img := images.ReadFromPath("/sdcard/screenshot.png")
results := ocr.OcrFromImage(img, "000000")
```

### OcrFromBase64

从Base64编码的图像数据进行OCR文字识别。

**参数：**
- `b64` {string} Base64编码的图像数据
- `colorStr` {string} 文字颜色范围，格式如 "FFFFFF" 或 "FFFFFF-101010"

**返回值：**
- `[]Result` 识别结果数组

**示例：**

```go
results := ocr.OcrFromBase64(base64String, "000000")
```

### OcrFromPath

从图像文件路径进行OCR文字识别。

**参数：**
- `path` {string} 图像文件路径
- `colorStr` {string} 文字颜色范围，格式如 "FFFFFF" 或 "FFFFFF-101010"

**返回值：**
- `[]Result` 识别结果数组

**示例：**

```go
results := ocr.OcrFromPath("/sdcard/screenshot.png", "000000")
```

### Close

关闭 PPOCR 实例。

**示例：**

```go
ocr.Close()
```

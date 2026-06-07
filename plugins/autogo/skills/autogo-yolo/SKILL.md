---
name: "yolo - 目标检测"
description: "提供基于YOLO的目标检测功能，支持YOLOv5和YOLOv8模型，包括屏幕截图检测、图片检测、Base64图片检测、文件路径检测等。当用户需要进行目标检测、物体识别、定位屏幕上的目标时调用。"
---

# Yolo 模块

提供基于 YOLO 的目标检测功能。

## Result 结构体

Result 结构体用于存储目标检测结果，包含以下字段：

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

创建一个 Yolo 实例对象。成功返回实例对象*Yolo，如果加载失败则返回 nil。

**参数：**
- `version` {string} 模型版本，目前仅支持v5和v8
- `cpuThreadNum` {int} 用于模型推理的 CPU 线程数
- `paramPath` {string} 模型参数文件路径
- `binPath` {string} 模型二进制文件路径
- `labels` {string} 标签文本，多个标签使用,进行隔开

**返回值：**
- `*Yolo` 实例对象，加载失败返回 nil

**示例：**

```go
yolo := yolo.New("v8", 4, "/data/local/tmp/param", "/data/local/tmp/bin", "person,bicycle,car")
if yolo == nil {
    fmt.Println("模型加载失败")
    return
}
fmt.Println("模型加载成功")
```

### Detect

在屏幕指定区域进行目标检测。

**参数：**
- `x1` {int} 区域左上角的 x 坐标
- `y1` {int} 区域左上角的 y 坐标
- `x2` {int} 区域右下角的 x 坐标，当为 0 时表示使用屏幕最大宽度
- `y2` {int} 区域右下角的 y 坐标，当为 0 时表示使用屏幕最大高度
- `displayId` {int} 屏幕ID，0表示主屏幕，其他值表示虚拟屏幕

**返回值：**
- `[]Result` 检测结果数组

**示例：**

```go
results := detector.Detect(0, 0, 0, 0, 0) // 在主屏幕全屏范围内检测目标
for _, result := range results {
    fmt.Printf("检测到 %s，置信度: %.2f\n", result.Label, result.Score)
}
```

### DetectFromImage

从图像对象进行目标检测。

**参数：**
- `img` {*image.NRGBA} 要检测的图像对象

**返回值：**
- `[]Result` 检测结果数组

**示例：**

```go
img := images.ReadFromPath("/sdcard/photo.jpg")
results := detector.DetectFromImage(img)
```

### DetectFromBase64

从Base64编码的图像数据进行目标检测。

**参数：**
- `b64` {string} Base64编码的图像数据
- `colorStr` {string} 保留参数，可以传入空字符串

**返回值：**
- `[]Result` 检测结果数组

**示例：**

```go
results := detector.DetectFromBase64(base64String, "")
```

### DetectFromPath

从图像文件路径进行目标检测。

**参数：**
- `path` {string} 图像文件路径
- `colorStr` {string} 保留参数，可以传入空字符串

**返回值：**
- `[]Result` 检测结果数组

**示例：**

```go
results := detector.DetectFromPath("/sdcard/photo.png", "")
```

### Close

关闭 YOLO 模型实例，释放相关资源。

**示例：**

```go
yolo.Close()
```

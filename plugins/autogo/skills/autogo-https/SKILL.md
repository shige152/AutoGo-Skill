---
name: "https - 网络请求"
description: "提供HTTP/HTTPS网络请求API，包括GET、POST、PUT、DELETE请求、文件上传下载、请求头设置、JSON数据处理、Cookie管理等。当用户需要发送网络请求、获取网页内容、上传文件或与Web服务交互时调用。"
---

# Https 模块 API 文档

https 模块提供了发送 HTTP/HTTPS 请求的功能，可用于与网络服务进行交互，获取网页内容，上传文件等。

## API 函数

### Get

发送 GET 请求并返回响应状态码和数据。

**参数：**
- url {string} 请求的 URL
- timeout {int} 请求的超时时间（毫秒），如果为 0 则不设置超时

**返回值：**
- code {int} 响应状态码
- data {[]byte} 响应数据

```go
code, data := https.Get("https://example.com", 5000)
if code == 200 {
    fmt.Println("请求成功：", string(data))
} else {
    fmt.Println("请求失败，状态码：", code)
}
```

### Post

发送 POST 请求并返回响应状态码和数据。支持自定义请求头和请求体，适用于发送 JSON、XML 等格式的数据。

**参数：**
- url {string} 请求的 URL
- data {[]byte} 请求体数据（如 JSON 序列化后的字节数组）
- headers {map[string]string} 自定义请求头，如果为 nil 或未设置 Content-Type，默认使用 application/json
- timeout {int} 请求的超时时间（毫秒），如果为 0 则不设置超时

**返回值：**
- code {int} 响应状态码
- data {[]byte} 响应数据

```go
// 构造 JSON 请求体
reqData := map[string]interface{}{
    "name": "张三",
    "age":  25,
}
jsonData, _ := json.Marshal(reqData)

// 设置请求头
headers := map[string]string{
    "Content-Type":  "application/json",
    "Authorization": "Bearer your-token",
}

// 发送请求
code, data := https.Post("https://example.com/api/user", jsonData, headers, 10000)
if code == 200 {
    fmt.Println("请求成功：", string(data))
} else {
    fmt.Println("请求失败，状态码：", code)
}
```

### PostMultipart

发送带有文件的 POST 请求（multipart/form-data 格式）并返回响应状态码和数据。

**参数：**
- url {string} 请求的 URL
- fileName {string} 文件名
- fileData {[]byte} 文件数据（字节数组）
- timeout {int} 请求的超时时间（毫秒），如果为 0 则不设置超时

**返回值：**
- code {int} 响应状态码
- data {[]byte} 响应数据

```go
// 读取文件数据
fileData := files.ReadBytes("/sdcard/image.jpg")

// 发送请求
code, data := https.PostMultipart("https://example.com/upload", "image.jpg", fileData, 10000)
if code == 200 {
    fmt.Println("上传成功：", string(data))
} else {
    fmt.Println("上传失败，状态码：", code)
}
```

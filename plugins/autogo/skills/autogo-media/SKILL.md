---
name: "media - 多媒体"
description: "提供多媒体编程支持，包括媒体文件扫描、音频播放、短信发送等功能。当用户需要扫描媒体文件、播放音频、处理多媒体内容或发送短信时调用。"
---

# Media 模块

提供多媒体编程的支持,目前仅支持媒体文件扫描,后续会加入更多功能。

## API 函数

### ScanFile

扫描指定文件,将其加入媒体库中。

**参数：**
- `path` {string} 要扫描的文件路径。

**示例：**
```go
media.ScanFile("/sdcard/1.png")
```

### PlayMP3

播放指定路径的 MP3 音频文件。

**参数：**
- `path` {string} 要播放的 MP3 文件路径。

**示例：**
```go
media.PlayMP3("/sdcard/music.mp3")
```

### SendSMS

向指定手机号发送短信。

**参数：**
- `number` {string} 接收短信的目标手机号。
- `message` {string} 要发送的短信内容。

**示例：**
```go
media.SendSMS("10086", "Hello AutoGo")
```

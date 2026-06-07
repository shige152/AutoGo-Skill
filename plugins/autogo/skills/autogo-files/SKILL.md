---
name: "files - 文件操作"
description: "提供文件和文件夹操作API，包括读取、写入、复制、移动、删除、创建目录、判断文件存在、追加写入、列出目录内容等。当用户需要操作文件系统或管理文件目录时调用。"
---

# Files 模块 API 文档

提供文件和文件夹的操作接口，例如读取、写入、移动等。

## 函数速查

| 分类 | 函数 | 简述 |
|------|------|------|
| 判断 | IsFile / IsDir / IsEmptyDir / Exists | 判断文件/目录/空目录/存在 |
| 创建 | Create / EnsureDir | 创建文件/确保目录存在 |
| 读取 | Read / ReadBytes | 读取文本/字节数据 |
| 写入 | Write / WriteBytes | 写入文本/字节数据 |
| 追加 | Append / AppendBytes | 追加文本/字节数据 |
| 操作 | Copy / Move / Rename / Remove | 复制/移动/重命名/删除 |
| 信息 | GetName / GetNameWithoutExtension / GetExtension | 文件名/无扩展名/扩展名 |
| 信息 | GetMd5 / Path | MD5值/转绝对路径 |
| 列表 | ListDir | 列出目录内容 |

## API 函数

### IsFile

判断路径是否是文件。

**参数：**
- path {string} 路径

```go
isFile := files.IsFile("/sdcard/example.txt")
```

### IsDir

判断路径是否是文件夹。

**参数：**
- path {string} 路径

```go
isDir := files.IsDir("/sdcard/example_folder")
```

### IsEmptyDir

判断文件夹是否为空。如果路径不是文件夹，返回 false。

**参数：**
- path {string} 文件夹路径

```go
isEmpty := files.IsEmptyDir("/sdcard/example_folder")
```

### Create

创建文件或文件夹。如果文件已存在，返回 true。

**参数：**
- path {string} 路径

```go
success := files.Create("/sdcard/new_file.txt")
```

### Exists

判断路径是否存在。

**参数：**
- path {string} 路径

```go
exists := files.Exists("/sdcard/example.txt")
```

### EnsureDir

确保文件夹存在，如果不存在则创建。

**参数：**
- path {string} 路径

```go
success := files.EnsureDir("/sdcard/new_folder")
```

### Read

读取文本文件的内容。

**参数：**
- path {string} 文件路径

```go
content := files.Read("/sdcard/example.txt")
```

### ReadBytes

读取文件的字节数据。

**参数：**
- path {string} 文件路径

```go
data := files.ReadBytes("/sdcard/example.txt")
```

### Write

将文本写入文件。如果文件不存在则创建，存在则覆盖。

**参数：**
- path {string} 文件路径
- text {string} 要写入的文本

```go
files.Write("/sdcard/example.txt", "Hello, World!")
```

### WriteBytes

将字节数据写入文件。如果文件不存在则创建，存在则覆盖。

**参数：**
- path {string} 文件路径
- bytes {[]byte} 要写入的字节数据

```go
files.WriteBytes("/sdcard/example.txt", []byte("Hello, World!"))
```

### Append

将文本追加到文件末尾。如果文件不存在则创建。

**参数：**
- path {string} 文件路径
- text {string} 要追加的文本

```go
files.Append("/sdcard/example.txt", "Appended text")
```

### AppendBytes

将字节数据追加到文件末尾。如果文件不存在则创建。

**参数：**
- path {string} 文件路径
- bytes {[]byte} 要追加的字节数据

```go
files.AppendBytes("/sdcard/example.txt", []byte("Appended bytes"))
```

### Copy

复制文件。

**参数：**
- fromPath {string} 源文件路径
- toPath {string} 目标文件路径

```go
success := files.Copy("/sdcard/source.txt", "/sdcard/destination.txt")
```

### Move

移动文件。

**参数：**
- fromPath {string} 源文件路径
- toPath {string} 目标文件路径

```go
success := files.Move("/sdcard/source.txt", "/sdcard/destination.txt")
```

### Rename

重命名文件。

**参数：**
- path {string} 文件路径
- newName {string} 新文件名

```go
success := files.Rename("/sdcard/example.txt", "new_name.txt")
```

### GetName

获取文件名。

**参数：**
- path {string} 文件路径

```go
name := files.GetName("/sdcard/example.txt")
```

### GetNameWithoutExtension

获取不含扩展名的文件名。

**参数：**
- path {string} 文件路径

```go
name := files.GetNameWithoutExtension("/sdcard/example.txt")
```

### GetExtension

获取文件的扩展名。

**参数：**
- path {string} 文件路径

```go
extension := files.GetExtension("/sdcard/example.txt")
```

### GetMd5

获取文件的 MD5 值。

**参数：**
- path {string} 文件路径

```go
md5 := files.GetMd5("/sdcard/example.txt")
```

### Remove

删除文件或文件夹。如果是文件夹，则删除其所有内容。

**参数：**
- path {string} 文件路径或文件夹路径

```go
success := files.Remove("/sdcard/example.txt")
```

### Path

将相对路径转换为绝对路径。

**参数：**
- relativePath {string} 相对路径

```go
absolutePath := files.Path("./example.txt")
```

### ListDir

列出文件夹下的所有文件和文件夹。

**参数：**
- path {string} 文件夹路径

```go
entries := files.ListDir("/sdcard/example_folder")
```

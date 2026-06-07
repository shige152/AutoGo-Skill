---
name: "system - 系统功能"
description: "提供与系统相关的功能,包括进程管理和资源查询。当用户需要管理进程、查询系统资源或控制脚本执行时调用。"
---

# System 模块

提供与系统相关的功能,包括进程管理和资源查询。

## API 函数

### GetPid

获取指定进程的 PID。

**参数：**
- `processName` {string} 进程名。如果为空则返回当前进程的 PID。

**示例：**
```go
pid := system.GetPid("com.example.app")
fmt.Println("Process ID:", pid)
```

### GetMemoryUsage

获取指定进程的内存使用量。

**参数：**
- `pid` {int} 进程的 PID。如果为 0 则获取当前进程的内存使用量。

**示例：**
```go
memoryUsage := system.GetMemoryUsage(12345)
fmt.Println("Memory Usage (KB):", memoryUsage)
```

### GetCpuUsage

获取指定进程的 CPU 使用率。

**参数：**
- `pid` {int} 进程的 PID。如果为 0 则获取当前进程的 CPU 使用率。

**示例：**
```go
cpuUsage := system.GetCpuUsage(12345)
fmt.Println("CPU Usage (%):", cpuUsage)
```

### RestartSelf

重启当前脚本进程。

**示例：**
```go
system.RestartSelf()
```

### SetBootStart

设置脚本开机自动运行,需要 root 权限。

**参数：**
- `enable` {bool} 是否启用开机自启。

**示例：**
```go
system.SetBootStart(true)
```

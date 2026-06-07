---
name: "autogo"
description: "AutoGo MCP 总入口技能。用于设备发现、项目运行、日志跟踪、界面节点读取、ADB 调试等场景的统一导航。先阅读本技能，再按场景使用 autogo-* 子技能或直接调用 autogo_mcp 工具。"
---

# AutoGo MCP 总入口

这个 skill 的职责是说明 `autogo_mcp` 工具应该怎么用，以及不同 `autogo-*` 子 skill 分别负责什么。

它不是 MCP 服务本身，也不会注册工具。
真正可调用的能力来自当前环境里已经接入的 `autogo_mcp` 服务。

## 适用场景

当用户提到以下需求时，优先使用本 skill：

- 运行 AutoGo 项目
- 停止项目
- 持续跟踪运行日志
- 查看已连接设备
- 连接远程 ADB 设备
- 同步 so 或 assets 到设备
- 获取当前屏幕节点树
- 执行 ADB 命令
- 调试设备或调试 UI 自动化流程

## 核心原则

- 先确认设备，再执行依赖设备的操作
- 运行项目后，立刻读取日志，不要只返回 sessionId
- 首次日志可能为空，需要继续轮询
- 明确区分“工具可用”和“项目可运行成功”是两回事
- `projectPath` 应传项目根目录的绝对路径

## 标准流程

### 运行项目

1. 调用 `list_devices`
2. 取出目标设备 `serial`
3. 调用 `run_project(projectPath, device)`
4. 立刻调用 `read_project_logs(sessionId, fromOffset=0)`
5. 若日志为空，继续轮询
6. 向用户汇报编译、部署、启动或报错结果

### 停止项目

1. 若已有设备序列号，直接调用 `stop_project(projectPath, device)`
2. 若没有，先 `list_devices`
3. 再执行停止

### 跟踪日志

1. 首次使用 `fromOffset=0`
2. 后续使用上一次返回的 `nextOffset`
3. 结合 `running` 与 `exitCode` 判断是否结束

### 读取节点树

1. 先确认设备可用
2. 调用 `get_screen_nodes(device, filters...)`
3. 优先向用户展示关键字段：
   - `text`
   - `id`
   - `desc`
   - `className`
   - `bounds`

## 工具职责

- `list_devices`：列出当前可用 ADB 设备
- `connect_device`：连接远程设备，例如 `ip:port`
- `run_project`：编译并运行 AutoGo 项目
- `stop_project`：停止当前项目
- `read_project_logs`：读取运行日志
- `sync_files`：同步 so 与资源文件
- `get_screen_nodes`：抓取当前界面节点树
- `adb_command`：执行任意 ADB 命令

## 常见问题处理

- `Missing device`
  先重新调用 `list_devices`，再重试

- `read_project_logs` 为空
  可能仍在编译、部署或启动，继续轮询

- `run_project` 成功但项目退出
  读取日志，优先判断是编译失败、依赖缺失、权限问题还是脚本异常

- `get_screen_nodes` 失败
  可能是锁屏、安全界面或无障碍不可用，提示用户解锁或切换到可读取界面

- ADB 管道命令报错
  在 Windows 下，把管道放进引号中交给 Android shell 执行，例如：
  `adb shell "dumpsys battery | grep level"`

## 子技能分工

- `autogo-device`
  设备连接、设备状态、基础 ADB 相关操作

- `autogo-uiacc`
  节点树读取、控件定位、界面分析

- `autogo-images`
  图像处理相关能力

- `autogo-opencv`
  OpenCV 图像识别相关能力

- `autogo-ppocr`
  OCR 识别相关能力

- `autogo-files`
  文件读写、路径与资源组织

- `autogo-system`
  系统功能、环境依赖、系统状态排查

## 对 AI 的执行要求

- 只要用户是在问 AutoGo MCP 相关操作，优先按本 skill 的流程走
- 调用 `run_project` 后必须继续跟日志，不要停在“已启动”
- 发现失败时，优先给出真实报错原因，而不是泛泛描述
- 如果需要更多细节，再进入对应 `autogo-*` 子 skill

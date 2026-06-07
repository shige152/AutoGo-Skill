---
name: "autogo"
description: "AutoGo MCP 总入口技能。用于设备发现、项目运行、日志跟踪、界面节点读取、ADB 调试等场景的统一导航。先阅读本技能，再按场景使用 autogo-* 子技能或直接调用 autogo_mcp 工具。"
---

# AutoGo MCP 总入口

这个 skill 的职责是说明 `autogo_mcp` 工具应该怎么用，以及不同 `autogo-*` 子 skill 分别负责什么。

它不是 MCP 服务本身，也不会注册工具。
真正可调用的能力来自当前环境里已经接入的 `autogo_mcp` 服务。

## 硬性前置

- AutoGo 项目运行在 Android 设备上，编译与运行应通过 AutoGo MCP 调用 `C:\Users\Public\ag.exe` 完成。
- 在执行任何 AutoGo 项目验证前，必须先确认 MCP 工具已经暴露，例如能调用 `list_devices`、`run_project`、`read_project_logs`、`stop_project` 或 `adb_command`。
- 如果没有找到 AutoGo MCP 工具，不要改走本机 `go build`、`go test`、CGO、NDK 或 MinGW 编译检查；这些不能代表 AutoGo 手机端运行结果。
- MCP 工具缺失时，应立即向用户说明：当前未加载 AutoGo MCP，需要用户手动安装/启用插件，或检查 `.mcp.json`、`npx -y autogo-mcp`、Codex 插件/MCP 加载状态。
- 只有 MCP 工具可用后，才继续设备发现、项目运行、日志读取和截图/ADB 验证。

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

- MCP 可用性是 AutoGo 项目验证的硬门槛；没有 MCP 时停止并提示排查，不做本机编译替代验证
- 先确认设备，再执行依赖设备的操作
- 运行项目后，立刻读取日志，不要只返回 sessionId
- 首次日志可能为空，需要继续轮询；如果脚本没有打印语句或日志调用，日志为空是正常结果
- 日志持续为空时，不要停在空日志；改用设备进程、截图和节点树验证运行状态
- 明确区分“工具可用”和“项目可运行成功”是两回事
- `projectPath` 应传项目根目录的绝对路径

## 标准流程

### 运行项目

0. 先确认 AutoGo MCP 工具已暴露；若未暴露，停止并提示用户安装/启用/排查 MCP，不要执行本机编译
1. 调用 `list_devices`
2. 取出目标设备 `serial`
3. 调用 `run_project(projectPath, device)`；该流程会通过 `C:\Users\Public\ag.exe` 处理 AutoGo 项目编译与运行
4. 立刻调用 `read_project_logs(sessionId, fromOffset=0)`
5. 若日志为空，先判断代码是否预期会打印日志；没有打印语句或日志调用时，不要把空日志判定为失败
6. 若多次轮询仍无法证明运行状态，转向设备侧验证：
   - 用 `adb_command` 查询 `libgoeval` 相关进程，例如 `adb shell "ps -A | grep libgoeval"`
   - 用 `adb_command` 截图确认界面，例如 `adb shell screencap -p /sdcard/autogo_verify.png`，再 `adb pull /sdcard/autogo_verify.png <local-path>`
   - 可读取节点时，用 `get_screen_nodes` 补充验证关键控件是否出现
7. 向用户汇报编译、部署、启动、日志、进程、截图或报错结果

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

- AutoGo MCP 工具未暴露
  停止当前 AutoGo 项目验证流程，提示用户手动安装/启用 AutoGo MCP 或排查插件加载状态；不要退回本机 Go/CGO/NDK 编译。

- `Missing device`
  先重新调用 `list_devices`，再重试

- `read_project_logs` 为空
  先确认脚本是否有打印语句或日志调用；没有输出代码时，空日志是预期结果。需要继续验证时，改查设备进程、截图或节点树；只有在代码明确会输出日志时，才把持续空日志作为异常线索继续排查。

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
- 找不到 MCP 工具时，必须提示用户安装/启用/排查 MCP；禁止用本机编译、CGO 或 NDK 交叉编译替代 AutoGo 手机端验证
- 调用 `run_project` 后必须继续跟日志，不要停在“已启动”
- 日志为空时必须说明可能原因；没有打印语句时按预期处理，并用进程、截图或节点树完成验证闭环
- 发现失败时，优先给出真实报错原因，而不是泛泛描述
- 如果需要更多细节，再进入对应 `autogo-*` 子 skill

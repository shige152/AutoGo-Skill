---
name: "device - 设备"
description: "提供设备信息与操作API，包括屏幕分辨率、IMEI、Android ID、IP地址、MAC地址、电量、音量、亮度、震动、息屏挂机、唤醒设备、保持屏幕常亮、重启等。当用户需要获取设备信息或控制设备硬件时调用。"
---

# Device 模块 API 文档

device 模块提供了与设备有关的信息与操作，例如获取设备宽高、内存使用率、IMEI，调整设备亮度、音量等。

## 函数速查

| 分类 | 函数 | 简述 |
|------|------|------|
| 屏幕 | GetDisplayInfo | 获取屏幕分辨率/DPI/旋转角度 |
| 屏幕 | WakeUp | 唤醒设备 |
| 屏幕 | KeepScreenOn | 保持屏幕常亮 |
| 屏幕 | SetDisplayPower | 设置屏幕电源（熄屏挂机） |
| 屏幕 | IsScreenOn / IsScreenUnlock | 判断屏幕点亮/解锁状态 |
| 设备标识 | GetImei / GetAndroidId / GetSerial | 获取IMEI/Android ID/序列号 |
| 网络 | GetIp / GetWifiMac / GetWlanMac | 获取IP/MAC地址 |
| 音量 | GetMusicVolume / SetMusicVolume | 获取/设置媒体音量 |
| 音量 | GetNotificationVolume / SetNotificationVolume | 获取/设置通知音量 |
| 音量 | GetAlarmVolume / SetAlarmVolume | 获取/设置闹钟音量 |
| 亮度 | GetBrightness / GetBrightnessMode | 获取亮度值/亮度模式 |
| 电量 | GetBattery / GetBatteryStatus | 获取电量/充电状态 |
| 电量 | SetBatteryLevel / SetBatteryStatus | 模拟设置电量/充电状态 |
| 内存 | GetTotalMem / GetAvailMem | 获取总内存/可用内存 |
| 通知 | GetNotification | 获取所有通知消息 |
| 其他 | Vibrate / CancelVibration | 震动/取消震动 |
| 其他 | Reboot | 重启设备 |

## 设备信息变量

以下是 `device` 包中定义的设备信息变量：

| 变量名 | 类型 | 说明 |
|--------|------|------|
| `CpuAbi` | `string` | 设备的 CPU 架构，如"arm64-v8a", "x86", "x86_64"等 |
| `BuildId` | `string` | 修订版本号，或者诸如"M4-rc20"的标识 |
| `Broad` | `string` | 设备的主板型号 |
| `Brand` | `string` | 与产品或硬件相关的厂商品牌，如"Xiaomi", "Huawei"等 |
| `Device` | `string` | 设备在工业设计中的名称 |
| `Model` | `string` | 设备型号 |
| `Product` | `string` | 整个产品的名称 |
| `Bootloader` | `string` | 设备 Bootloader 的版本 |
| `Hardware` | `string` | 设备的硬件名称 |
| `Fingerprint` | `string` | 构建 (build) 的唯一标识码 |
| `Serial` | `string` | 硬件序列号 |
| `SdkInt` | `int` | 安卓系统 API 版本。例如安卓 4.4 的 sdkInt 为 19 |
| `Incremental` | `string` | 设备构建的内部版本号 |
| `Release` | `string` | Android 系统版本号。例如 "5.0", "7.1.1" |
| `BaseOS` | `string` | 设备的基础操作系统版本 |
| `SecurityPatch` | `string` | 安全补丁程序级别 |
| `Codename` | `string` | 开发代号，例如发行版是"REL" |

## API 函数

### GetDisplayInfo

获取指定屏幕的分辨率信息。

**参数：**
- `displayId` {int} 屏幕 ID，0 表示主屏幕，其他值表示虚拟屏幕

**返回值：**
- width, height, dpi, rotation

```go
width, height, dpi, rotation := device.GetDisplayInfo(0)
fmt.Printf("屏幕分辨率：%dx%d, DPI: %d, 旋转角度：%d\n", width, height, dpi, rotation)
```

### GetImei

获取设备的 IMEI 码。

```go
imei := device.GetImei()
```

### GetAndroidId

获取设备的 Android ID。

```go
androidId := device.GetAndroidId()
```

### GetWifiMac

获取设备 WIFI 网卡的 MAC 地址。

```go
wifiMac := device.GetWifiMac()
```

### GetWlanMac

获取设备以太网网卡的 MAC 地址。

```go
wlanMac := device.GetWlanMac()
```

### GetIp

获取设备局域网 IP 地址。

```go
ip := device.GetIp()
```

### GetNotification

获取设备当前所有通知消息。

```go
notifications := device.GetNotification()
for _, notification := range notifications {
    fmt.Println("ID:" + notification.Id)
    fmt.Println("包名:" + notification.PackageName)
    fmt.Println("标题:" + notification.Title)
    fmt.Println("内容:" + notification.Text)
    fmt.Println("标签:" + notification.Tag)
    fmt.Println()
}
```

### GetBrightness

获取当前屏幕亮度值，范围为 0~255。

```go
brightness := device.GetBrightness()
```

### GetBrightnessMode

获取当前屏幕亮度调节模式，0 为手动调节，1 为自动调节。

```go
mode := device.GetBrightnessMode()
```

### GetMusicVolume

获取当前媒体音量。

```go
volume := device.GetMusicVolume()
```

### GetNotificationVolume

获取当前通知音量。

```go
volume := device.GetNotificationVolume()
```

### GetAlarmVolume

获取当前闹钟音量。

```go
volume := device.GetAlarmVolume()
```

### GetMusicMaxVolume

获取媒体音量最大值。

```go
maxVolume := device.GetMusicMaxVolume()
```

### GetNotificationMaxVolume

获取通知音量最大值。

```go
maxVolume := device.GetNotificationMaxVolume()
```

### GetAlarmMaxVolume

获取闹钟音量最大值。

```go
maxVolume := device.GetAlarmMaxVolume()
```

### SetMusicVolume

设置媒体音量。

**参数：**
- `volume` {int} 要设置的音量值

```go
device.SetMusicVolume(8)
```

### SetNotificationVolume

设置通知音量。

**参数：**
- `volume` {int} 要设置的音量值

```go
device.SetNotificationVolume(8)
```

### SetAlarmVolume

设置闹钟音量。

**参数：**
- `volume` {int} 要设置的音量值

```go
device.SetAlarmVolume(8)
```

### GetBattery

获取当前电量百分比。

```go
battery := device.GetBattery()
```

### GetBatteryStatus

获取电池状态。

**返回值说明：**
- 1：没有充电
- 2：正充电
- 3：没插充电器
- 4：不充电
- 5：电池充满

```go
status := device.GetBatteryStatus()
```

### SetBatteryStatus

模拟设置电池状态。

**参数：**
- `value` {int} 要设置的电池状态（1：没有充电；2：正充电；5：电池充满）

```go
device.SetBatteryStatus(2)
```

### SetBatteryLevel

模拟设置电池电量百分比，范围 0-100。

**参数：**
- `value` {int} 要设置的电池电量百分比

```go
device.SetBatteryLevel(75)
```

### GetTotalMem

获取设备总内存，单位 KB。

```go
totalMem := device.GetTotalMem()
```

### GetAvailMem

获取设备当前可用内存，单位 KB。

```go
availMem := device.GetAvailMem()
```

### IsScreenOn

判断屏幕是否点亮状态。

```go
isOn := device.IsScreenOn()
```

### IsScreenUnlock

判断屏幕是否已解锁。

```go
isUnlock := device.IsScreenUnlock()
```

### SetDisplayPower

设置屏幕电源模式，不影响脚本运行。

**参数：**
- `on` {bool} 是否点亮

```go
device.SetDisplayPower(false) // 熄屏挂机
```

### WakeUp

唤醒设备，包括唤醒 CPU、屏幕等，可以用来点亮屏幕。

```go
device.WakeUp()
```

### Reboot

重启设备。

```go
device.Reboot()
```

### KeepScreenOn

保持屏幕常亮。

```go
device.KeepScreenOn()
```

### Vibrate

使设备震动一段时间（单位毫秒，需要 root 权限）。

**参数：**
- `ms` {int} 要震动的时间（毫秒）

```go
device.Vibrate(500)
```

### CancelVibration

如果设备处于震动状态，则取消震动。

```go
device.CancelVibration()
```

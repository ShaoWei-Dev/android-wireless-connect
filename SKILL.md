---
name: android-wireless-connect
description: 不用数据线也能把 App 装到 Android 设备上测试。当用户想无线导出、无线调试、不想用 USB 线连接手机/平板/XR 设备时使用此 skill。
---

# 无线连接 Android 设备

不用找数据线，把你的 App 装到 Android 设备上测试。

## 工作流

```
连接进度：
[ ] 1. 检查环境是否就绪
[ ] 2. 优先使用 Android 11+ 无线调试配对
[ ] 3. 如果设备不支持配对，再使用 USB + tcpip 兼容方案
[ ] 4. 验证设备已经无线连接
```

### 1. 检查环境

终端输入 `adb --version`。如果报错，说明环境没装好，需要先装 Android SDK（Unity 自带就有）。

### 2. Android 11+ 无线调试配对

如果设备是 Android 11 或更高版本，优先使用系统自带的无线调试配对流程。

在设备上打开：设置 → 开发者选项 → 无线调试。

选择“使用配对码配对设备”，设备会显示一个 `IP:端口` 和一组配对码。

```
adb pair 192.168.x.x:配对端口
```

根据终端提示输入设备上显示的配对码。

配对成功后，回到无线调试主界面，找到真正用于连接的 `IP:端口`，然后执行：

```
adb connect 192.168.x.x:连接端口
```

注意：配对端口和连接端口通常不是同一个端口。`adb pair` 使用配对端口，`adb connect` 使用连接端口。

### 3. USB + tcpip 兼容方案

如果设备没有无线调试配对入口，或者当前 XR 设备只适合先用 USB 授权，可以使用兼容方案。

先插上数据线，终端输入 `adb devices`。设备应该显示为 `device`（不是 `unauthorized`）。如果弹出授权窗口，手机上点确定。

然后切换到无线模式：

```
adb tcpip 5555
```

查询设备 IP：

```
adb shell ip addr show wlan0
```

找到类似 `192.168.x.x` 的那一行。

也可以在设备上手动查：设置 → 关于 → 网络状态 / IP 地址。

然后无线连接：

```
adb connect 192.168.x.x:5555
```

把 `192.168.x.x` 换成上一步查到的 IP。

兼容方案会打开固定的 5555 端口，建议只在可信网络中使用。

### 4. 验证

```
adb devices
```

应该看到 `192.168.x.x:端口` 状态为 `device`。如果你使用的是 USB + tcpip 兼容方案，现在可以拔掉数据线。

## 如果连不上

| 现象 | 怎么办 |
|------|--------|
| 显示 unauthorized | 看手机屏幕，有授权弹窗的话点同意 |
| 显示 connection refused | 先确认 `adb connect` 使用的是连接端口，不是配对端口；如果使用兼容方案，插回数据线重新执行 `adb tcpip 5555` |
| 找不到设备 | 换一根能传数据的 USB 线试试 |

## Development Build 调试

如果想在 Unity 里打断点看变量：导出时勾选 Development Build 和 Script Debugging。详见 [references/development-build.md](references/development-build.md)。

## 收工

```
adb disconnect 192.168.x.x:端口
```

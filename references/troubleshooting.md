# 常见问题

| 现象 | 原因 | 解决 |
|------|------|------|
| unauthorized | 设备未授权 USB 调试 | 检查设备屏幕，确认授权弹窗并勾选"始终允许" |
| connection refused | 使用了错误端口，或兼容方案没有完成 tcpip 切换 | Android 11+ 先确认 `adb connect` 使用的是无线调试主界面的连接端口；兼容方案则插回 USB，重新执行 `adb tcpip 5555` |
| no devices/emulators found | ADB 找不到设备或 PATH 配置错误 | 检查 USB 线是否支持数据传输、检查 `adb --version` 输出路径是否正确 |
| 多个设备同时列出 | USB 和无线端口冲突 | `adb disconnect` 清理后再连接 |
| 重装 Unity 后 adb 失效 | PATH 指向旧 SDK 路径 | 更新环境变量中的路径指向新版本 platform-tools |
| 连接成功但断点不触发 | 未勾选 Script Debugging | Unity 导出时勾选 Development Build + Script Debugging |
| 配对成功但连接失败 | 把配对端口误当成连接端口 | 回到设备的无线调试主界面，使用真正的 `IP:连接端口` 执行 `adb connect` |

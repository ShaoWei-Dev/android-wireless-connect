# Development Build 调试

## 前提

Unity 导出时必须勾选以下两项：

- Development Build
- Script Debugging

## 附加调试工具

Development Build 模式附带以下工具（仅在该模式下可用）：

- Profiler —— 性能分析
- Console —— Debug.Log 输出
- Frame Debug —— 渲染帧调试
- Input Debugger —— 输入调试

## 断点调试

无线连接建立后：

1. 在 VS / Rider 中打开项目
2. 在目标代码行设置断点
3. 附加到 Unity 调试器（Attach to Unity Player），选择代表 Android 设备的进程
4. Unity 执行到断点时会自动暂停，可查看变量、单步执行

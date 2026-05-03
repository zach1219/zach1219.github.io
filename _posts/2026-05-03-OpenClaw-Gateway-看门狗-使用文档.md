---
title: 'OpenClaw Gateway 看门狗 使用文档'
date: 2026-05-03
permalink: /posts/2026/05/03/OpenClaw-Gateway-看门狗-使用文档/
tags:
  - 飞书文档
---


1. 部署（创建定时任务）
schtasks /Create /TN "OpenClawGatewayWatchdog" /TR "powershell.exe -NoProfile -ExecutionPolicy Bypass -WindowStyle Hidden -File `"C:\Users\zach\.openclaw\workspace\tools\gateway-watchdog\watchdog.ps1`"" /SC MINUTE /MO 5 /RL HIGHEST /F 

每 5 分钟触发一次，最高权限运行。

1. 手动启动看门狗
Start-ScheduledTask -TaskName "OpenClawGatewayWatchdog" 

1. 手动测试脚本（直接运行，Ctrl+C 停止）
powershell -ExecutionPolicy Bypass -File "C:\Users\zach\.openclaw\workspace\tools\gateway-watchdog\watchdog.ps1" 

1. 停止看门狗
# *方式一：禁用任务（保留）* 

schtasks /Change /TN "OpenClawGatewayWatchdog" /DISABLE 

 

# *方式二：直接删除* 

schtasks /Delete /TN "OpenClawGatewayWatchdog" /F 

1. 查看状态
Get-ScheduledTask -TaskName "OpenClawGatewayWatchdog" | Select-Object TaskName, State 

Get-ScheduledTask -TaskName "OpenClawGatewayWatchdog" | Get-ScheduledTaskInfo | Select-Object LastRunTime, NextRunTime, LastTaskResult 

1. 查看日志
Get-Content "C:\Users\zach\.openclaw\workspace\tools\gateway-watchdog\watchdog.log" -Tail 50 

1. 修改配置（编辑 watchdog.ps1 顶部变量）
1. 完全卸载
schtasks /Delete /TN "OpenClawGatewayWatchdog" /F 

Remove-Item "C:\Users\zach\.openclaw\workspace\tools\gateway-watchdog" -Recurse -Force 

核心逻辑简述

- 双重检测：TCP 端口探测（3s 超时）+ WMI 查进程命令行（匹配 openclaw/gateway）
- 连续失败 2 次 → kill 残留进程 → 重新拉起 gateway.cmd
- 重启成功 → 发飞书通知 → 冷却 30 秒
- 全局 Mutex 锁防止多实例并行
- 日志超 10MB 自动轮转
# OpenClaw Gateway 看门狗 使用文档

## 1. 概述

OpenClaw Gateway 看门狗是一个自动监控和恢复工具。它定期检测网关进程是否存活，当发现网关异常时自动重启，并通过飞书机器人发送通知。

**核心问题：网关挂了 → 工具链断 → AI Agent 无法自救 → 需要外部看门狗。**

## 2. 功能特性

- 双重检测：TCP 端口探测（18789）+ WMI 进程命令行匹配
- 自动重启：连续失败 2 次后 kill 残留进程并拉起 gateway.cmd
- 飞书通知：网关恢复后自动发送飞书消息通知
- 日志记录：运行日志写入 watchdog.log，超过 10MB 自动轮转
- 多实例保护：全局 Mutex 锁，防止多个看门狗并行运行
- 冷却保护：重启成功后 30 秒冷却期，避免频繁重启
## 3. 文件结构

```python
tools\gateway-watchdog\
├── watchdog.ps1      # 主脚本
├── deploy.ps1        # 部署脚本
├── watchdog.log      # 运行日志
└── README.md         # 使用文档
```

## 4. 部署

### 前提条件

- Windows 10/11
- PowerShell 5.1+
- 管理员权限（创建 Scheduled Task 需要）
### 步骤一：确认配置

打开 watchdog.ps1，检查顶部配置区

### 步骤二：创建定时任务

以管理员身份运行 PowerShell，执行以下命令：

```
schtasks /Create /TN "OpenClawGatewayWatchdog" /TR "powershell.exe -NoProfile -ExecutionPolicy Bypass -WindowStyle Hidden -File `"C:\Users\zach\.openclaw\workspace\tools\gateway-watchdog\watchdog.ps1`"" /SC MINUTE /MO 5 /RL HIGHEST /F
```

### 步骤三：验证部署

```
Get-ScheduledTask -TaskName "OpenClawGatewayWatchdog"
```

看到 State: Ready 表示部署成功。

## 5. 使用

```
# 启动`nStart-ScheduledTask -TaskName "OpenClawGatewayWatchdog"`n`n# 停止`nschtasks /Change /TN "OpenClawGatewayWatchdog" /DISABLE`n`n# 删除`nschtasks /Delete /TN "OpenClawGatewayWatchdog" /F
```

## 6. 工作原理

1. 端口探测：连接 127.0.0.1:18789，超时 3 秒
1. 进程检查：WMI 查询 node.exe 命令行含 openclaw/gateway
1. 双重验证：端口可达且进程存在 = 正常
1. 异常时 kill 残留 + 启动 gateway.cmd
1. 恢复后发飞书通知 + 30 秒冷却
## 7. 飞书通知

- 触发条件：网关异常被自动恢复后
- 标题：✅ OpenClaw 网关已自动恢复
- 修改接收人：
```
$FeishuNotifyEnabled = $false  # 禁用通知
```

## 8. 故障排查

1. 检查任务状态：Get-ScheduledTask -TaskName 'OpenClawGatewayWatchdog'
1. 查看日志：Get-Content 'watchdog.log' -Tail 50
1. 检查路径：Test-Path 'C:\Users\zach\.openclaw\gateway.cmd'
1. 飞书通知：检查日志中飞书相关错误
## 9. 高级配置

```
$RetryIntervalSec = 10     # 检查间隔（秒）`n$ConsecutiveFailMax = 3    # 连续失败阈值`n$GatewayPort = 18790        # 网关端口
```

## 10. 卸载

```
schtasks /Delete /TN "OpenClawGatewayWatchdog" /F`nRemove-Item "tools\gateway-watchdog" -Recurse -Force
```

## 11. 已知限制

- 仅支持本地网关（127.0.0.1）
- Start-Process cmd.exe 可能短暂可见窗口
- 重启后网关内存状态无法恢复
文档版本：v1.0 | 创建日期：2026-05-02

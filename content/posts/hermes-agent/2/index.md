+++
date = '2026-05-14T10:45:04+08:00'
draft = false
author = "码一行"
title = 'Hermes浏览器自动化配置教程'
+++

## 背景
Windows 用户在使用`Hermes Agent`时，由于 Hermes 运行在 WSL 上，拥有独立网络环境，无法直接控制本地浏览器。

## 第一步：启用Chrome远程调试端口
确保 Chrome 版本 >= 144
右键 Chrome 图标 → 复制 → 桌面空白处粘贴
右键新建图标 → 属性 → 修改目标为：
```bash
"C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9222 --user-data-dir=C:\chrome-debug-profile
```
点击应用 → 双击打开 Chrome → 地址栏输入 `chrome://inspect/#remote-debugging`
然后打开远程调试

## 第二步：配置 WSL 网络模式（关键）
进入C盘用户目录，新建文件 `.wslconfig`，内容：
```bash
[wsl2]
networkingMode=mirrored
localhostForwarding=true
```
保存后
PowerShell 运行：`wsl --shutdown` 重启 WSL

## 第三步：配置 Hermes ChromeDevMCP
打开hermes,给让发送：
```yaml
    # 在 ~/.hermes/config.yaml 里加 MCP服务器配置：
    mcp_servers:
      chrome-devtools:
        command: npx
        args:
        - -y
        - chrome-devtools-mcp@latest
        - --browser-url
        - http://127.0.0.1:9222
```
配置远程端口 `9222`，重启 Hermes 生效。
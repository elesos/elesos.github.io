---
layout: post
title: windows软件推荐系列11:wsl2
date: 2025-03-11 05:05:00 +0800
categories: [windows软件推荐]
tags: [windows软件推荐]
---


---

### **一、安装WSL 2**
#### **1. 系统要求**
- **Windows版本**：Win 10 1903+（内部版本18362+）或 Win 11。
- **虚拟化支持**：需在BIOS中启用虚拟化（任务管理器 → “性能”页 → 确认“虚拟化：已启用”）。

#### **2. 安装步骤**
1. **启用必要功能**（管理员PowerShell执行）：
   ```powershell
   dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
   dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
   ```
   重启电脑。

2. **安装Linux内核更新包**：
   - 下载[x64内核更新包](https://aka.ms/wsl2kernel)并安装。

3. **设置WSL 2为默认版本**：
   ```powershell
   wsl --set-default-version 2
   ```

4. **安装Linux发行版**：

# 安装默认 Ubuntu 版本
wsl --install -d Ubuntu

# 安装特定版本
wsl --install -d Ubuntu-22.04


---

### **二、常用命令**
| **功能**                | **命令**                                  |
|-------------------------|------------------------------------------|
| 列出已安装发行版        | `wsl -l -v`                              |
| 运行指定发行版          | `wsl -d Ubuntu-22.04`                    |
| 设置默认发行版          | `wsl -s Ubuntu-22.04`                    |
| 终止发行版              | `wsl -t Ubuntu-22.04`                    |
| 关闭所有WSL实例         | `wsl --shutdown`                         |
| 导出/导入发行版         | `wsl --export Ubuntu-22.04 backup.tar`<br>`wsl --import Ubuntu-22.04 C:\path backup.tar` |
| 卸载发行版              | `wsl --unregister Ubuntu-22.04`          |
| 更新WSL内核             | `wsl --update --web-download`            |
| 在WSL中执行Windows命令  | `notepad.exe`（直接输入exe文件名） |

---

### **三、高效操作技巧**
1. **文件系统互访**：
   - **Windows访问WSL**：文件资源管理器输入 `\\wsl$\Ubuntu-22.04` 直接访问Linux文件。
   - **WSL访问Windows**：所有Windows盘符挂载在 `/mnt/` 下（如 `/mnt/c/Users`）。

2. **配置国内镜像加速**：
   ```bash
   sudo sed -i "s@http://.*archive.ubuntu.com@http://mirrors.aliyun.com@g" /etc/apt/sources.list
   sudo apt update && sudo apt upgrade -y
   ```
   适用于apt和conda（修改 `~/.condarc` 为清华源）。

3. **SSH远程连接配置**：
   ```bash
   sudo apt install openssh-server
   sudo vi /etc/ssh/sshd_config  # 修改：`PasswordAuthentication yes`
   sudo service ssh start
   ```
   通过 `ifconfig` 获取IP后，用Windows SSH客户端连接。

4. **开机自启动服务**（如Nginx/MySQL）：
   - 在WSL创建启动脚本 `/etc/init.wsl`，添加 `service nginx start` 等命令。
   - Windows任务计划程序设置登录时运行：`wsl -d Ubuntu-22.04 -u root /etc/init.wsl`。






# 安装Linux子系统

---

## Linux系统

- 要使用Linux系统一共有4种方式：
  
  1. 使用虚拟机：安装便捷，体验感较差（需要手动分配内存和储存空间，网速较慢）
  
  2. 安装Linux双系统：安装复杂，与Window系统之间来回切换比较麻烦，一次只能使用一个系统
  
  3. 安装Linux子系统（WSL, Window Subsystem or Linux）：安装便捷，使用感好
  
  4. 使用服务器：不占用电脑内存，无需安装，但需要购买服务器

## 适用于Windows的Linux子系统（WSL）

More in [WSL官方教程](https://learn.microsoft.com/zh-cn/windows/wsl/)

- WSL在Window系统中提供一个Linux兼容接口，能够运行GNU/Linux环境（包括大部分命令行工具、实用工具和应用程序）且不会产生传统虚拟机或双系统的设置开销。

- WSL提供一个完全支持**Linux文件系统**的环境，能够和Windows上的设备和文件**互通**，可以使用**同一个剪切板**。

## 安装Ubuntu

1. 先决条件
   
   需要安装Window 10 2004版本以上或者Window 11才能使用

2. 系统设置
   
   启用`适用于Linux的Windows子系统`功能
   
   - 打开【设置--隐私与安全性--开发者选项】，选择开启`开发者模式`
   
   - 打开【控制面板-程序-程序与功能--启用或关闭Windows功能】，勾选`适用于Linux的Windows子系统`，`虚拟机平台`，`Windows虚拟机程序监控平台`功能
   
   - 重启电脑

3. 打开Microsoft Store，搜索并下载Ubuntu
   
   下载后安装报错：0x800701bc
   
   解决方案：下载安装适用于x64计算机的最新WSL 2 Linux内核更新包（[下载Linux内核更新包](https://learn.microsoft.com/zh-cn/windows/wsl/install-manual "Step4 - 下载Linux 内核更新包“)，将WSL 2设置为默认版本：
   
   ```powershell
   wsl --set-default-version 2
   ```
   
   重新打开Ubuntu进行安装，安装后设计用户名和密码（在Linux中输入密码不会显示字符）
   
   
   
   
   
   
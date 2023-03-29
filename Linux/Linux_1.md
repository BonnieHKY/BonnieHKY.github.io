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

## 安装Docker

- Docker是一种工具，通过容器（contaniner）用于创建、部署和运行应用程序。容器可以把应用以及与之相关的部件（库、框架、依赖项等）打包为一个包一起交付。Docker容器与虚拟机类似，但不会创建整个虚拟操作系统，而允许应用使用与运行它的系统相同的Linux内核。

- Docker Desktop for Windows 为生成、交付、运行Docker化的应用提供了一个开发环境。通过启用基于WSL2的引擎，可以在同一计算机的Docker Desktop上运行Linux和Windows的容器。

- 虽然Docker Desktop支持Linux和Windows的两个容器，但是两个容器不能同时运行。

### 安装流程

1. 从[Install Docker Desktop on Windows](https://docs.docker.com/desktop/install/windows-install/)下载Docker Desktop Installer.exe

2. 双击运行安装，安装后需要重启

3. 在Windows Shell检查是否安装成功
   
   ```
   PS C:\Users\86180> docker --version
   Docker version 20.10.23, build 7155243 
   
   PS C:\Users\86180> docker run hello-world
   Unable to find image 'hello-world:latest' locally
   latest: Pulling from library/hello-world
   2db29710123e: Pull complete
   Digest: sha256:6e8b6f026e0b9c419ea0fd02d3905dd0952ad1feea67543f525c73a0a790fefb
   Status: Downloaded newer image for hello-world:latest
   
   Hello from Docker!
   This message shows that your installation appears to be working correctly.
   ```

4. 打开Docker-Desktop，注册一个Docker-Hub账号，在Setting-Docker Engine设置镜像：
   
   ```
   {
     "builder": {
       "gc": {
         "defaultKeepStorage": "20GB",
         "enabled": true
       }
     },
     "experimental": false,
     "features": {
       "buildkit": true
     }, #注意添加逗号
     "registry-mirrors": [
       "https://registry.docker-cn.com",
       "https://docker.mirrors.ustc.edu.cn",
       "http://hub-mirror.c.163.com"
     ]
   }
   ```

5. 重新设置镜像和容器存放位置（不想放在C盘）
   
   首先需要关闭docker-desktop
   
   ```
   PS C:\Users\86180> wsl --list -v
     NAME                   STATE           VERSION
   * Ubuntu-22.04           Running         2
     docker-desktop-data    Stopped         2
     docker-desktop         Stopped         2
   PS C:\Users\86180> wsl --export docker-desktop-data D:\Docker-wsl\data\docker-desktop-data.tar
   PS C:\Users\86180> wsl --export docker-desktop D:\Docker-wsl\distro\docker-desktop.tar
   PS C:\Users\86180> wsl --unregister docker-desktop-data
   正在注销...
   PS C:\Users\86180> wsl --unregister docker-desktop
   正在注销...
   PS C:\Users\86180> wsl --import docker-desktop-data D:\Docker-wsl\data D:\Docker-wsl\data\docker-desktop-data.tar
   PS C:\Users\86180> wsl --import docker-desktop D:\Docker-wsl\distro\ D:\Docker-wsl\distro\docker-desktop.tar
   ```

### 使用Docker Image

1. `load` ：加载Docker Image
   
   ```
   # 在Windows PowerShell上运行
   # Usage:  docker load [OPTIONS]
   # Load an image from a tar archive or STDIN
   # Options:
   #  -i, --input string   Read from tar archive file, instead of STDIN
   #  -q, --quiet          Suppress the load output
   
   PS C:\Users\86180> docker load -i D:\Bioinfo-Docker\bioinfo_PartI-PartII-PartIII1-3.tar.gz 
   # 随后会打印一大段加载信息
   ```

2. `run`：从一个Image中新建一个Container
   
   - 首先需要在宿主机上新建一个文件夹用于和container进行文件共享，此处为`D:\test\Share`
   
   - 设置一个在container中用于与宿主机进行共享的文件夹`home/test/share`，此时这两个文件可以在不同系统下访问和交流
   
   ```
   docker run --name=bioinfo -dt -h bioinfo_docker --restart unless-stopped -v D:\test\Share:/home/test/share xfliu1995/bioinfo_tsinghua:2
   ```
   
   - 需要注意Windows下的目录使用`\`而，Linux下的目录使用`/`

3. `exec`：建立了container后，每次只需要启动Docker程序，然后在Power Shell中输入以下命令即可进入container
   
   ```
   docker exec -it bioinfo bash
   ```

4. `rm`：删除container。
   
   - 当误删了某些程序导致container不能正常工作程序时，可以删除container再重新建立一个新的container
   
   ```
   docker rm -f bioinfo
   ```

5. `rmi`：删除image
   
   - 如果某些image加载后不再需要使用，可以使用该命令进行删除
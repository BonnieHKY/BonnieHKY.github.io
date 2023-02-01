# Linux的文件管理系统

---

## 目录与路径（PATH）

- 绝对路径：一定从根目录`/`写起的，如`/usr/share/doc`

- 相对路径：相对于目前工作路径，如在`/usr/share/doc`下寻找`/usr/share/man`，则可以输入`../man`。

> 特殊的目录：
> 
> - `.` 表示此层目录
> 
> - `..`表示上一层目录
> 
> - `-`表示前一个工作目录
> 
> - `~`表示目前使用者的所在的主文件夹
> 
> - `~username`表示username这个用户的主文件夹

## Linux的目录配置

参考：[《Linux系统》Linux文件系统的介绍 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/128669031)

- 根目录`/`：根目录是整个系统中最重要的目录，所有的目录均有根目录衍生出来。通常根目录下只存放目录，不存放文件。
  
  ```
  bonniehky@Bonnie_U:~$ ls /
  bin   dev  home  lib    lib64  
  lost+found  mnt  proc  run   snap  sys  usr
  boot  etc  init  lib32  libx32  media   
  opt  root  sbin  srv   tmp  var
  ```

- `/bin`、`/usr/bin`：命令文件目录，也称为二进制目录。包含了供系统管理员及普通用户使用的重要的Linux命令和二进制（可执行）文件。

- `/boot`：主要放置开机使用的文件，包括Linux核心文件以及开机菜单和开机所需配置文件等。

- `/dev`：设备（device）文件目录。在Linux中所有的设备均以文件的形式存在。访问该目录下的某个文件相当于访问某个设备。如硬盘和U盘：`/dev/sd[a-p]`

- `/home`：系统默认的用户宿主目录，新增用户账号时，用户的宿主目录都存放在此目录下。建议单独分区，并设置较大的磁盘空间，方便用户存放数据。

- `/lib`、`/usr/lib`：函数库（library）目录，即系统动态连接资源库，内核模块文件目录。几乎所有的应用程序都用到此资源库。

- `/lost+found`：该目录存放系统意外崩溃或意外挂机时，产生的文件碎片。当系统重新启动时，fsck工具会检查这里，并修复已经损坏的文件系统。

- `/media`：可移除设备目录，如软盘、光盘等设备都暂时挂载在这里。

- `/mnt`：临时挂载（mount）文件目录，与`/media`相似。在WSL，可以通过`/mnt`访问Windows系统中的文件。

- `/opt`：放置第三方协助软件的目录。例如：想要自行安装额外的软件，可以安装在此处。

- `/proc`：此目录放置的数据均在内存中，例如：系统核心、外部设备、网络状态。此目录本身是一个“虚拟文件系统（virtual filesystem）”，不占用任何磁盘空间。

- `/root`：系统管理员（root）的主文件夹。系统第一个启动的分区为`/`，当进入单人维护模式而仅挂载根目录时，该目录能够拥有roo的主文件夹，因此`/`和`/root`主文件夹放置在同一个分区中。

- `/run`：存放开机后产生的各项信息的目录。

- `/sbin`、`/user/sbin`：放置系统管理员使用的可执行命令，如fdish、shutdown、mount等，一般用户只能“查看”而不能设置和使用。

- `/srv`：一些网络服务（service）启动后，所需获取的数据目录。常见的服务如WWW，FTP等。

- `/usr`：Unix操作系统软件资源（Unix software resource）目录，系统默认软件都会放置在`/usr`下。`/usr/bin`存放所有一般用户都能够使用的指令。`/usr/local`系统管理员在本机自行安装的软件可以安装到此目录下，也可以存放软件升级包。`/usr/share`放置只读架构的数据文件，包括程序说明文件man page的存放目录`/usr/share/man`、系统说明文件的存放目录`/usr/share/doc`。

- `/var`：常态性变动文件目录。`/var/cache`存放高速暂存文件的目录。`/var/log`登录文件放置目录，里面保存了重要的登陆者信息文件目录，如`/var/log/messages`、`/var/log/wtmp`。`/var/mail/`放置个人电子邮件信息目录。

- `/sys`：这个目录其实跟`/proc`类似，也是一个虚拟的档案系统，主要记录核心与系统硬件信息相关的信息， 包括目前已载入的核心模组与核心侦测到的硬体装置资讯等等。 这个目录同样不占硬盘容量。

## 文件属性与权限

- 文件属性示意图，使用`ls -l`可以查看文件夹下各文件和目录的属性（详细指令介绍可以查看”文件夹具体操作“）：
  
  ![Linux4.1](..\figure\Linux4.1.png)
  
  > 第一栏表示文件/文件夹的类型与权限（permission），共10个字符。
  > 
  > 第二栏表示有多少文件名链接到此节点（i-node）
  > 
  > 第三栏表示这个文件（或目录）的拥有者
  > 
  > 第四栏表示这个文件（或目录）所属的群组
  > 
  > 第五栏表示这个文件（或目录）的容量大小，默认单位为Bytes
  > 
  > 第六栏表示这个文件的创建时间或最近修改时间
  > 
  > 第七栏表示这个文件（或目录）的名称

## 文件的类型与权限码

          ![Linux4.2](..\figure\Linux4.2.png)

- 第一个字符表示文件的类型`d`表示目录；`-`表示文件；`l`表示链接文件（link file）；`b`表示设备文件中可供储存的周边设备（可随机存取设备）；`c`表示设备文件中的序列埠设备，如键盘、鼠标等（一次性读取设备）。

- 下面的九个字符以`rwx`三个参数为一组，依次代表文件拥有者、加入此群组用户、未加入此群组的其它用户所拥有的权限。

- `r`代表可读（read），`w`代表可写（write），`x`代表可执行（execute），三个参数的位置是固定的，若没有权限则显示`-`。

## 修改文件的属性与权限

1. `chgrp` (change group)：改变文件或目录所属群组
   
   - 语法：`chgrp [-options] group file`
   
   - 选项：`-R`一同更改目录下的文件和次目录

2. `chown` (change owner)：改变文件拥有者
   
   - 语法：
     
     `chown [-options] owner file`：`chown`可以仅用于更改文件的拥有者
     
     `chown [-options] owner:group file`：也可以用于同时更改文件的拥有者和所属群组

3. `chmod` (change mode)：改变文件的权限
   
   **方式1**：使用数字类型修改文件权限，用法为`chmod [-R] xyz [file]`
   
   - 数字类型权限`xyz`使用x、y、z的数值分别代表owner、group、others三种身份的权限。计算方式为：`r`权限+4，`w`权限+2，`x`权限+1，`-`+0，将三种身份的权限进行加和计算则分别得到数字类型权限`xyz`。
     
     ![Linux4.3](..\figure\Linux4.3.png)
   
   - `-R`、`--recursive`：将次目录下的文件与目录全部进行修改
   
   **方式2**：使用符号类型修改文件权限，用法为`chmod [[ugoa][-+=][rwx]] [file]`。其中，u：user，g：group，o：others，a：all。
   
   - 把`.bashrc`文件的权限设置为`rwxr-xr-x`：
   
   ```
   chmod u=rwx,go=rx .bashrc
   ```
   
   - 把所有人（all）的execute权限移除：
   
   ```
   chmod a-x .bashrc
   ```
   
   - 为所有人增加execute权限：
   
   ```
   chmod a+x .bashrc
   ```

## 文件的时间参数

- modification time (mtime)
  
  当文件的数据内容（不含属性和权限）变更时，会更新此时间。默认情况下，`ls`显示的是该最近修改时间。

- status time (ctime)
  
  当文件的状态（属性和权限）改变时，会更新此时间。

- access time (atime)
  
  当文件的内容被取用，会更新此时间。例如使用`cat`读取文件内容就会更新此时间。

- `touch`：修改文件的时间
  
  - 语法：`touch [-options] file`
  
  - 选项（部分）：
    
    `-a`：仅修改atime（默认情况下mtime、ctime、atime都会更新为当前时间）
    
    `-c`、`--no-creat`：仅修改文件的时间，若该文件不存在则不自动创建新文件（默认情况下若文件不存在则创建一个新的空文件）
    
    `-d`：后面可以接指定的日期而不用使用目前的日期，也可以使用`--date=STRING`定义时间
    
    `-m`：仅修改mtime
    
    `-t`：后面可以接指定的日期而不用使用目前的日期，格式为`YYYYMMDD`
  
  - 示例：
    
    `touch filename`：默认把三个时间更新为当前时间
    
    ```
    bonniehky@Bonnie_U:/tmp$ ll wtmp;ll --time=atime wtmp;ll --time=ctime wtmp
    -rw-r--r-- 1 bonniehky bonniehky 0 Jan 29 17:57 wtmp
    -rw-r--r-- 1 bonniehky bonniehky 0 Jan 29 17:57 wtmp
    -rw-r--r-- 1 bonniehky bonniehky 0 Jan 29 17:57 wtmp
    bonniehky@Bonnie_U:/tmp$ touch wtmp
    bonniehky@Bonnie_U:/tmp$ ll wtmp;ll --time=atime wtmp;ll --time=ctime wtmp
    -rw-r--r-- 1 bonniehky bonniehky 0 Jan 30 11:29 wtmp
    -rw-r--r-- 1 bonniehky bonniehky 0 Jan 30 11:29 wtmp
    -rw-r--r-- 1 bonniehky bonniehky 0 Jan 30 11:29 wtmp
    ```
    
    `touch -d "yesterday" filename`：把mtime和atime改成昨天当前同一时间，而ctime更新为当前时间
    
    ```
    bonniehky@Bonnie_U:/tmp$ touch -d "yesterday" wtmp
    bonniehky@Bonnie_U:/tmp$  ll wtmp;ll --time=atime wtmp;ll --time=ctime wtmp
    -rw-r--r-- 1 bonniehky bonniehky 0 Jan 29 11:31 wtmp
    -rw-r--r-- 1 bonniehky bonniehky 0 Jan 29 11:31 wtmp
    -rw-r--r-- 1 bonniehky bonniehky 0 Jan 30 11:31 wtmp
    ```
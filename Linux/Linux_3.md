# Linux指令操作

---

## 指令的结构

![Linux3.1](..\figure\Linux3.1.png)

- 一行指令中第一个输入的部分是指令（command）或者可执行文件案（如脚本script）

- 选项（options）在输入时不需要输入`[]`
  
  - options为单字母一般会带`-`，例如`-h`，有时候使用选项的全称时会带`--`，例如`--help`
  
  - 如果输入多个选项可以连写，如：`ls -a -l`也可以写成`ls -al`

- 参数为依附于选项后的参数，或者是指令的参数

- 指令、选项、参数之间用空格以区分，无论空几格shell均会视为一格

- 按下`Enter`后，该指令就会立刻执行。如果指令太长，在一行末端加反斜线`\`来跳脱`Enter`，使指令连续到下一行

- 指令需要区分大小写。

## 获取指令的使用说明

- 使用`--help`这个选项，可以查看指令的基本用法和选项参数的介绍：`date --help`

- 使用`man`指令选择想要了解的指令介绍（打开一个manual page）：`man date`

- 使用`info`指令，不过需要查询的指令说明具有info page格式

## 基础指令操作

1. 显示日期与时间：`date`

2. 显示日历的指令：`ncal`（new calendar）或者`cal`（calendar）
   
   - 刚开始输入`cal`和`ncal`的指令时，显示没有安装，则安装`ncal`程序：
     
     先用`sudo apt update`更新apt数据库
     
     在使用`sudo apt -y install ncal`安装`ncal`程序
   
   - 尝试操作如下，`cal`和`ncal`的详细说明：[Linux Cal and Ncal Command Help and Examples](https://www.computerhope.com/unix/ucal.htm)
   
   ```
   bonniehky@Bonnie_U:~$ ncal 
       January 2023
   Su  1  8 15 22 29
   Mo  2  9 16 23 30
   Tu  3 10 17 24 31
   We  4 11 18 25
   Th  5 12 19 26
   Fr  6 13 20 27
   Sa  7 14 21 28
   bonniehky@Bonnie_U:~$ cal
       January 2023
   Su Mo Tu We Th Fr Sa
    1  2  3  4  5  6  7
    8  9 10 11 12 13 14
   15 16 17 18 19 20 21
   22 23 24 25 26 27 28
   29 30 31
   ```

3. 计算器：`bc`
   
   输入`bc`,屏幕上会显示出版本信息
   
   ```
   bonniehky@Bonnie_U:~$ bc
   bc 1.07.1
   Copyright 1991-1994, 1997, 1998, 2000, 2004, 2006, 2008, 2012-2017 Free Software Foundation, Inc.
   This is free software with ABSOLUTELY NO WARRANTY.
   For details type `warranty`
   
   这个时候光标会停在这里等待输入
   ```
   
   - `+`：加法
   
   - `-`：减法
   
   - `*`：乘法
   
   - `/`：除法（设置显示到小数点后n位：`scale=n`）
   
   - `^`：指数
   
   - `%`：余数
   
   - `quit`：关闭计算器

4. 账号管理
   
   1. 切换账号：`su [user name]`，`exit`退出当前用户返回root用户
   
   2. 使用管理员权限：`sudo command parameter`
   
   3. 创建新用户：`useradd [user name]` ；删除用户：`userdel [user name]`；需要管理员权限
   
   4. 修改密码：`Passwd [user name]`

# Bash Shell

## 简介

- 用户需要通过Shell与Kernel（内核）进行沟通，而Kernel真正控制硬件工作

- bash是Linux默认的shell。'bash'的全称是'Bourne Again SHell'，这个shell是Bourne Shell的增强版本，也是基于GNU的架构下发展出来的

## Shell的变量

## 命令别名与历史命令

1. 命令别名的设置：`alias`，`unalias`
   
   - 直接输入`alias`可以查询现有的命令别名
     
     ```
     bonniehky@Bonnie_U:~$ alias
     alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo terminal || echo error)" "$(history|tail -n1|sed -e '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'
     alias egrep='egrep --color=auto'
     alias fgrep='fgrep --color=auto'
     alias grep='grep --color=auto'
     alias l='ls -CF'
     alias la='ls -A'
     alias ll='ls -alF'
     alias ls='ls --color=auto'
     ```
   
   - 设置别名：`alias name='value'`
     
     删除别名：`unalias name`
     
     ```
     bonniehky@Bonnie_U:~$ alias rm='rm -i'
     bonniehky@Bonnie_U:~$ unalias rm
     ```

2. 历史命令：`history`
   
   - `history [n]`：打印目前最近的n条命令；默认打印所有命令数据（错误输入命令也会记录）
   
   - `history [-c]`：将目前的shell中的所有history清除
     
     ```
     bonniehky@Bonnie_U:~$ history 3
       430  history --help
       431  history ~=3
       432  history 3
     ```
   
   - 在输入命令时，使用方向键“上”和“下”可以搜索历史命令

## 数据重定向

![Linux7.1.png](..\figure\Linux7.1.png)

- 当我们执行一个命令时，这个命令可能从文件读入数据，经过数据处理后，再把数据输出到屏幕上。数据重定向就是把本应打印在屏幕上的数据存储到其它位置
  
  - 标准输入（stdin）：代码为0，使用`<`或`<<`
  
  - 标准输出（stdout）：代码为1，使用`>`或`>>`
  
  - 标准错误输出（stderr）：代码为2，使用`2>`或`2>>`
  
  ```
  bonniehky@Bonnie_U:/tmp$ ll
  total 16
  drwxrwxrwt  4 root      root      4096 Mar  2 20:54 ./
  drwxr-xr-x 20 root      root      4096 Mar  2 20:00 ../
  drwxr-xr-x  2 bonniehky bonniehky 4096 Jan 29 12:50 711/
  drwxr-xr-x  2 bonniehky bonniehky 4096 Jan 29 12:46 755/
  bonniehky@Bonnie_U:/tmp$ ll > ./Tredir 
  # 目录下没有该文件Tredir，则新建一个文件并且把stdout写进去
  # 当目录下没有目标文件，使用>和>>效果相同
  bonniehky@Bonnie_U:/tmp$ cat Tredir
  total 16
  drwxrwxrwt  4 root      root      4096 Mar  2 20:55 ./
  drwxr-xr-x 20 root      root      4096 Mar  2 20:00 ../
  drwxr-xr-x  2 bonniehky bonniehky 4096 Jan 29 12:50 711/
  drwxr-xr-x  2 bonniehky bonniehky 4096 Jan 29 12:46 755/
  -rw-r--r--  1 bonniehky bonniehky    0 Mar  2 20:55 Tredir
  ```

- 输出到文件的方法
  
  - `>`：以覆盖的方式把输出内容写进文件，（如果只有错误信息则文件会被清空）
  
  - `>>`：以追加的方式把输出内容写到文件的后面
  
  ```
  bonniehky@Bonnie_U:/tmp$ ls -l > Tredir
  bonniehky@Bonnie_U:/tmp$ history 3 >> Tredir
  bonniehky@Bonnie_U:/tmp$ cat Tredir
  total 12
  drwxr-xr-x 2 bonniehky bonniehky 4096 Jan 29 12:50 711
  drwxr-xr-x 2 bonniehky bonniehky 4096 Jan 29 12:46 755
  -rw-r--r-- 1 bonniehky bonniehky    0 Mar  2 21:03 Tredir
  -rw-r--r-- 1 bonniehky bonniehky  353 Mar  2 20:56 Tredir2
    478  cat Tredir
    479  ls -l
    480  history 3 >> Tredir 
  
  
  bonniehky@Bonnie_U:/tmp$ histry >> Tredir 2>> Tredir2
  # 把正确的信息写进Tredir，把错误的信息写进Tredir2
  bonniehky@Bonnie_U:/tmp$ ls Tredir >& Tredir2 
  # 把正确和错误的信息都以覆盖的方式写进Tredir2
  ```

- 标准输入
  
  - `<<`：代表输入结束
  
  ```
  bonniehky@Bonnie_U:/tmp$ cat > Tredir2 << "123"
  > This is a test
  > abc
  > stop
  > 123 
  # 当输入关键字123时会停止输入，可以把关键字以上的内容写入Tredir2
  bonniehky@Bonnie_U:/tmp$ cat Tredir2
  This is a test
  abc
  stop
  ```
  
  

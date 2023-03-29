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

## 命令执行的判断

- 不考虑命令相关性的执行：`cmd1 ; cmd2`，用分号分开两条命令，则先执行cmd1，再执行cmd2

- 具有相关性的命令执行：根据前一个命令的执行情况，决定后一个命令是否执行
  
  - `cmd1 && cmd2`：当cmd1执行完毕并且结果正确后执行cmd2
  
  - `cmd1 || cmd2`：当cmd1执行完毕并且结果错误后执行cmd2
  
  > 在Linux中，当一个命令执行正确会回传一个命令回传码$?，当回传码等于0则表示命令执行结果正确，若不等于0则表示命令执行结果错误。

## 管道命令

- `cmd1 | cmd2`，把cmd1的输出（stdout）作为cmd2的输入（stdin）；其中cmd2仅能接受标准输入，而标准错误输入不会被处理

- cmd2中的命令必须要是可以接受stdin的的命令，这样的命令称为**管道命令**

### 1. 选取命令

- `cut`：以行为单位，把某一段分割出来
  
  `-d`：自定义分割字符，后用' '设置分割字符。默认使用`Tab`为分割字符
  
  `-f number`：根据分割字符把一行信息分为多段，设置选出第几段
  
  `-c`：以字符为单位，去除固定的字符区间
  
  ```
  以1.gtf为例
  bonniehky@Bonnie_U:/mnt/d/test/2.linux$ cat 1.gtf | head | tail -5 | cut -f 4,5
  1802    2953
  1802    2953
  1802    2953
  1802    2950
  1802    1804
  bonniehky@Bonnie_U:/mnt/d/test/2.linux$ cat 1.gtf | head | tail -5 | cut -f  9 | cut -d ';' -f 1-2
  gene_id "YDL248W"; gene_version "1"
  gene_id "YDL248W"; gene_version "1"
  gene_id "YDL248W"; gene_version "1"
  gene_id "YDL248W"; gene_version "1"
  gene_id "YDL248W"; gene_version "1" 
  bonniehky@Bonnie_U:/mnt/d/test/2.linux$ cat 1.gtf | head | tail -5 | cut -c 10-15
  l       gene
  l       tran
  l       exon
  l       CDS
  l       star
  ```

- `grep`：分析一整行的信息，若其中含有所需的信息`char`，则将该行取出来
  
  用法：`grep [-options] 'char' {filename or stdin}`
  
  `-c`：计算找到`char`的次数
  
  `-i`：忽略大小写的不同
  
  `v`：反向选择，即输出没有`char`的行
  
  - `grep`常常联合正则表达式使用

### 2. 排序命令

- `sort`：对数据进行排序
  
  用法：`sort [-options] {filename or stdin}`
  
  `-f`：忽略大小写的差异
  
  `-n`：使用纯数字进行排序（默认以文字形式进行排序）
  
  `-r`：反向排序
  
  `-k`：以哪一个区间进行排序，默认以`Tab`进行分割区间；`-t`可以用于设置分割符号

- `wc`：查看文件内有多少行、多少词、多少字符
  
  ```
  bonniehky@Bonnie_U:/mnt/d/test/2.linux$ wc 1.gtf
     42252  1202536 12053570 1.gtf # 依次为行数、词数、字符数
  ```

- `uniq`：合并重复数据，仅列出一个显示
  
  `-c`：进行计数
  
  `-i`：忽略大小写的差异
  
  ```
  bonniehky@Bonnie_U:/mnt/d/test/2.linux$ cat 1.gtf | cut -f 3 | sort |uniq -c
        1 #!genebuild-last-updated 2011-12
        1 #!genome-build R64-1-1
        1 #!genome-build-accession :GCA_000146045.2
        1 #!genome-date 2011-09
        1 #!genome-version R64-1-1
     7050 CDS
     7553 exon
     7126 gene
     6700 start_codon
     6692 stop_codon
     7126 transcript
  ```

### 3. 双向重定向

- `tee`：把stdout转存到文件，同时把相同的数据送到屏幕进行处理
  
  用法：`tee [-a] filename`
  
  `-a`：默认以覆盖的方式写入stdout，添加`-a`可以设置以累加的方式写入信息

### 4. 字符转换命令

- `tr`：删除一段信息当中的文字，或进行文字信息的替换
  
  用法：`tr [-options] SET1 [SET2]`
  
  `-d`：删除SET1中的字符（不进行转义）
  
  `-s`：替换重复的字符
  
  ```
  bonniehky@Bonnie_U:/mnt/d/test/2.linux$ cat 1.gtf | tail |cut -f 9| cut -d ';' -f 1 | cut -d ' ' -f 2 | tr -d '"'
  tM(CAU)Q2
  RPM1
  RPM1
  RPM1
  Q0297
  Q0297
  Q0297
  Q0297
  Q0297
  Q0297 
  # 删除引号
  ```

- `col`：在许多UNIX说明文件里，都有RLF控制字符。当我们运用shell特殊字符`>`和`>>`，把说明文件的内容输出成纯文本文件时，控制字符会变成乱码，`col`指令则能有效滤除这些控制字符。

- `join`：找出两个文件中，指定栏位内容相同的行，并加以合并

- `paste`：合并多个文件，或把一个文件内的多行内容合并为一行
  
  用法1：`paste file1 file2 file3`
  
  用法2：`paste -s filename` 

- `expand`：把`Tab`转换成空格键（一般一个`Tab`转换为8个空格）

### 5. 划分命令

- `split`：把一个大文件划分为多个小文件

## 正则表达式和通配符

### 通配符

- 主要用于匹配文件名（用于文件名替换）

- 常用命令包括：`ls`，`find`，`cp`，`mv`

- 常用的通配符
  
  `*`：匹配0个或多个字符
  
  `[]`：匹配括号中的单个字符。`[abcd]*`：匹配以abcd任意一个字符开头的文件
  
  `?`：匹配任意单个字符
  
  `[!]`：匹配不在括号中的任意单个字符。`[!ab]*`：匹配不以ab开头的任意文件
  
  `[a-z]`：匹配a-z中的任意单个字符，只能用于查找文件，不能用用于创建文件
  
  `(a,b,z)`：以逗号为分隔，表示单独的字符。匹配以abz任意一个字符开头的文件，可以用于查找和创建文件
  
  `(a..z)`：以..为分割，表示连续的字符。匹配a到z任意一个字符开头的文件，可以用于查找和创建文件。

### 正则表达式

- 主要用于匹配文件中的字符串

- 常用的命令是：`grep`，`awk`，`sed`

- 常用的通配符
  
  `*`：匹配0个或多个字符
  
  `.`：匹配除了换行符以外的任意单个字符
  
  `^`：锚定行首
  
  `$`：锚定行尾
  
  `[abz]`：匹配括号中指定的任意单个字符
  
  `[^abz]`：匹配括号以外的任意单个字符
  
  `\`：转义符
  
  `\{n\}`：表示前面的字符出现几次
  
  `\{n,\}`：至少匹配前面的字符n次
  
  `\{n,m\}`：至少匹配前面的字符n次，但是不得多于m次
  
  `-`：连字符，[a-z]表示a到z

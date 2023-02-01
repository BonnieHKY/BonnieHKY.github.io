# 文件与目录的管理

---

## 查看文件与目录的属性

1. 显示当前目录：`pwd`（print working directory）
   
   ```
   bonniehky@Bonnie_U:~$ pwd
   /home/bonniehky
   ```

2. 显示文件夹中文件（夹）列表：`ls`（list directory contents）
   
   - 语法：`ls [-options] [file]`
   
   - 选项（部分）：
     
     `none`：显示目录下的文件及目录名称，不含隐藏文件、当前目录和父目录。
     
     `-a` (all)：显示所有文件及目录 （`.`开头的隐藏文件也会列出）
     
     `-A` (almost all)：同`-a`，但不列出`.`（目前目录）及 `..` （父目录）
     
     `-c`：按ctime（change time）排序（最新再前）；与`-lt`联用时，显示ctime并按ctime排序（最新在前）；与`-l`联用时，显示ctime，并按文件名排序
     
     `-d`：仅列出目录本身，而不列出目录下的文件数据
     
     `-h`：与`-l`和`-s`联用，按`1K 234M 2G`的形式打印文件大小
     
     `-l`：除文件名称外，将文件型态、权限、拥有者、文件大小等资讯详细列出（`ls -l`是最常用的选项，因此在很多默认情况下直接输入`ll`等同于输入`ls -l`）
     
     `-r`：将文件以相反次序显示
     
     `-t`：按时间次序输出，时间设置见`--time`
     
     `--time`：改变时间设置，默认为mtime（modification time）。`--time=atime`按atime（access time）排序； `--time=ctime`按ctime排序；`--time=birth, creation`按创建时间排序
     
     `-R`：若目录下有文件，则以下之文件亦皆依序列出
   
   - 示例：
     
     ```
     bonniehky@Bonnie_U:~$ ls -Fal
     total 28
     drwxr-x--- 2 bonniehky bonniehky 4096 Jan 28 17:41 ./  #当前目录
     drwxr-xr-x 3 root      root      4096 Jan 24 08:45 ../  #父目录
     -rw------- 1 bonniehky bonniehky  874 Jan 28 11:23 .bash_history
     -rw-r--r-- 1 bonniehky bonniehky  220 Jan 24 08:45 .bash_logout
     -rw-r--r-- 1 bonniehky bonniehky 3771 Jan 24 08:45 .bashrc
     -rw------- 1 bonniehky bonniehky   20 Jan 28 17:41 .lesshst
     -rw-r--r-- 1 bonniehky bonniehky    0 Jan 28 10:08 .motd_shown
     -rw-r--r-- 1 bonniehky bonniehky  807 Jan 24 08:45 .profile
     -rw-r--r-- 1 bonniehky bonniehky    0 Jan 27 16:49 .sudo_as_admin_successful
     ```

## 文件及目录的管理

1. 更换所在目录：`cd`（change directory）
   
   - 语法：`cd [PATH]`

2. 创建新目录：`mkdir`（make directories）
   
   1. 语法：`mkdir [-options] [filename]`
   
   2. 选项（部分）：
      
      `-m`，`--mode`：手动设置新建目录的权限（设置方式同`chmod`）。
      
      `-p`，`--parents`：直接创建所需目录（在上层目录未创建时先创建上层目录)
   
   3. 示例：
      
      1. 创建一个新目录（默认权限为rwxr-xr-x）
      
      ```
      bonniehky@Bonnie_U:/tmp$ mkdir test
      bonniehky@Bonnie_U:/tmp$ ls -ld test
      drwxr-xr-x 2 bonniehky bonniehky 4096 Jan 29 12:48 test
      ```
      
      2. 创建权限为rwx--x--x的目录
      
      ```
      bonniehky@Bonnie_U:/tmp$ chmod 711 test2
      bonniehky@Bonnie_U:/tmp$ ls -ld test2
      drwx--x--x 2 bonniehky bonniehky 4096 Jan 29 12:50 test2
      ```
      
      3. 创建数个新目录
      
      ```
      bonniehky@Bonnie_U:/tmp$ mkdir test3/testa/testb
      mkdir: cannot create directory ‘test3/testa/testb’: No such file or directory
      bonniehky@Bonnie_U:/tmp$ mkdir -p test3/testa/testb 
      #由于test3没有创建，所以直接用mkdir创建时会报错，而使用-p则可以之间创建目录，若父目录不存在则先创建父目录。
      ```

3. 删除空目录：`rmdir`（remove empty directories）
   
   - 语法：`rmdir [-options] [directory]`
   
   - 示例
     
     ```
     bonniehky@Bonnie_U:/tmp$ ls
     711  755  test  test1  test2  test3
     bonniehky@Bonnie_U:/tmp$ rmdir test #test为空目录，可以直接删除
     bonniehky@Bonnie_U:/tmp$ ls
     711  755  test1  test2  test3
     bonniehky@Bonnie_U:/tmp$ rmdir test3 #test3下还有子目录，不能直接删除
     rmdir: failed to remove 'test3': Directory not empty
     bonniehky@Bonnie_U:/tmp$ rmdir -p test3/testa/testb
     # 使用-p选项，可以通过递归把test3，testa，testb空目录删除
     bonniehky@Bonnie_U:/tmp$ ls
     711  755  test1  test2
     bonniehky@Bonnie_U:/tmp$ rmdir test*
     # 通过删除含test的目录
     bonniehky@Bonnie_U:/tmp$ ls
     711  755
     ```

4. 复制文件或目录：`cp`（copy）
   
   - 语法：`cp [-options] source1 source2 ... directory`（把source复制到directory下）
   
   - 选项（部分）：
     
     `-a`：相当于`-dr`
     
     `-d`：相当于`--no-dereference`，若来源文件为链接文件，则复制链接文件而非文件本身。如果没有选项时，默认复制原始文件。
     
     `-f`、`--force`：若一个目标文件（destination file）已经存在且无法打开，则移除该文件并再复制一次。
     
     `-i`：若目标文件已经存在，则会询问是否覆盖（若无`-i`则默认覆盖复制）
     
     `-p`：连同文件的属性（权限、用户、修改时间）一同复制，而不使用默认属性（进行备份时常用）
     
     `-r`、`--recursive`：递归持续复制，用于目录的复制
     
     `-s`：复制成链接文件，即“快捷方式”文件
     
     `-u`、`--update`：当源文件比目标文件更新，或者目标文件不存在时才进行复制。
   
   - 示例
     
         使用root身份进行复制：
     
     ```
     root@Bonnie_U:~# cd /tmp
     root@Bonnie_U:/tmp# cp ~/.bashrc . 
     # 把.bashrc.复制到当前文件夹
     
     root@Bonnie_U:/tmp# cp ~/.bashrc ./bashrc1
     # 把.bashrc.复制到当前文件夹，并且命名为bashrc1
     
     root@Bonnie_U:/tmp# cp -i ~/.bashrc ./bashrc1
     cp: overwrite './bashrc1'? n #询问是否覆盖，是输入y，否输入n
     
     root@Bonnie_U:/tmp# cp /var/log/wtmp .
     root@Bonnie_U:/tmp# ls -l wtmp
     -rw-r--r-- 1 root root 0 Jan 29 17:08 wtmp
     root@Bonnie_U:/tmp# ls -l /var/log/wtmp
     -rw-rw-r-- 1 root utmp 0 Oct 26 05:34 /var/log/wtmp
     # 把/var/log/wtmp复制到/tmp，发现文件权限和修改时间与原文件不同
     
     root@Bonnie_U:/tmp# cp -a /var/log/wtmp .
     root@Bonnie_U:/tmp# ls -l wtmp
     -rw-rw-r-- 1 root utmp 0 Oct 26 05:34 wtmp
     #使用-a进行复制可以把文件权限和修改时间一同复制
     
     root@Bonnie_U:/tmp# cp /etc/ .
     cp: -r not specified; omitting directory '/etc/' 
     # 目录不能直接复制，若需复制目录需要使用-r选项。
     ```
     
     使用普通用户复制文件：
     
     ```
     bonniehky@Bonnie_U:/tmp$ cp -a /var/log/wtmp /tmp/wtmptest
     bonniehky@Bonnie_U:/tmp$ ls -l wtmptest
     -rw-rw-r-- 1 bonniehky bonniehky 0 Oct 26 05:34 wtmptest 
     ```
     
     由于普通用户不能随意修改文件的拥有者和群组，因此虽然能够复制wtmp的相关权限和时间信息，但是与拥有者和群组相关信息，普通用户无法进行复制，因此即便使用`-a`也不能进行完整复制。
   
   - 即使我们复制一个文件时复制所有属性，也没有办法复制ctime属性。

5. 删除文件或目录：`rm`（remove）
   
   - 语法：`rm [-options] file`
   
   - 选项（部分）：
     
     `-f`、`--force`：忽略不存在的文件，不会出现警告信息
     
     `-i`：删除前会询问是否删除
     
     `-r`、`--recursive`：删除目录及其下的文件和子目录

6. 移动文件或目录：`mv`（move）
   
   - 语法：`mv [-options]source1 source2 ... directory`
   
   - 选项（部分）：
     
     `-f`、`--force`：在覆盖前不询问
     
     `-i`：在覆盖文件前询问
     
     `-u`、`--update`：当源文件比目标文件更新或目标文件不存在时才进行复制。

## 文件内容查阅

1. `cat` (concatenate)：打印文件内容，从第一行开始读取
   
   - 语法：`cat [-option] file1 file2 ...`
   
   - 选项（部分）：
     
     `-A`：打印所有内容，包括特殊字符，如`Tab`会以`^I`表示
     
     `-b`、`--number-nonblank`：列出行号，仅针对非空白行，空白行不显示行号。
     
     `-E`、`--show-ends`：将一行的结尾用`$`进行标注。
     
     `-n`、`--number`：列出所有行行号，包括空白行
   
   - 示例：
   
   ```
   bonniehky@Bonnie_U:/mnt/d/BonnieHKY.github.io$ cat index.html
   <!DOCTYPE html>
   <html lang="en">
   <head>
    ...
   </head>
   <body>
     ...
   </body>
   </html>
   ```

2. `tac` (`cat` in reverse)：打印文件内容，从最后一行开始读取
   
   - 示例：
   
   ```
   bonniehky@Bonnie_U:/mnt/d/BonnieHKY.github.io$ tac index.html
   </html>
   </body>
     ...
   <body>
   </head>
     ...
   <head>
   <html lang="en">
   <!DOCTYPE html>
   ```

3. `nl` (number lines)：添加行号打印文件内容
   
   - 语法：`nl [-options] file`
   
   - `nl`命令与`cat -n`的区别在于`nl`可以设置更多样化的添加标号的方式，可以通过`man nl`查询
   
   - 选项与参数（部分）：
     
     `-b`：指定行号标记方式，主要有`-b a`（无论是否有空行，都进行标号），`-b t`（不对空行进行标号，默认）
     
     `-n`：列出行号的表示方式，主要有`-n ln`行号在屏幕的最左方显示，`-n rn`行号在自己字段的最右方显示，且不加0，`-n rz`行号在自己字段的最左方显示，加0（默认为6个字符）
     
     `-w [number]`：设置行号字段占用的字符数。
   
   - 示例：
     
     `nl index.html`：
     
     ```
          1  <!DOCTYPE html>
          2  <html lang="en">
          3  <head>
          ...
     ```
     
     `nl -n rz index.html`：
     
     ```
     000001  <!DOCTYPE html>
     000002  <html lang="en">
     000003  <head> 
     ...
     ```
     
     `nl -n rz -w 3 index.html`：
     
     ```
     001     <!DOCTYPE html>
     002     <html lang="en">
     003     <head> 
     ...
     ```

4. `more`：可翻页地查看文件打印结果
   
   - 操作：
     
     `SPACE`：下一页
     
     `b`：上一页
     
     `q`：退出
     
     `/STRING`：直接输入后回车，向下搜寻”STRING“关键字
     
     `:f`：直接输入无需回车，可立即显示文件名以及目前显示的行数

5. `less`：可翻页地查看文件打印结果，与`more`类似，但用法更为灵活
   
   - 操作：
     
     `SPACE`或[pagedown]：下一页
     
     [pageup]：上一页
     
     `/STRING`：向下搜寻”STRING“关键字
     
     `?STRING`：向上搜寻”STRING“关键字
     
     `n`：重复前一个搜寻
     
     `N`：反向重复前一个搜寻
     
     `g`：前进到数据的第一行
     
     `G`：前进到数据的最后一行
     
     `q`：退出

6. `head`：打印一个文件的前面几行
   
   - 语法：`head [-n number] file`
   
   - 选项：`-n number`代表显示行数，若不设定，则一般显示10行；若number为负数，如`-n x`则表示打印前面所有行内容，除了最后x行。

7. `tail`：打印一个文件的后面几行
   
   - 语法：`tail [-n number] file`
   
   - 选项：`-n number`代表显示行数，若不设定，则一般显示10行；若想打印第x行之后的内容则使用，如`-n +x`。
   
   - 如果想打印第x行至第y行的内容：`head -n y filename | tail -n y-x+1`，`|`表示前面指令输出的讯息交由后续指令继续使用。

8. `od`：读取执行文本，由于可执行文本通常是binary file，因此使用该命令读取其内容。如果使用`cat`读取，可能读取到乱码。

## 文件的搜索

1. `which`：寻找可执行文件（搜索指令的文件路径）
   
   - 语法：`which [-a] command`
   
   - 选项：`-a`表示所有由PATH目录中可以找到的指令均列出，而不止第一个被找到的指令
   
   - 示例：
     
     ```
     bonniehky@Bonnie_U:~$ which ls
     /usr/bin/ls
     bonniehky@Bonnie_U:~$ which -a ls
     /usr/bin/ls
     /bin/ls
     ```

2. `type`

3. `whereis`：仅在系统的特定目录下搜索文件或目录的路径
   
   - 语法：`whereis [-options] filename/dirname`
   
   - 选项：
     
     `-l`：可以罗列出`whereis`去查找的几个主要目录，主要针对binary可执行文件和`/usr/share/man`下的man page文件等。
     
     `-b`：只找binary格式的文件
     
     `-m`：只找在说明文档路径`/usr/share/man`下的文件
     
     `-s`：只找source来源文件
     
     `-u`：查找不在上述三个项目中的其它特殊文件

4. `locate`、`plocate`：查找包含关键词的文件
   
   - `locate`寻找数据是由已创建的数据库/var/lib/mlocate/mlocate.db中查找（`plocate`对应/var/lib/plocate/plocate.db），而不用在硬盘中查找，因此查找速度很快。`plocate`是一个更新、搜索速度更快的`locate`。
   
   - 语法：`(p)locate [-options] keywords`
   
   - 选项：
     
     `-i`、`--igonre-case`：忽略大小写差异
     
     `-c`、`--count`：不输出文件路径，仅输出找到的文件总数
     
     `-l number`：仅输出几个文件
     
     `-r`：后面可接正则表达式的显示方式
   
   - `updatedb`：手动更新/var/lib/plocate/plocate.db内的数据。由于`plocate`仅在数据中查找文件，而数据库默认一天更新一次，因此可能无法查找到新创建的文件。为了能够找到文件，需要手动进行更新。

5. `find`：在硬盘中，在指定目录下查找指定文件
   
   - 语法：`find [PATH] [option] [action]`
   
   - 选项与参数见[Linux下find命令详解](https://blog.csdn.net/l_liangkk/article/details/81294260)

## 文件的压缩和打包

- Linux中压缩指令很多，不同压缩方式对应的解压技术也不相同。可以通过压缩文件的拓展名（虽然Linux中文件的属性与拓展名无关，这仅是为了便于区分不同压缩文件的压缩方式）判断该文件是如何压缩，因而选择对应的解压方式。

- 常见的压缩文件拓展名：
  
  | 拓展名        | 压缩程序               |
  | ---------- | ------------------ |
  | `.Z`       | compress程序压缩       |
  | `.zip`     | zip程序压缩（支持Windows） |
  | `.gz`      | gzip程序压缩           |
  | `.bz2`     | bzip2程序压缩          |
  | `.xz`      | xz程序压缩             |
  | `.tar`     | tar程序打包的数据，没有进行压缩  |
  | `.tar.gz`  | tar程序打包后由gzip程序压缩  |
  | `.tar.bz2` | tar程序打包后由bzip2程序压缩 |
  | `.tar.xz`  | tar程序打包后由xz程序压缩    |

- 常用压缩指令
  
  |       | `gzip`                   | `bzip2`                    | `xz`                     |
  | ----- | ------------------------ | -------------------------- | ------------------------ |
  | 语法    | `gzip [-cdtv#] filename` | `bzip2 [-cdkvz#] filename` | `xz [-cdklt#] filename`  |
  | 读取文本  | `zcat`, `zmore`, `zless` | `bcat`, `bmore`, `bless`   | `xcat`, `xmore`, `xless` |
  | 搜寻关键字 | `zgrep`                  | `bgrep`                    | `xgrep`                  |
  
  - 选项与参数
    
    `-c`：把压缩的数据输出到屏幕上
    
    `-d`：进行解压缩
    
    `-k`：在压缩和解压缩的过程中，保留原文件不删除
    
    `-l`：列出压缩文件的相关信息（压缩体积，压缩比等）
    
    `-t`：用于检验压缩文件的一致性，
    
    `-v`：可以显示出压缩比以及压缩前后文件的名称
    
    `-#`：`#`为数字1-9，即压缩等级，默认为6，压缩等级越小，压缩速度越快，相应压缩后程度越小。

- `tar`：打包（压缩）指令
  
  上述压缩指令在压缩一个目录下的文件时，实质上是对目录下的文件逐个压缩，而不是像WIndows一样压缩成一整个文件包。
  
  为了把多个目录和文件压缩成一个大文件，需要使用`tar`指令，在使用`tar`时，可以通过选项在打包的同时进行压缩。
  
  - 语法
    
    打包压缩：`tar [-z/j/J] [-cv] [-f new_file] source...`
    
    解压缩：`tar [-z/j/J] [-xv] [-f tarfile] [-C dirname]`
    
    查看：`tar [-z/j/J] [tv] [-f tarfile]`
  
  - 选项
    
    `-z`, `-j`, `-J`：选择相应的压缩方式，分别对应`gzip`，`bzip2`，`xz`。三个选项不能同时出现在一个表达式中。
    
    `-c`：创建打包文件
    
    `-x`：解打包/解压缩文件，与`-C`连用可以解压到指定目录下
    
    `-t`：查看打包文件包含哪些文件名
    
    `-v`：在压缩/解压的过程中查看正在处理的文件名
    
    `--exclude=filename`：不打包目录下的某些文件
  
  - 

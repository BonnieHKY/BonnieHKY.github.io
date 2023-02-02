# Vim文书编辑器

---

## Vim的简介

Vim是Linux系统上的一款文本编辑器。Vim是由Vi文书编辑器发展而来，Vim可以用颜色显示程序码，会根据文件的扩展名或者文件内的开头信息自动调用该文件的语法判断式，因此Vim实质上也是一个“程序编辑器”。

## Vim的四种模式

- Normal mode：正常模式（默认）。一般用于浏览文件（用上下左右移动光标），包括使用复制、粘贴、删除等操作

- Insert mode：插入模式。该模式启动后，进入编辑状态，可以通过键盘输入内容

- Command mode：命令模式。在命令模式中可以执行一些VIM或插件提供的指令，包括设置环境、文件操作、功能调用。

- Visual mode：可视模式。可以进行区块选择，复制粘贴，删除。
  
  ![四种模式的相互切换](..\figure\Linux6.1.png)

## Normal mode

- 用Vim打开文件：`vim filename ...`
  
  如果文件已存在，则打开此文件；若文件不存在，则创建一个新文件。打开后默认进入Normal mode。
1. 光标的移动
   
   *字符间移动*
   
   `h`，$\leftarrow$：光标向左移动一个字符
   
   `j`，$\downarrow$：光标向下移动一个字符
   
   `k`，$\uparrow$：光标向上移动一个字符
   
   `l`，$\rightarrow$：光标向右移动一个字符
   
   `n<space>`：光标向右移动n个字符
   
   `0`，`[Home]`：移动到当前行最前面的字符处
   
   `$`，`[End]`：移动到当前行最后面的字符处
   
   *行间移动*
   
   `H`：移动到当前屏幕最上方一行的第一个字符处
   
   `M`：移动到当前屏幕中间一行的第一个字符处
   
   `L`：移动到当前屏幕最后一行的第一个字符处
   
   `+`：移动到非空白字符的下一行
   
   `-`：移动到非空字符的上一行
   
   `n<Enter>`：光标向下移动n行
   
   `nG`：移动到这个文件的第n行
   
   *翻页操作*
   
   `gg`：移动到整个文件的第一列
   
   `G`：移动到整个文件的最后一列
   
   `[Ctrl]+f`，`PgDn`：屏幕向下移动一页
   
   `[Ctrl]+b`，`PgUp`：屏幕向上移动一页
   
   `[Ctrl]+d`：屏幕向下移动半页
   
   `[Ctrl]+u`：屏幕向上移动半页

2. 删除
   
   *字符操作*
   
   `x`：删除光标后一个字符
   
   `X`：删除光标前一个字符
   
   `nx`：删除光标后的连续n个字符
   
   *行操作*
   
   `dd`：删除光标所在的一整行
   
   `ndd`：删除光标所在的向下n行（包含光标所在行）
   
   `d1G`：删除光标所在行到第一列的所有数据
   
   `dG`：删除光标所在行到最后一行的所有数据
   
   `d$`：删除光标所在处到该行最后一个字符
   
   `d0`：删除光标所在处到该行第一个字符

3. 复制
   
   `yy`：复制光标所在的一整行
   
   `nyy`：复制光标所在的向下n行
   
   `y1G`：复制光标所在行到第一行的所有数据
   
   `yG`：复制光标所在行到最后一行的所有数据
   
   `y$`：复制光标所在处到该行最后一个字符
   
   `y0`：复制光标所在处到该行的第一个字符

4. 粘贴
   
   `p`：把已复制的数据在光标的下一行进行粘贴
   
   `P`：把已复制的数据在光标的上一行进行粘贴

5. 其它
   
   `J`：把光标所在行和下一行合并成一行
   
   `u`：复原前一个动作
   
   `.`：重复上一个动作

6. 取代
   
   `r`：取代光标后的一个字符（不会进入`Replace`模式）

7. 查找
   
   `/word`：在光标后寻找word的字符串
   
   `?word`：在光标后寻找word的字符串
   
   `n`：重复前一个查找动作
   
   `N`：反向重复前一个查找动作
   
   `\<word`：以word开头的单词；`word\>`：以word结尾的单词；`\<word\>`：word全词匹配

8. 退出Vim
   
   `ZZ`：若文件没有更改，则不储存离开；若文件被改动过，则储存后离开

## Insert mode

- 进入Insert mode
  
  进入Insert mode后，屏幕最下方会出现`-- INSERT --`的字样，此时可以通过键盘输入内容。
  
  - 指令
    
    `i`：从光标所在处开始插入
    
    `I`：从光标所在行的第一个非空字符处开始插入
    
    `a`：从光标所在处的下一个字符处开始插入
    
    `A`：从光标所在行的最后一个字符处开始插入
    
    `o`：从光标所在行的下方新建一行开始插入
    
    `O`：从光标所在行的上方新建一行开始插入

- 进入Replace mode
  
  进入Replace mode后，屏幕最下方会出现`-- REPLACE --`的字样，此时可以依次替代光标后的文字
  
  - 指令
    
    `R`：依次替代光标后的字符

## Command mode

- 进入命令模式：输入`:`进入命令行模式，屏幕最下方会出现输入的指令，指令输入完需要按下`[Enter]`执行指令
1. 储存和离开
   
   `:w`：保存文件
   
   `:w!`：当文件属性为“只读”时，强制写入文件；不过是否能成功写入与文件权限有关
   
   `:w [filename]`：把编辑的数据另存为一个新的文件，命名为filename
   
   `:x,y w [filename]`：把x行至y行的数据另存为一个新的文件，命名为filename
   
   `:q`：离开vim
   
   `:q!`：若曾经修改过文件但不保存直接离开vim
   
   `:wq`：保存文件并退出vim

2. 读取写入
   
   `:r [filename]`：读入filename的文件内容，添加到光标所在行后面

3. 多文件操作
   
   `:e [filename]`：打开另一个文件
   
   `:sp [filename]`：上下分屏当前文件和新打开的文件filename
   
   `:vsp [filename]`：左右分屏当前文件和新打开的文件filename
   
   `:x`：退出文件并保存对文件的修改
   
   `:ls`：查看目前打开的文件
   
   `:bn`：切换到第n个文件

4. 行号
   
   `:set nu`：显示行号
   
   `:set nonu`：取消行号
   
   `:n`：定位到n行

5. 文本替换
   
   语法：`:[range]s/[target]/[substitute]/[flag]`，把target字符串替换成substitute字符串
   
   `range`：作用范围
   
   - 无（默认）：光标所在行
   
   - `%`：全文
   
   - `x,y`：从x行到y行
   
   `flag`：替换标志（相当于options）
   
   - `g`：（global）全部替换，即对于作用范围内所有target换成substitute（默认为仅替换第一个找到的target）
   
   - `c`：（confirmation）替换前显示确认信息
   
   - `i`：（ignore）忽略大小写差异
   
   - `&`：重复上一个选项
   
   - `n`：不进行替换，仅打印有多少个target会被替换

## Visual mode

- 进入Visual mode，可以进行区块选择和复制粘贴、删除
  
  `v`：字符选择，光标经过的地方反白选择，屏幕下方显示`-- VISUAL --`
  
  `V`：行选择，光标经过的地方反白选择，屏幕下方显示`-- VISUAL LINE --`
  
  `y`：把反白选择的区域进行复制
  
  `d`：把反白选择的区域进行删除

## Vim的暂存和数据恢复

在编辑的过程中，Vim会在当前目录中生成一个以`.swp`结尾的临时文件，用于记录编辑的文件内容。如果Vim因为某些原因意外中断而没有保存文件的话，可以根据`.swp`文件进行恢复。

如果因为意外中断，若Vim发现`.swp`文件的存在，则会显示错误信息

```
E325: ATTENTION
Found a swap file by the name ".newMD.md.swp"
          owned by: bonniehky   dated: Thu Feb 02 20:40:41 2023
         file name: /tmp/vitest/newMD.md
          modified: no
         user name: bonniehky   host name: Bonnie_U
        process ID: 136 (STILL RUNNING)
While opening file "newMD.md"
             dated: Thu Feb 02 20:32:01 2023

(1) Another program may be editing the same file.  If this is the case,
    be careful not to end up with two different instances of the same
    file when making changes.  Quit, or continue with caution.
(2) An edit session for this file crashed.
    If this is the case, use ":recover" or "vim -r newMD.md"
    to recover the changes (see ":help recovery").
    If you did this already, delete the swap file ".newMD.md.swp"
    to avoid this message.

Swap file ".newMD.md.swp" already exists!
[O]pen Read-Only, (E)dit anyway, (R)ecover, (Q)uit, (A)bort:
```

- 第一种情况：可能有其它人正在编辑这个文件
  
  - 如果无需编辑内容，则可以打开只读模式`O`
  
  - 如果需要编辑内容，则请其它人完成编辑后再进行编辑

- 第二种情况：因为Vim中断（crashed）而产生的后果
  
  - `R`可以根据`.swp`文件恢复上次未保存的工作，但`.swp`文件不会在recover后自动删除，因此需要手动删除以避免以后每次打开都显示警告信息。
  
  - 如果确认`.swp`文件不用，可以使用`D`把它删除掉

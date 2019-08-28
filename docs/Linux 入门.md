---
typora-copy-images-to: pics
---

---

# Linux 入门

## 安装 CentOS 虚拟机

详细步骤参考博客：

[VMware 安装 CentOS 7，网络配置、安装桌面 ](<https://blog.csdn.net/baidu_32523857/article/details/82880678>)

[CentOS 7 命令行安装 GNOME、KDE 图形界面 ](<https://www.Linuxidc.com/Linux/2018-04/152000.htm>)

1. 准备： CentOS 7 镜像文件， VMware workstation 15 安装包；
2. 安装 VMware workstation 15；
3. 在 VMware 中安装 CentOS 7 虚拟机；
4. CentOS 7 命令行安装 GNOME 图形界面。（此安装过程较长，约 30-60 min）

## CentOS 基本操作

### 图形界面和命令行界面

`init 5` :从命令行界面切换到图形界面

`init 3` :从图形界面切换到命令行界面

### 切换用户

- 打开终端，提示符为 “$”，表明该用户为普通用户；提示符变为 “#”，表明用户为 root 用户。
- 切换为 root 用户，直接输 `su`，回车，输入 root 密码，回车，切换到 root 用户下，此时的提示符变为 “#”。（输入密码时终端是不显示的）
- 切换回普通用户：输入 ` su 用户名`。

![1566102987352](E:\GitHub\Linux-Notes\docs\pics\1566102987352.png)

### 网络配置

[VMware 虚拟机下的 CentOS7 网络配置 ](<https://blog.csdn.net/weixin_41811114/article/details/80678684>)

1. 在 VMware 中将网络连接改为桥接模式；
2. 查看主机的 DNS 地址；
3. 切换为 root 用户；
4. 修改 CentOS 7 网络配置文件：
   - 输入 cd /etc/sysconfig/network-scripts/
   - 输入 vi ifcfg-ens33，打开网络配置文件 ifcfg-ens33
   - 修改 ONBOOT=yes 并增添 DNS1=192.168.31.1（之前查看的本主机的 DNS 地址），此 DNS 地址设为本机的 DNS 地址，输入 Esc :wq!退出
   - 输入 systemctl restart network 重启网络，没有提示任何信息，则表示网络重启成功，如下图所示：

![1566104609952](E:\GitHub\Linux-Notes\docs\pics\1566104609952.png)

5. 打开 Firefox ，会发现已经成功联网，可以访问网页了。

### 安装 VM Tools

1. 安装软件包

安装依赖组件

```
yum -y install gcc gcc-C++ make
yum -y install kernel-devel
```

更新 Kernel 软件包并重新启动 lilux 系统：

```
yum update kernel -y
init 6  或 reboot
```

2. 安装 VM Tools

- 找到 VMware Tools 下的 VMwareTools-10.3.2-9925305.tar.gz 文件，复制到桌面，右击桌面该文件，选择用归档管理器打开，将文件解压到桌面，或者在终端运行解压命令进行解压。

```
cd /home/dreamwhigh/桌面
tar zxvf VMwareTools-10.3.2-9925305.tar.gz
```

![1566144764959](E:\GitHub\Linux-Notes\docs\pics\1566144764959.png)

- 进入解压后的文件夹，右击选择在终端打开，输入 `./vmware-install.pl` 命令，然后一直回车即可。

  或者在终端通过以下命令进入目标文件夹

```
cd /home/dreamwhigh/桌面/vmware-tools-distrib
./vmware-install.pl
```

![1566145170616](E:\GitHub\Linux-Notes\docs\pics\1566145170616.png)

3. 如若 VM Tools 未生效，可以重启虚拟机。

### 个人目录文件夹路径中文转英文

```
export LANG=en_US
xdg-user-dirs-gtk-update
# 跳出对话框询问是否将目录转化为英文路径,同意并关闭.
export LANG=zh_CN
# 关闭终端,并重启
```

## vim

所有的 Unix Like 系统都会内建 vi 文书编辑器，老式的字处理器。

Vim 是从 vi 发展出来的一个文本编辑器，代码补完、编译及错误跳转等方便编程的功能特别丰富，是程序开发者的一项很好用的工具。

### vim 模式

#### 一般指令模式（Command mode）

启动 vim 后默认处于命令模式，常用命令如下：

- **i** 切换到输入模式，以输入字符。
- **x** 删除当前光标所在处的字符。
- **:** 切换到底线命令模式，以在最底一行输入命令。

命令模式只有一些最基本的命令，因此仍要依靠底线命令模式输入更多命令。

#### 插入模式 (Insert-mode)

在命令模式下按下 i 就进入了输入模式。

- **字符按键以及 Shift 组合**，输入字符
- **ENTER**，回车键，换行
- **BACKSPACE**，退格键，删除光标前一个字符
- **DEL**，删除键，删除光标后一个字符
- **方向键**，在文本中移动光标
- **HOME**/**END**，移动光标到行首/行尾
- **Page Up**/**Page Down**，上/下翻页
- **Insert**，切换光标为输入/替换模式
- **ESC**，退出输入模式，切换到命令模式

#### 底线命令模式（Last line mode）

在命令模式中，按下：（冒号）键，会进入底线命令模式。

常用的命令有：q（退出）、q!（强制退出）、w（保存）、wq（保存并退出

#### 按键说明

[vim 按键说明 ](<https://www.runoob.com/Linux/Linux-vim.HTML>)

## Linux 启动过程

Linux 的启动过程分为 5 个阶段：

- 内核的引导
- 运行 init
- 系统初始化
- 建立终端 
- 用户登录系统

### 运行级别

7 个运行级别：

- 0： 系统停机（关机）模式；
- 1：单用户模式，root 权限，用于系统维护，禁止远程登陆，类似于 Windows 下的安全模式登录；
- 2：多用户模式，没有 NFS 网络支持；
- 3：完整的多用户文本模式，有 NFS，登陆后进入控制台命令行模式；
- 4：系统未使用，保留一般不用；
- 5：图形化模式，登陆后进入图形 GUI 模式或 GNOME、KDE 图形化界面；
- 6：重启模式。

### 用户登录系统

 一般来说，用户的登录方式有三种：

- 命令行登录
- ssh 登录
- 图形界面登录

### 相关命令

#### shutdown

给系统**计划一个时间**关机。

```shell
# shutdown -p now  ### 关闭机器
# shutdown -H now  ### 停止机器      
# shutdown -r09:35 ### 在 09:35am 重启机器
# shutdown -c  ###取消即将进行的关机操作
```

#### halt 

**通知硬件**来停止所有的 CPU 功能，但是仍然**保持通电**。你可以用它使系统处于低层维护状态。注意在有些情况会它会完全关闭系统。

```shell
# halt             ### 停止机器
# halt -p          ### 关闭机器
# halt --reboot    ### 重启机器
```

#### poweroff

发送一个 **ACPI 信号**来通知系统关机

```shell
# poweroff           ### 关闭机器
# poweroff --halt    ### 停止机器
# poweroff --reboot  ### 重启机器
```

#### reboot

命令 reboot 通知系统重启

```shell
# reboot           ### 重启机器
# reboot --halt    ### 停止机器
# reboot -p        ### 关闭机器
```

## 目录结构

![img](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/Linux-filesystem.png)

**系统启动必须：**

- **/boot：**存放的启动 Linux 时使用的内核文件，包括连接文件以及镜像文件。
- **/etc：**存放**所有**的系统需要的**配置文件**和**子目录列表，**更改目录下的文件可能会导致系统不能启动。
- **/lib**：存放基本代码库（比如 C++库），其作用类似于 Windows 里的 DLL 文件。几乎所有的应用程序都需要用到这些共享库。
- **/sys**： 这是 Linux2.6 内核的一个很大的变化。该目录下安装了 2.6 内核中新出现的一个文件系统 sysfs 。sysfs 文件系统集成了下面 3 种文件系统的信息：针对进程信息的 proc 文件系统、针对设备的 devfs 文件系统以及针对伪终端的 devpts 文件系统。该文件系统是内核设备树的一个直观反映。当一个内核对象被创建的时候，对应的文件和目录也在内核对象子系统中

**指令集合：**

- **/bin：**存放着最常用的程序和指令
- **/sbin：**只有系统管理员能使用的程序和指令。

**外部文件管理：**

- **/dev ：**Device(设备) 的缩写, 存放的是 Linux 的外部设备。**注意：**在 Linux 中访问设备和访问文件的方式是相同的。
- **/media**：类 windows 的**其他设备，**例如 U 盘、光驱等等，识别后 Linux 会把设备放到这个目录下。
- **/mnt**：临时挂载别的文件系统的，我们可以将光驱挂载在/mnt/上，然后进入该目录就可以查看光驱里的内容了。

**临时文件：**

- **/run**：是一个临时文件系统，存储系统启动以来的信息。当系统重启时，这个目录下的文件应该被删掉或清除。如果你的系统上有 /var/run 目录，应该让它指向 run。
- **/lost+found**：一般情况下为空的，系统非法关机后，这里就存放一些文件。
- **/tmp**：这个目录是用来存放一些临时文件的。

**账户：**

- **/root**：系统管理员的用户主目录。
- **/home**：用户的主目录，以用户的账号命名的。
- **/usr**：用户的很多应用程序和文件都放在这个目录下，类似于 windows 下的 program files 目录。
- **/usr/bin：**系统用户使用的应用程序与指令。
- **/usr/sbin：**超级用户使用的比较高级的管理程序和系统守护程序。
- **/usr/src：**内核源代码默认的放置目录。

**运行过程中要用：**

- **/var**：存放经常修改的数据，比如程序运行的日志文件（/var/log 目录下）。
- **/proc**：管理**内存空间！**虚拟的目录，是系统内存的映射，我们可以直接访问这个目录来，获取系统信息。这个目录的内容不在硬盘上而是在内存里，我们也可以直接修改里面的某些文件来做修改。

**扩展用的：**

- **/opt**：默认是空的，我们安装额外软件可以放在这个里面。
- **/srv**：存放服务启动后需要提取的数据**（不用服务器就是空）**

## 文件基本属性

使用 `ll` 或者 `ls –l`命令来显示一个文件的属性以及文件所属的用户和组，如：

```shell
[dreamwhigh@localhost 文档 ]$ ls -l
总用量 12
drwxrwxr-x. 2 dreamwhigh dreamwhigh    6 8 月  19 10:54 dirtest1
-rw-r--r--. 1 dreamwhigh dreamwhigh   43 8 月  18 13:35 file 1
-rw-r--r--. 1 dreamwhigh dreamwhigh   43 8 月  19 10:55 file 2
-rw-rw-r--. 1 dreamwhigh dreamwhigh    0 8 月  19 11:02 file3
-rw-r--r--. 1 dreamwhigh dreamwhigh 3313 8 月  19 10:48 Linux

```

第 0 位确定**文件类型**，第 1-3 位确定**属主（该文件的所有者）**拥有该文件的权限，第 4-6 位确定**属组（所有者的同组用户**）拥有该文件的权限，第 7-9 位确定**其他用户**拥有该文件的权限。

第一个字符代表这个文件是目录、文件或链接文件等等。

- **d**：目录
- **-** ：文件；
- **l** ：链接文档 (link file)；
- **b**：装置文件里面的可供储存的接口设备 (可随机存取装置)；
- **c**：装置文件里面的串行端口设备，例如键盘、鼠标。

接下来的字符中，以三个为一组，且均为『rwx』 的三个参数的组合。

| 字符 | 含义             |
| ---- | ---------------- |
| r    | 可读 (read)      |
| w    | 可写 (write)     |
| x    | 可执行 (execute) |
| -    | 没有权限         |

### 更改文件属性

#### chgrp

```
chgrp [-R] 属组名 文件名
chown [–R] 属主名 文件名
chown [-R] 属主名：属组名 文件名
```

#### 属性设置方法

基本权限就有九个，分别是 owner/group/others 三种身份各有自己的 read/write/execute 权限

##### 两种设置方法

- 数字设置

分值对照表如下：

| 字符 | 分值 |
| ---- | ---- |
| r    | 4    |
| w    | 2    |
| e    | 1    |

每种身份 (owner/group/others) 各自的三个权限 (r/w/x) 分数累加。

```
权限为：-rwxrwx---
owner = rwx = 4+2+1 = 7
group = rwx = 4+2+1 = 7
others= --- = 0+0+0 = 0

# chmod [-R] 770 文件或目录名
```

- 符号设置

```
将文件权限设置为 -rwxr-xr-- 
# chmod u=rwx,g=rx,o=r 文件或目录名
```

## 文件与目录管理

### 路径

#### 绝对路径

路径的写法，由根目录 / 写起：

`/usr/share/doc `

#### 相对路径

不是由 / 写起，例如由 /usr/share/doc 要到 /usr/share/man 底下时：

`cd ../man`

##### 相关符号说明

| 符号  | 目录                     |
| ----- | ------------------------ |
| 。    | 用户所处的当前目录       |
| .     | 此层目录                 |
| ..    | 上一级目录               |
| -     | 前一个工作目录           |
| ~     | 当前用户自家的根目录     |
| ~USER | 用户名为 USER 家的根目录 |

### 处理目录的常用命令

- ls: 列出目录
- cd：切换目录
- pwd：显示当前的目录
- mkdir：创建一个新的目录
- rmdir：删除一个空的目录
- touch：更新文件时间或者建立新文件
- cp: 复制文件或目录
- rm: 移除文件或目录
- mv: 移动文件与目录，或修改文件与目录的名称

可以使用 *man [命令 ]* 来查看各个命令的使用文档，如 ：man cp。

#### ls 列出目录

```HTML
# ls [-aAdfFhilnrRSt] file|dir
-a ：列出全部的文件
-d ：仅列出目录本身
-l ：以长数据串行列出，包含文件的属性与权限等等数据
```

#### cd 切换目录

Change Directory

```HTML
cd [相对路径或绝对路径 ]
```

#### pwd 显示当前目录

Print Working Directory ，以绝对路径的方式显示用户当前工作目录

```HTML
[dreamwhigh@localhost Documents]$ pwd 
/home/dreamwhigh/Documents
```

#### mkdir 创建目录

```HTML
# mkdir [-mp] 目录名称
-m ：配置目录权限
-p ：递归创建目录
```

举例

```HTML
#直接创建
[dreamwhigh@localhost Documents]$ rmdir dir2
[dreamwhigh@localhost Documents]$ ll
总用量 0
drwxrwxr-x. 2 dreamwhigh dreamwhigh 6 8 月  19 22:33 dir1

#配置目录权限为 741
[dreamwhigh@localhost Documents]$ mkdir -m 741 dir2
[dreamwhigh@localhost Documents]$ ll
总用量 0
drwxrwxr-x. 2 dreamwhigh dreamwhigh 6 8 月  19 22:33 dir1
drwxr----x. 2 dreamwhigh dreamwhigh 6 8 月  19 22:42 dir2

#递归创建目录（包含上一级目录）
#直接从创建失败
[dreamwhigh@localhost Documents]$ mkdir dir3/dir31
mkdir: 无法创建目录"dir3/dir31": 没有那个文件或目录
#利用参数 -p 后创建成功
[dreamwhigh@localhost Documents]$ mkdir -p dir3/dir31
[dreamwhigh@localhost Documents]$ ls
dir1  dir2  dir3  f1  f2
[dreamwhigh@localhost Documents]$ cd ./dir3
[dreamwhigh@localhost dir3]$ ls
dir31
```

#### rmdir 删除目录

目录必须为空

```HTML
rmdir 目录名称
```

#### touch 更新文件时间或创建文件

```HTML
# touch [-acdmt] filename
-a ： 更新 atime
-c ： 更新 ctime，若该文件不存在则不建立新文件
-m ： 更新 mtime
-d ： 后面可以接更新日期而不使用当前日期，也可以使用 --date="日期或时间"
-t ： 后面可以接更新时间而不使用当前时间，格式为[YYYYMMDDhhmm]
```

三种时间属性：

- atime（access time）：访问时间，读一次这个文件的内容，这个时间就会更新。
- mtime（modifiy time）：修改时间，是文件内容最后一次被修改时间。ls -l 列出的时间就是这个时间。 
- ctime（change time）：状态改动时间，是在写入文件、更改所有者、权限或链接设置时随 i 节点的内容更改而更改的，是该文件的 i 节点最后一次被修改的时间，通过 chmod、chown 命令修改一次文件属性，这个时间就会更新。

#### cp 复制

如果源文件有两个以上，则目的文件一定要是目录才行。

```HTML
cp [-adfilprsu] source destination
-a ：相当于 -dr --preserve=all
-d ：若来源文件为链接文件，则复制链接文件属性而非文件本身
-i ：若目标文件已经存在时，在覆盖前会先询问
-p ：连同文件的属性一起复制过去
-r ：递归复制
-u ：destination 比 source 旧才更新 destination，或 destination 不存在的情况下才复制
--preserve=all ：除了 -p 的权限相关参数外，还加入 SELinux 的属性, links, xattr 等也复制了
```

#### rm 删除

可用于删除非空目录 `rm -rf 目录`

```HTML
# rm [-fir] 文件或目录
-f ：就是 force 的意思，忽略不存在的文件，不会出现警告信息
-i ：互动模式，在删除前会询问使用者是否动作
-r ：递归删除
```

#### mv 移动

```HTML
# mv [-fiu] source destination
# mv [options] source1 source2 source3 .... directory
-f ： force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖
-i ：若目标文件 (destination) 已经存在时，就会询问是否覆盖！
-u ：若目标文件已经存在，且 source 比较新，才会升级 (update)
```

mv 实现重命名

```HTML
# mv ./dir21 ./dir11
```

以上命令相当于将 dir21 目录重命名为 dir11

### 文件内容查看

- cat  由第一行开始显示文件内容
- tac  从最后一行开始显示，可以看出 tac 是 cat 的倒著写！
- nl   显示的时候，顺道输出行号！
- more 一页一页的显示文件内容
- less 与 more 类似，但是比 more 更好的是，他可以往前翻页！
- head 只看头几行
- tail 只看尾巴几行

#### cat

由第一行开始显示文件内容

```HTML
# cat [-AbEnTv]
-A ：相当于 -vET 的整合选项，可列出一些特殊字符而不是空白而已；
-b ：列出行号，空白行不标行号；
-n ：列出行号，连同空白行也会有行号；
-E ：将结尾的断行字节 $ 显示出来；
-T ：将 [tab] 按键以 ^I 显示出来；
-v ：列出一些看不出来的特殊字符
```

#### tac

tac 与 cat 命令刚好相反，文件内容从最后一行开始显示，可以看出 tac 是 cat 的倒着写。

#### nl

显示行号

```HTML
# nl [-bnw] 文件
-b ：指定行号指定的方式，主要有两种：
-b a ：表示不论是否为空行，也同样列出行号 (类似 cat -n)；
-b t ：如果有空行，空的那一行不要列出行号 (默认值)；
-n ：列出行号表示的方法，主要有三种：
-n ln ：行号在荧幕的最左方显示；
-n rn ：行号在自己栏位的最右方显示，且不加 0 ；
-n rz ：行号在自己栏位的最右方显示，且加 0 ；
-w ：行号栏位的占用的位数。
```

#### more

一页一页翻动，more 运行时可以输入的命令有：

- 空白键 (space)：代表向下翻一页；
- Enter ：代表向下翻『一行』；
- /字串：代表在这个显示的内容当中，向下搜寻『字串』这个关键字；
- :f ：立刻显示出档名以及目前显示的行数；
- q ：代表立刻离开 more ，不再显示该文件内容；
- b 或 [ctrl]-b ：代表往回翻页，不过这动作只对文件有用，对管线无用。

#### less

一页一页翻动，less 运行时可以输入的命令有：

- 空白键    ：向下翻动一页；
- [pagedown]：向下翻动一页；
- [pageup]  ：向上翻动一页；
- /字串     ：向下搜寻『字串』的功能；
- ?字串     ：向上搜寻『字串』的功能；
- n         ：重复前一个搜寻 (与 / 或 ? 有关！)
- N         ：反向的重复前一个搜寻 (与 / 或 ? 有关！)
- q         ：离开 less 这个程序；

#### head

取出文件前面几行

```HTML
# head [-n number] 文件 
-n ：后面接数字，代表显示几行的意思，n 默认为 10
```

#### tail

取出文件后面几行

```HTML
tail [-n number] 文件 
-n ：后面接数字，代表显示几行的意思，n 默认为 10
-f ：表示持续侦测后面所接的档名，要等到按下[ctrl]-c 才会结束 tail 的侦测
```

## 用户和用户组管理

### 系统账号管理

#### useadd

添加新的用户账号

```HTML
# useradd 选项 用户名
-d 目录 指定用户主目录，如果此目录不存在，则同时使用-m 选项，可以创建主目录
-c comment 指定一段注释性描述。
-g 用户组 指定用户所属的用户组。
-G 用户组，用户组 指定用户所属的附加组。
-s Shell 文件 指定用户的登录 Shell。
-u 用户号 指定用户的用户号，如果同时有-o 选项，则可以重复使用其他用户的标识号。

# useradd –d  /home/sam -m sam
创建了一个用户 sam，其中-d 和-m 选项用来为登录名 sam 产生一个主目录 /home/sam
```

#### userdel

删除帐号

```HTML
userdel 选项 用户名
 -r 把用户的主目录一起删除
 
 # userdel -r sam
 删除用户 sam 在系统文件中（主要是/etc/passwd, /etc/shadow, /etc/group 等）的记录，同时删除用户的主目录
```

#### usermod

修改账号

```HTML
# usermod 选项 用户名
-c, -d, -m, -g, -G, -s, -u 以及-o 等，意义与 useradd 命令相同

# usermod -s /bin/ksh -d /home/z –g developer sam
将用户 sam 的登录 Shell 修改为 ksh，主目录改为/home/z，用户组改为 developer
```

#### passwd

用户口令的管理，口令类似于 windows 中的登陆密码

```
# passwd 选项 用户名
-l 锁定口令，即禁用账号
-u 口令解锁
-d 使账号无口令
-f 强迫用户下次登录时修改口令
```

### 系统用户组的管理

#### groupadd

增加一个新的用户组

```HTML
# groupadd 选项 用户组
-g GID 指定新用户组的组标识号（GID）
-o 一般与-g 选项同时使用，表示新用户组的 GID 可以与系统已有用户组的 GID 相同
```

#### groupdel

删除一个已有的用户组

```HTML
# groupdel 用户组
```

#### groupmod

修改用户组的属性

```HTML
groupmod 选项 用户组
-g，-o 与 groupadd 中意义类似
-n 新用户组 将用户组的名字改为新名字
```

#### newgrp

如果一个用户同时属于多个用户组，那么用户可以在用户组之间切换，以便具有其他用户组的权限，使用命令 newgrp 切换到其他用户组

```HTML
$ newgrp 目的用户组
要去目的用户组确实是该用户的主组或附加组
```



```HTML
＃ cat /etc/passwd

root:x:0:0:Superuser:/:
daemon:x:1:1:System daemons:/etc:
bin:x:2:2:Owner of system commands:/bin:
sys:x:3:3:Owner of system files:/usr/sys:
adm:x:4:4:System accounting:/usr/adm:
uucp:x:5:5:UUCP administrator:/usr/lib/uucp:
auth:x:7:21:Authentication administrator:/tcb/files/auth:
cron:x:9:16:Cron daemon:/usr/spool/cron:
listen:x:37:4:Network daemon:/usr/net/nls:
lp:x:71:18:Printer administrator:/usr/spool/lp:
sam:x:200:50:Sam san:/home/sam:/bin/sh
```

### 用户账号相关的系统文件

#### /etc/passwd 文件

```HTML
＃ cat /etc/passwd

root:x:0:0:Superuser:/:
daemon:x:1:1:System daemons:/etc:
bin:x:2:2:Owner of system commands:/bin:
sys:x:3:3:Owner of system files:/usr/sys:
adm:x:4:4:System accounting:/usr/adm:
uucp:x:5:5:UUCP administrator:/usr/lib/uucp:
auth:x:7:21:Authentication administrator:/tcb/files/auth:
cron:x:9:16:Cron daemon:/usr/spool/cron:
listen:x:37:4:Network daemon:/usr/net/nls:
lp:x:71:18:Printer administrator:/usr/spool/lp:
sam:x:200:50:Sam san:/home/sam:/bin/sh
```

/etc/passwd 中一行记录对应着一个用户，每行记录又被冒号 (:) 分隔为 7 个字段，其格式和具体含义如下：

```HTML
用户名:口令:用户标识号:组标识号:注释性描述:主目录:登录 Shell
```

以上用户中包含了伪用户，这些用户在/etc/passwd 文件中也占有一条记录，但是不能登录，因为它们的登录 Shell 为空。它们的存在主要是方便系统管理，满足相应的系统进程对文件属主的要求。

常见的伪用户如下所示：

```HTML
bin 拥有可执行的用户命令文件 
sys 拥有系统文件 
adm 拥有帐户文件 
uucp UUCP 使用 
lp lp 或 lpd 子系统使用 
nobody NFS 使用
```

#### 拥有帐户文件

##### 伪用户

除了上面列出的伪用户外，还有许多标准的伪用户，例如：audit, cron, mail, usenet 等，它们也都各自为相关的进程和文件所需要。

##### /etc/shadow 文件

/etc/shadow 中的记录行与/etc/passwd 中的一一对应，它由 pwconv 命令根据/etc/passwd 中的数据自动产生。

它的文件格式与/etc/passwd 类似：

```
登录名:加密口令:最后一次修改时间:最小时间间隔:最大时间间隔:警告时间:不活动时间:失效时间:标志
```

示例：

```hmtl
＃ cat /etc/shadow

root:Dnakfw28zf38w:8764:0:168:7:::
daemon:*::0:0::::
bin:*::0:0::::
sys:*::0:0::::
adm:*::0:0::::
uucp:*::0:0::::
nuucp:*::0:0::::
auth:*::0:0::::
cron:*::0:0::::
listen:*::0:0::::
lp:*::0:0::::
sam:EkdiSECLWPdSa:9740:0:0::::
```

##### /etc/group 文件

用户组的所有信息都存放在/etc/group 文件中。此文件的格式也类似于/etc/passwd 文件：

```HTML
组名:口令:组标识号:组内用户列表
```

## 磁盘管理

### 常用命令

- df：列出文件系统的整体磁盘使用量
- du：检查磁盘空间使用量
- fdisk：用于磁盘分区

#### df

检查文件系统的磁盘空间占用情况。可以利用该命令来获取硬盘被占用了多少空间，目前还剩下多少空间等信息。

```HTML
df [-ahikHTm] [目录或文件名 ]
-a ：列出所有的文件系统，包括系统特有的 /proc 等文件系统；
-k ：以 KBytes 的容量显示各文件系统；
-m ：以 MBytes 的容量显示各文件系统；
-h ：以人们较易阅读的 GBytes, MBytes, KBytes 等格式自行显示；
-H ：以 M=1000K 取代 M=1024K 的进位方式；
-T ：显示文件系统类型, 连同该 partition 的 filesystem 名称 (例如 ext3) 也列出；
-i ：不用硬盘容量，而以 inode 的数量来显示
```

#### du

对文件和目录磁盘使用的空间的查看。

```HTML
du [-ahskm] 文件或目录名称
-a ：列出所有的文件与目录容量，因为默认仅统计目录底下的文件量而已。
-h ：以人们较易读的容量格式 (G/M) 显示；
-s ：列出总量而已，而不列出每个各别的目录占用容量；
-S ：不包括子目录下的总计，与 -s 有点差别。
-k ：以 KBytes 列出容量显示；
-m ：以 MBytes 列出容量显示；
```

#### fdisk

fdisk 是 Linux 的磁盘分区表操作工具。

```HTML
fdisk [-l] 装置名称
-l ：输出后面接的装置所有的分区内容。若仅有 fdisk -l 时， 则系统将会把整个系统内能够搜寻到的装置的分区均列出来。
```

#### mkfs 磁盘格式化

磁盘分割完毕后需要进行文件系统的格式化

```HTML
mkfs [-t 文件系统格式 ] 装置文件名
-t ：可以接文件系统格式，例如 ext3, ext2, vfat 等 (系统有支持才会生效)
```

#### fsck 磁盘检验

fsck（file system check）用来检查和维护不一致的文件系统，若系统掉电或磁盘发生问题，可利用 fsck 命令对文件系统进行检查。

```HTML
fsck [-t 文件系统 ] [-ACay] 装置名称
-t : 给定档案系统的型式，若在 /etc/fstab 中已有定义或 kernel 本身已支援的则不需加上此参数
-s : 依序一个一个地执行 fsck 的指令来检查
-A : 对/etc/fstab 中所有列出来的 分区（partition）做检查
-C : 显示完整的检查进度
-d : 打印出 e2fsck 的 debug 结果
-p : 同时有 -A 条件时，同时有多个 fsck 的检查一起执行
-R : 同时有 -A 条件时，省略 / 不检查
-V : 详细显示模式
-a : 如果检查有错则自动修复
-r : 如果检查有错则由使用者回答是否修复
-y : 选项指定检测每个文件是自动输入 yes，在不确定那些是不正常的时候，可以执行 # fsck -y 全部检查修复。
```

#### 磁盘挂载与卸除

Linux 的磁盘挂载使用 `mount` 命令，卸载使用 `umount` 命令。

```
mount [-t 文件系统 ] [-L Label 名 ] [-o 额外选项 ] [-n]  装置文件名  挂载点

umount [-fn] 装置文件名或挂载点
-f ：强制卸除！可用在类似网络文件系统 (NFS) 无法读取到的情况下；
-n ：不升级 /etc/mtab 情况下卸除
```

## yum 命令

yum（ Yellow dog Updater, Modified）是一个在 Fedora 和 RedHat 以及 SUSE 中的 Shell 前端软件包管理器。

基於 RPM 包管理，能够从指定的服务器自动下载 RPM 包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软体包，无须繁琐地一次次下载、安装。

```
yum [options] [command] [package ...]
options：可选，选项包括-h（帮助），-y（当安装过程提示选择全部为"yes"），-q（不显示安装的过程）等等。
command：要进行的操作。
package 操作的对象。
```

### 常用命令

- 列出所有可更新的软件清单命令：yum check-update
- 更新所有软件命令：yum update
- 仅安装指定的软件命令：yum install <package_name>
- 仅更新指定的软件命令：yum update <package_name>
- 列出所有可安裝的软件清单命令：yum list
- 删除软件包命令：yum remove <package_name>
- 查找软件包 命令：yum search < keyword >
- 清除缓存命令:
  - yum clean packages: 清除缓存目录下的软件包
  - yum clean headers: 清除缓存目录下的 headers
  - yum clean oldheaders: 清除缓存目录下旧的 headers
  - yum clean, yum clean all (= yum clean packages; yum clean oldheaders) :清除缓存目录下的软件包及旧的 headers

## 其他

### 实体连接与符号连接 ln

[实体连接与符号连接](<https://blog.csdn.net/JiangJunDriver/article/details/71429527>)

## 参考资料

[菜鸟驿站 - Linux 教程](<https://www.runoob.com/linux/linux-tutorial.html>)
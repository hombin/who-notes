# 运维 Linux 学习笔记



## 目录

- [Linux 基础](##Linux-基础)
  - [文件系统](###文件系统)
    - [文件](####文件) [基本目录](####基本目录)
    - [编辑器](####编辑器)
  - [系统操作](###系统操作)
    - [开关机 & 重启](####开关机-&-重启)
    - [用户操作](####用户操作) [用户管理](####用户管理) [用户组管理](####用户组管理)
    - [找回 root 密码](####找回-root-密码)
  - [常见命令](###常见命令)
    - [帮助命令](####帮助命令) 
    - [文件命令](####文件命令) [文件夹命令](####文件夹命令)  [目录命令](####目录命令)
    - [磁盘命令](####磁盘命令) [网络命令](####网络命令) [系统命令](####系统命令)
- [Linux 实操](##Linux-实操)
  - [权限管理](###权限管理)
    - [组管理](####组管理) [权限控制](####权限控制)
  - [定时任务](###定时任务)
    - [crontab](####crontab) [at](####at)
  - [磁盘分区 & 挂载](###磁盘分区-&-挂载)
    - [分区](####分区) [挂载](####挂载) [扩容](####扩容)
  - [网络配置](###网络配置)
    - [查看网络](####查看网络) [测试网络](####测试网络) [DNS](####DNS)
  - 进程管理
    - 查看进程 终止进程 进程树
  - 包管理
    - RPM YUM
  - Shell 编程（待补充）
    - 脚本格式 变量 环境变量 预定义变量
    - 位置参数 运算符 条件判断 流程判断
    - 循环 控制台输入 函数
- Linux 系统
  - 防火墙
    - iptable firewall
  - 日志控制
    - 系统日志 日志轮替 内存日志
  - 备份与恢复
    - dump restore



## Linux 基础

### 文件系统

#### 文件

1. Linux 系统中一切皆为文件
2. Linux 文件系统 为树状文件目录



#### 基本目录

1. /bin：binary 缩写，存放常用命令和程序
2. /sbin：s 是 super user，存放 系统管理员 的系统管理程序和命令
3. /home：存放普通用户的主目录，以用户的账号命名文件夹
4. /root：系统管理员，或超级权限者的用户主目录
5. /lib：系统或者程序最基本的动态连接共享库目录，类似于 Windows 下的 DLL 文件
6. /lost+found：使用标准的 ext2 / ext3 档案系统格式才有。当档案系统发生错误时， 将一些遗失的片段放置到这个目录下
7. /etc：所有系统或者程序管理所需要的配置文件和子目录
8. /usr：用户相关的应用程序和文件一般都存放在此目录下，类似于 Windows 下的 program files
9. /boot：存放启动 Linux 时使用的一些核心文件，包括连接文件和镜像文件
10. /proc：一个虚拟目录，是系统内存的映射，通过访问这个目录来获取系统信息
11. /srv：service 缩写。存放一些服务启动之后需要提取的数据
12. /sys：安装 Linux 2.6 版本内核 新出现的一个文件系统 sysfs
13. /tmp：存放一些临时文件
14. /dev：以文件形式存储连接的所有硬件的信息，类似于 Windows 的设备管理器
15. /media：挂载 Linux 识别的外部接入设备，比如 U盘，光驱等
16. /mnt：提供给用户临时挂载别的文件系统的目录，通过目录可访问挂载的外部存储
17. /opt：存放主机额外安装软件的目录，默认为空
18. /usr/local：也是存放主机额外安装软件的目录，一般为编译编码方式安装的程序
19. /var：存放经常修改的目录，包括各种日志文件
20. /selinux：security-enhanced linux SELinux 是安全子系统，控制程序只能访问特定文件，有三种工作模式



#### 编辑器

1. Linux 系统有内置的 vi 编辑器，部分版本需要自行安装 Vim
2. Vim 具有程序编辑的能力，可视为 Vi 的增强版本，功能丰富且可定制化
3. 三种模式：
   - 正常模式
     - 默认模式
     - 视图操作 复制 粘贴  删除字符 删除整行
   - 插入模式
     - 按下 i I o O a A r R 均可进入编辑模式，区别在于进入后的光标位置
   - 命令行模式
     - 输入 ESC + 冒号，提供 读取、存盘、替换、显示行号、退出等动作
4. Vim 命令：http://vimdoc.sourceforge.net/htmldoc/help.html#reference_toc



### 系统操作

#### 开关机 & 重启

1. 基本操作

      ```shell
      # 立刻进行关机
      shutdown -h now
      # 指定 [分钟数] 后进行关机
      shutdown -h 1
      # 重启
      shutdown -r now
      # 现在重新启动计算机
      reboot
      
      # halt 指令及其参数
      halt [选项]
      -d 不在wtmp中记录
      -f 强制关机或者重启，不调用shutdown
      -h 让硬件保持standby的状态
      -i 关闭系统前先关闭网络系统
      -n 不执行sync
      -p halt后执行poweroff，只执行halt是停止系统不关闭电源
      -w 在wtmp中记录，不关闭系统
      
      # init 指令
      关机init 0
      重启init 6
      
      # 同步内存数据到磁盘
      sync
      ```
      
2. 注意点

      - 不管重启还是关机，建议先要运行 sync 命令，保存内存数据



#### 用户操作

1. 登录
   - 尽量少用 root 用户直接登录，建议使用普通用户登录，再用 su - 用户名 或者 sudo su 切换系统管理员身份
2. 注销
   - 提示符 logout 可注销用户，exit 可退出切换的用户身份
   - logout 命令在运行级别 3 以下有效，在图形运行级别无效



#### 用户管理

1. Linux 是 多用户多任务  的操作系统，任何使用系统资源的用户，都必须向系统管理员申请账号进入系统

2. 添加用户

   ```shell
   # 添加 [用户名] 的用户，系统会自动创建 /home 下同名的用户目录
   useradd [用户名]
   # 可通过指定 [home目录] 创建 [新用户]
   useradd -d [指定目录] [新的用户名]
   ```

3. 指定 [用户] 修改密码

   ```shell
   # 指定 [用户] 密码
   passwd [用户名]
   ```

4. 删除用户

   ```shell
   # 删除指定 [用户]，但保留用户 home 目录
   userdel [用户名]
   # 删除 [用户] 及其 home 目录
   userdel -r [用户名]
   ```

5. 查询用户

   ```shell
   # 查看当前用户
   whoami
   who am i
   # 查询已知 [用户名]
   id [用户名]
   # 查看系统所有用户
   cat /etc/passwd
   # root:x:0:0:root:/root:/bin/bash
   # 用户名：密码：用户ID：群组ID：用户描述信息：家目录：使用的shell类型
   ```

6. 切换用户

   ```shell
   # 指定 [用户名] 切换
   su - [用户名]
   # 普通用户切换到root用户需要密码，该密码是普通用户的密码
   # 不加 - 是不改变当前变量
   su [用户名]
   ```
   
7. 用户密码影子文件

   ```shell
   # 只有 root 用户拥有读权限，保证密码安全性
   cat /etc/shadow
   # daemon:*:15513:0:99999:7:::
   # 用户名：加密密码：最后修改时间：最小修改时间间隔：密码有效期：密码需要变更前的警告天数：过期后宽限时间：失效时间：保留字段
   ```

   

#### 用户组管理

1. 主组 & 附加组

   - 主组
     - 也叫初始组，是用户登录系统时的组
     - 创建新用户时，若未明确指定该用户所属的主组，会默认创建一个与用户名相同的组，作为该用户的主组
     - 用户创建文件时，文件的所属权限组就是当前用户的主组
     - 使用 `useradd` 命令时用 `-g` 参数可以指定主组，则不会默认创建同名的主组
     - 用户有且只能所属一个主组
     - 用户的主组不能被删除
     - 用户不能直接被移出主组，但可以更换主组
     - 用户被删除时它的主组若没有其他所属用户，则会自动删除该主组
   - 附加组
     - 登录后可切换的其他组
     - 用户可以所属零个或多个附加组
     - 用户的附加组和主组可相同
     - 附加组可以直接被删除而无需关心是否所属于用户
     - 附加组可以新增和移除任意个所属用户
     - 用户被删除时所属附属组不会受影响

2. 添加用户组

   ```shell
   # 指定 [组名] 创建用户组
   groupadd [选项] [组名]
   -g GID 指定新用户组的组标识号（GID）。
   -o 一般与-g选项同时使用，表示新用户组的GID可以与系统已有用户组的GID相同。
   ```

3. 删除用户组

   ```shell
   # 指定 [组名] 删除用户组
   groupdel [组名]
   ```

4. 查看用户组

   ```shell
   # 查看各个组的基本信息
   cat /etc/group
   # hadoop:x:1002:mapred
   # 名：密码：组ID：组内用户列表（仅显示将该组作为附加组的用户）
   ```

5. 修改用户组属性

   ```shell
   # 指定 [组名] 修改属性
   groupmod [选项] [组名]
   -g GID 为用户组指定新的组标识号。
   -o 与-g选项同时使用，用户组的新GID可以与系统已有用户组的GID相同。
   -n 新用户组 将用户组的名字改为新名字
   ```

6. 切换用户组

   ```shell
   # 指定 [组名] 切换用户组
   newgrp [组名]
   ```

7. 修改用户的组

   ```shell
   # 指定 [组名] 修改用户的主组
   usermod -g [新主组组名] [用户名]
   # 将 [用户] 加入附加组
   usermod -G [附加组组名] [用户名]
   # 建议使用gpasswd命令而不是usermod，因为 usermod -G 命令如果不写全用户的附属组，会清空之前的所有附属组
   gpasswd -a [用户名] [附加组组名]
   ```



#### 找回 root 密码

- 指定 单用户 模式进入系统

    1. 输入 reboot 重启进入开机页面迅速按下 esc 键进入到引导界面
    2. 输入小写 e 进入命令编辑模式，用上下键移动到第二项 kernel/vmlinuz
    3. 按小写 e，然后输入 1 按回车把模式更改成单用户模式
    4. 按下b进入启动引导模式，接下来可以看到虚拟机已经进入到了单用户模式，并且使用的是不需要密码的 root 账户登录的
    5. 在命令行输入 passwd root 更改 root 密码。在两次输入密码之后可以看到 root 密码修改成功了
    6. 在命令行输入 reboot 重启机器即可
    
- init 指令说明

    ```shell
    # 指定运行级别
    init [0123456]
    0：关机
    1：单用户【找回丢失密码】
    2：多用户状态没有网络服务
    3：多用户状态有网络服务
    4：系统未使用保留给用户
    5：图形界面
    6：系统重启
    ```



### 常见命令

**Linux 命令大全：https://www.runoob.com/linux/linux-command-manual.html**



#### 帮助命令

1. man

   ```shell
   # man: 获得帮助信息
   man ls
   ```

2. help

   ```shell
   # help: bash 获得 shell 内置命令的帮助信息
   help cd
   ```

3. run-help

    ```shell
    # run-help: zsh 获得 shell 内置命令的帮助信息
    run-help cd
    ```



#### 文件命令

1. touch

   ```shell
   # touch 用于修改文件或者目录的时间属性，包括存取时间和更改时间。若文件不存在，系统会建立一个新的文件。
   touch [-acfm][-d<日期时间>][-r<参考文件或目录>] [-t<日期时间>][文件或目录…]
   
   -a 改变档案的读取时间记录。
   -m 改变档案的修改时间记录。
   -c 假如目的档案不存在，不会建立新的档案。与 --no-create 的效果一样。
   -f 不使用，是为了与其他 unix 系统的相容性而保留。
   -r 使用参考档的时间记录，与 --file 的效果一样。
   -d 设定时间与日期，可以使用各种不同的格式。
   -t 设定档案的时间记录，格式与 date 指令相同。
   ```

2. cat

   ```shell
   # cat 用于连接文件并打印到标准输出设备上，比如控制台
   cat [-AbeEnstTuv] [文件名]
   
   -n 由 1 开始对所有输出的行数编号。
   -b 和 -n 相似，只不过对于空白行不编号。
   -s 当遇到有连续两行以上的空白行，就代换为一行的空白行。
   -v 使用 ^ 和 M- 符号，除了 LFD 和 TAB 之外。
   -E 在每行结束处显示 $。
   -T 将 TAB 字符显示为 ^I。
   ```

3. tail

   ```shell
   # tail 用于查看文件的内容
   tail [参数] [文件]
   tail -f homflow-notes.log  
   
   -f 循环读取
   -q 不显示处理信息
   -v 显示详细的处理信息
   -c <数目> 显示的字节数
   -n <行数> 显示文件的尾部 n 行内容
   --pid=PID 与-f合用,表示在进程ID，PID死掉之后结束
   -q, --quiet, --silent 从不输出给出文件名的首部
   -s, --sleep-interval=S 与-f合用,表示在每次反复的间隔休眠S秒
   ```

4. more

   ```shell
   # more
   more [-dlfpcsu] [-num] [+/pattern] [+linenum] [fileNames..]
   
   -num 一次显示的行数
   -l 取消遇见特殊字元 ^L（送纸字元）时会暂停的功能
   -f 计算行数时，以实际上的行数，而非自动换行过后的行数（有些单行字数太长的会被扩展为两行或两行以上）
   -p 不以卷动的方式显示每一页，而是先清除萤幕后再显示内容
   -c 跟 -p 相似，不同的是先显示内容再清除其他旧资料
   -s 当遇到有连续两行以上的空白行，就代换为一行的空白行
   +/pattern 在每个文档显示前搜寻该字串（pattern），然后从该字串之后开始显示
   +linenum 从第 num 行开始显示
   fileNames 欲显示内容的文档，可为复数个数
   ```

5. less

   ```shell
   # less（和 more 类似）支持翻页和搜索，部分系统需要自行安装
   less [文件名]
   ```

6. cp

   ```shell
   # cp 用于复制文件或目录
   cp [options] source dest
   cp –r test/ newtest       
   
   -a 此选项通常在复制目录时使用，它保留链接、文件属性，并复制目录下的所有内容。其作用等于dpR参数组合。
   -d 复制时保留链接。这里所说的链接相当于 Windows 系统中的快捷方式。
   -f 覆盖已经存在的目标文件而不给出提示。
   -i 与 -f 选项相反，在覆盖目标文件之前给出提示，要求用户确认是否覆盖，回答 y 时目标文件将被覆盖。
   -p 除复制文件的内容外，还把修改时间和访问权限也复制到新文件中。
   -r 若给出的源文件是一个目录文件，此时将复制该目录下所有的子目录和文件。
   -l 不复制文件，只是生成链接文件。
   ```

7. mv

   ```shell
   # mv 用来为文件或目录改名、或将文件或目录移入其它位置
   mv [options] source dest
   mv ./logs/ /home/hombin/demo/logs
   
   -b 当目标文件或目录存在时，在执行覆盖前，会为其创建一个备份。
   -i 如果指定移动的源目录或文件与目标的目录或文件同名，则会先询问是否覆盖旧文件，输入 y 表示直接覆盖，输入 n 表示取消该操作。
   -f 如果指定移动的源目录或文件与目标的目录或文件同名，不会询问，直接覆盖旧文件。
   -n 不要覆盖任何已存在的文件或目录。
   -u 当源文件比目标文件新或者目标文件不存在时，才执行移动操作。
   ```

8. ln

   ```shell
   # ln 软链接 & 硬链接，为某一个文件在另外一个位置建立一个同步的链接
   ln [参数][源文件或目录][目标文件或目录]
   ln -s hobin2021.log link2021
   
   -b 删除，覆盖以前建立的链接
   -d 允许超级用户制作目录的硬链接
   -f 强制执行
   -i 交互模式，文件存在则提示用户是否覆盖
   -n 把符号链接视为一般目录
   -s 软链接(符号链接)
   -v 显示详细的处理过程
   ```

9. look

   ```shell
   # look 命令用于查询单词
   look [-adf][-t<字尾字符串>][字首字符串][字典文件]
   # 查看 testfile 里 L 开头的所有行
   look L testfile
   
   -a 使用另一个字典文件web2，该文件也位于/usr/dict目录下。
   -d 只对比英文字母和数字，其余一慨忽略不予比对。
   -f 忽略字符大小写差别。
   -t<字尾字符串> 设置字尾字符串。
   ```

10. grep

    ```shell
    # grep 用于查找 [文件] 里符合条件的字符串
    grep [-abcEFGhHilLnqrsvVwxy][-A<显示行数>][-B<列数>][-C<列数>][-d<动作>][-e<范本样式>][-f<范本文件>][文件或目录...]
    
    -a 或 --text : 不要忽略二进制的数据。
    -A <显示行数> 或 --after-context=<显示行数> : 除了显示符合范本样式的那一列之外，并显示该行之后的内容。
    -b 或 --byte-offset : 在显示符合样式的那一行之前，标示出该行第一个字符的编号。
    -B <显示行数> 或 --before-context=<显示行数> : 除了显示符合样式的那一行之外，并显示该行之前的内容。
    -c 或 --count : 计算符合样式的列数。
    -C <显示行数> 或 --context=<显示行数>或-<显示行数> : 除了显示符合样式的那一行之外，并显示该行之前后的内容。
    -d <动作> 或 --directories=<动作> : 当指定要查找的是目录而非文件时，必须使用这项参数，否则grep指令将回报信息并停止动作。
    -e <范本样式> 或 --regexp=<范本样式> : 指定字符串做为查找文件内容的样式。
    -E 或 --extended-regexp : 将样式为延伸的正则表达式来使用。
    -f <规则文件> 指定规则文件，其内容含有一个或多个规则样式，让grep查找符合规则条件的文件内容，格式为每行一个规则样式。
    -F 或 --fixed-regexp : 将样式视为固定字符串的列表。
    -G 或 --basic-regexp : 将样式视为普通的表示法来使用。
    -h 或 --no-filename : 在显示符合样式的那一行之前，不标示该行所属的文件名称。
    -H 或 --with-filename : 在显示符合样式的那一行之前，表示该行所属的文件名称。
    -i 或 --ignore-case : 忽略字符大小写的差别。
    -l 或 --file-with-matches : 列出文件内容符合指定的样式的文件名称。
    -L 或 --files-without-match : 列出文件内容不符合指定的样式的文件名称。
    -n 或 --line-number : 在显示符合样式的那一行之前，标示出该行的列数编号。
    -o 或 --only-matching : 只显示匹配PATTERN 部分。
    -q 或 --quiet或--silent : 不显示任何信息。
    -r 或 --recursive : 此参数的效果和指定"-d recurse"参数相同。
    -s 或 --no-messages : 不显示错误信息。
    -v 或 --invert-match : 显示不包含匹配文本的所有行。
    
    # grep 常用语法
    grep [-acinv] [--color=auto] '搜寻字符串' filename
    
    -a ：将 binary 文件以 text 文件的方式搜寻数据
    -c ：计算找到 '搜寻字符串' 的次数
    -i ：忽略大小写的不同，所以大小写视为相同
    -n ：顺便输出行号
    -v ：反向选择，即显示出没有 '搜寻字符串' 内容的那一行
    --color=auto ：可以将找到的关键词部分加上颜色
    
    # grep 正则表达式
    # 查找 test 或者 tast
    grep -n 't[ae]st' regular_express.txt
    # 查找 除了 g 开头的 包含 oo 的行，比如 foo Coo
    grep -n '[^g]oo' regular_express.txt
    # 找出空白行
    grep -n '^$' regular_express.txt
    # 找出包含任意数字的行
    grep -n '[0-9][0-9]*' regular_express.txt
    ```

    

#### 文件夹命令

1. mkdir

   ```shell
   # mkdir
   mkdir [-p] [文件夹名称]
   mkdir -m 777 hombin_logs
   -p parents，表示递归创建目录。确保目录名称存在，不存在的就建一个。
   -m 指定权限并创建目录
   ```

2. tar

   ```shell
   # tar 用于备份文件，可以添加或者解开备份文件 tape archive
   tar [-ABcdgGhiklmMoOpPrRsStuUvwWxzZ][-b <区块数目>][-C <目的目录>][-f <备份文件>][-F <Script文件>][-K <文件>][-L <媒体容量>][-N <日期时间>][-T <范本文件>][-V <卷册名称>][-X <范本文件>][-<设备编号><存储密度>][--after-date=<日期时间>][--atime-preserve][--backuup=<备份方式>][--checkpoint][--concatenate][--confirmation][--delete][--exclude=<范本样式>][--force-local][--group=<群组名称>][--help][--ignore-failed-read][--new-volume-script=<Script文件>][--newer-mtime][--no-recursion][--null][--numeric-owner][--owner=<用户名称>][--posix][--erve][--preserve-order][--preserve-permissions][--record-size=<区块数目>][--recursive-unlink][--remove-files][--rsh-command=<执行指令>][--same-owner][--suffix=<备份字尾字符串>][--totals][--use-compress-program=<执行指令>][--version][--volno-file=<编号文件>][文件或目录...]
   
   # 打包 test.txt
   tar -czvf test.tar.gz test.txt
   # 列出 test.tar.gz 内容
   tar -tzvf test.tar.gz
   # 解包 test.tar.gz
   tar -xzvf test.tar.gz
   
   -c 或 --create	建立新的备份文件。
   -f <备份文件> 或 --file=<备份文件>	指定备份文件。
   -t 或 --list 列出备份文件的内容。
   -v 或 --verbose 显示指令执行过程。
   -x 或 --extract 或 --get 从备份文件中还原文件。
   -z 或 --gzip 或 --ungzip 通过gzip指令处理备份文件。有z参数才有压缩操作。
   ```

3. zip & unzip

   ```shell
   # zip 用于压缩文件，压缩率不如 rar 和 tar.gz	
   zip [-AcdDfFghjJKlLmoqrSTuvVwXyz$][-b <工作目录>][-ll][-n <字尾字符串>][-t <日期时间>][-<压缩效率>][压缩文件][文件...][-i <范本样式>][-x <范本样式>]
   
   # 打包 hombin 目录下 所有文件及文件夹
   zip -q -r hombin.zip /home/hombin
   # 从压缩文件中删除 test.txt
   zip -dv hombin.zip test.txt
   
   
   -d 从压缩文件内删除指定的文件。
   -f 更新现有的文件。
   -q 不显示指令执行过程。
   -r 递归处理，将指定目录下的所有文件和子目录一并处理。
   -t <日期时间> 把压缩文件的日期设成指定的日期。
   -v 显示指令执行过程或显示版本信息。
   -z 替压缩文件加上注释。
   
   # unzip 用于解压缩zip文件
   unzip [-cflptuvz][-agCjLMnoqsVX][-P <密码>][.zip文件][文件][-d <目录>][-x <文件>] 或 unzip [-Z]
   # 解压 abc.zip
   unzip abc.zip
   
   -l 显示压缩文件内所包含的文件。
   -M 将输出结果送到more程序处理。
   -o 不必先询问用户，unzip执行后覆盖原有文件。
   -P <密码> 使用zip的密码选项。
   -z 仅显示压缩文件的备注文字。
   -Z 等价于执行zipinfo指令。列出压缩文件信息。
   ```



#### 目录命令

1. pwd

   ```shell
   # pwd 显示当前工作目录的路径
   pwd
   
   -L 打印逻辑上的工作目录（默认是 -L）
   -P 打印物理上的工作目录（可用于指向软链接的实际目录）
   ```

2. ls

   ```shell
   # ls [选项] [目录或者文件]
   ls -a -l
   ls -alh
   
   -a 显示所有文件及目录 (. 开头的隐藏文件也会列出)
   -l 除文件名称外，亦将文件型态、权限、拥有者、文件大小等资讯详细列出
   -r 将文件以相反次序显示(原定依英文字母次序)
   -t 将文件依建立时间之先后次序列出
   -A 同 -a ，但不列出 "." (目前目录) 及 ".." (父目录)
   -F 在列出的文件名称后加一符号；例如可执行档则加 "*", 目录则加 "/"
   -R 若目录下有文件，则以下之文件亦皆依序列出
   ```

3. cd

   ```shell
   # cd [选项] 或者 [目录位置] 切换到指定目录
   cd
   cd ~
   cd ../..
   cd /home/hombin
   ```

4. tree

   ```shell
   # tree 列出指定目录下的所有文件，包括子目录里的文件
   tree [-aACdDfFgilnNpqstux][-I <范本样式>][-P <范本样式>][目录...]
   
   -a 显示所有文件和目录。
   -A 使用ASNI绘图字符显示树状图而非以ASCII字符组合。
   -C 在文件和目录清单加上色彩，便于区分各种类型。
   -d 显示目录名称而非内容。
   -D 列出文件或目录的更改时间。
   -f 在每个文件或目录之前，显示完整的相对路径名称。
   -F 在执行文件，目录，Socket，符号连接，管道名称名称，各自加上"*","/","=","@","|"号。
   -g 列出文件或目录的所属群组名称，没有对应的名称时，则显示群组识别码。
   -i 不以阶梯状列出文件或目录名称。
   -L level 限制目录显示层级。
   -l 如遇到性质为符号连接的目录，直接列出该连接所指向的原始目录。
   -n 不在文件和目录清单加上色彩。
   -N 直接列出文件和目录名称，包括控制字符。
   -p 列出权限标示。
   -P <范本样式> 只显示符合范本样式的文件或目录名称。
   -q 用"?"号取代控制字符，列出文件和目录名称。
   -s 列出文件或目录大小。
   -t 用文件和目录的更改时间排序。
   -u 列出文件或目录的拥有者名称，没有对应的名称时，则显示用户识别码。
   -x 将范围局限在现行的文件系统中，若指定目录下的某些子目录，其存放于另一个文件系统上，则将该子目录予以排除在寻找范围外
   ```

   

#### 磁盘命令

1. du

   ```shell
   # du 用于显示目录或文件的大小
   du [-abcDhHklmsSx][-L <符号连接>][-X <文件>][--block-size][--exclude=<目录或文件>][--max-depth=<目录层数>]
   # 可读方式查看 test 目录所占空间情况
   du -h test
   
   -a 或-all 显示目录中个别文件的大小。
   -b 或-bytes 显示目录或文件大小时，以byte为单位。
   -c 或--total 除了显示个别目录或文件的大小外，同时也显示所有目录或文件的总和。
   -D 或--dereference-args 显示指定符号连接的源文件大小。
   -h 或--human-readable 以K，M，G为单位，提高信息的可读性。
   -H 或--si 与-h参数相同，但是K，M，G是以1000为换算单位。
   -k 或--kilobytes 以1024 bytes为单位。
   -l 或--count-links 重复计算硬件连接的文件。
   -L <符号连接>或--dereference<符号连接> 显示选项中所指定符号连接的源文件大小。
   -m 或--megabytes 以1MB为单位。
   -s 或--summarize 仅显示总计。
   -S 或--separate-dirs 显示个别目录的大小时，并不含其子目录的大小。
   -x 或--one-file-xystem 以一开始处理时的文件系统为准，若遇上其它不同的文件系统目录则略过。
   -X <文件>或--exclude-from=<文件> 在<文件>指定目录或文件。
   --exclude=<目录或文件> 略过指定的目录或文件。
   --max-depth=<目录层数> 超过指定层数的目录后，予以忽略。
   ```

2. df

   ```shell
   # df 用于显示目前在 Linux 系统上的文件系统磁盘使用情况统计
   df [选项] [文件]
   -a, --all 包含所有的具有 0 Blocks 的文件系统
   -h, --human-readable 可读格式
   -l, --local 限制列出的文件结构
   -t, --type=TYPE 限制列出文件系统的 TYPE
   -T, --print-type 显示文件系统的形式
   -x, --exclude-type=TYPE 限制列出文件系统不要显示 TYPE
   ```

3. dd

   ```shell
   # dd 可从标准输入或文件中读取数据，根据指定的格式来转换数据，再输出到文件、设备或标准输出
   
   # 在 Linux 下制作启动盘
   dd if=boot.img of=/dev/fd0 bs=1440k 
   # 将 testfile_2 文件中的所有英文字母转换为大写，然后转成为 testfile_1 文件
   dd if=testfile_2 of=testfile_1 conv=ucase 
   
   if=文件名			输入文件名，默认为标准输入。即指定源文件。
   of=文件名			输出文件名，默认为标准输出。即指定目的文件。
   ibs=bytes			一次读入bytes个字节，即指定一个块大小为bytes个字节。
   obs=bytes			一次输出bytes个字节，即指定一个块大小为bytes个字节。
   bs=bytes			同时设置读入/输出的块大小为bytes个字节。
   cbs=bytes			一次转换bytes个字节，即指定转换缓冲区大小。
   skip=blocks		从输入文件开头跳过blocks个块后再开始复制。
   seek=blocks		从输出文件开头跳过blocks个块后再开始复制。
   count=blocks	仅拷贝blocks个块，块大小等于ibs指定的字节数。
   conv=<关键字>	关键字有11种
   	conversion：用指定的参数转换文件。
     ascii：转换ebcdic为ascii
     ebcdic：转换ascii为ebcdic
     ibm：转换ascii为alternate ebcdic
     block：把每一行转换为长度为cbs，不足部分用空格填充
     unblock：使每一行的长度都为cbs，不足部分用空格填充
     lcase：把大写字符转换为小写字符
     ucase：把小写字符转换为大写字符
     swap：交换输入的每对字节
     noerror：出错时不停止
     notrunc：不截短输出文件
     sync：将每个输入块填充到ibs个字节，不足部分用空（NUL）字符补齐。
   ```



#### 网络命令

1. ping

   ```shell
   # ping 用于检测远端主机网络
   ping [-dfnqrRv][-c<完成次数>][-i<间隔秒数>][-I<网络界面>][-l<前置载入>][-p<范本样式>][-s<数据包大小>][-t<存活数值>][主机名称或IP地址]
   
   -d 使用Socket的SO_DEBUG功能。
   -c <完成次数> 设置完成要求回应的次数。
   -t <存活数值> 设置存活数值TTL的大小。
   ```

2. telnet

   ```shell
   # telnet 用于远端登入，可用于服务或者容器间访问测试
   telnet [-8acdEfFKLrx][-b<主机别名>][-e<脱离字符>][-k<域名>][-l<用户名称>][-n<记录文件>][-S<服务类型>][-X<认证形态>][主机名称或IP地址<通信端口>]
   
   telnet 192.168.0.5 8090
   
   -a 尝试自动登入远端系统。
   -b <主机别名> 使用别名指定远端主机名称。
   -c 不读取用户专属目录里的.telnetrc文件。
   -d 启动排错模式。
   -l <用户名称> 指定要登入远端主机的用户名称。
   -X <认证形态> 关闭指定的认证形态。
   ```

3. netstat

   ```shell
   # netstat 用于显示网络状态
   netstat [-acCeFghilMnNoprstuvVwx][-A<网络类型>][--ip]
   # 显示UDP端口号的使用情况
   netstat -apu
   
   -a 或 --all 显示所有连线中的Socket。
   -i 或 --interfaces 显示网络界面信息表单。网卡列表。
   -l 或 --listening 显示监控中的服务器的Socket。
   -n 或 --numeric 直接使用IP地址，而不通过域名服务器。
   -s 或 --statistics 显示网络工作信息统计表。
   -t 或 --tcp 显示TCP传输协议的连线状况。
   -u 或 --udp 显示UDP传输协议的连线状况。
   ```



#### 系统命令

1. alias

   ```shell
   # alias 用于设置指令的别名。
   alias [别名]=[指令名称]
   alias la='ls -al'
   ```

   

2. export

   ```shell
   # export 用于设置或显示环境变量。
   export [-fnp][变量名称]=[变量设置值]
   
   -f 　代表[变量名称]中为函数名称。
   -n 　删除指定的变量。变量实际上并未删除，只是不会输出到后续指令的执行环境中。
   -p 　列出所有的shell赋予程序的环境变量。
   ```

3. date

   ```shell
   # date 用来显示或设定系统的日期与时间
   date [-u] [-d datestr] [-s datestr] [--utc] [--universal] [--date=datestr] [--set=datestr] [--help] [--version] [+FORMAT] [MMDDhhmm[[CC]YY][.ss]]
   
   -d datestr : 显示 datestr 中所设定的时间 (非系统时间)
   -s datestr : 将系统时间设为 datestr 中所设定的时间
   -u : 显示目前的格林威治时间
   
   # date
   三 5月 12 14:08:12 CST 2010
   # date '+%c' 
   2010年05月12日 星期三 14时09分02秒
   # date '+%D' //显示完整的时间
   05/12/10
   # date '+%x' //显示数字日期，年份两位数表示
   2010年05月12日
   # date '+%T' //显示日期，年份用四位数表示
   14:09:31
   # date '+%X' //显示24小时的格式
   14时09分39秒
   
   %  : 印出 %
   %n : 下一行
   %t : 跳格
   %H : 小时(00..23)
   %I : 小时(01..12)
   %k : 小时(0..23)
   %l : 小时(1..12)
   %M : 分钟(00..59)
   %p : 显示本地 AM 或 PM
   %r : 直接显示时间 (12 小时制，格式为 hh:mm:ss [AP]M)
   %s : 从 1970 年 1 月 1 日 00:00:00 UTC 到目前为止的秒数
   %S : 秒(00..61)
   %T : 直接显示时间 (24 小时制)
   %X : 相当于 %H:%M:%S
   %Z : 显示时区
   
   # 设定时间
   date --date '12:34:56'
   ```





## Linux 实操

### 权限管理

#### 组管理

1. 在 Linux 系统中 每个用户必须属于一个组，不能独立于组外。在 Linux 中每个文件有 所有者，所在组，其它组 的概念。

2. 修改 文件 / 目录 所在的组

   ```shell
   # 指令
   chgrp [组名] [文件名]
   
   # 例子（ root 组：用户 root ）
   groupadd fruit
   touch apple.txt
   chgrp fruit apple.txt
   ```

3. 其它组：除了文件所有者和所在组的用户，系统的其它用户都是文件的其他组

4. 改变用户的所在组

   ```shell
   # 更换用户所在组
   usermod -g [新的组名] [用户名]
   
   # 改变用户登录的初始目录 （需要用户有新目录的权限）
   usermod -d [目录名] [用户名]
   ```

#### 权限控制

1. ls -l 文件权限说明

   ```shell
   -rwxrw-r-- 1 root root 1614 Jul 29 17:29 conn_eip_clickhouse.py
   drwxr-xr-x 4 root root 4096 Aug 26 16:40 tiny_sh
   
   # -rwxrw-r-- [0-9]位说明：
   	第[0]位 确定文件类型
   		- 无，即默认文件类型
   		l 链接，相当于 windows 的快捷方式
   		d 目录，相当于 windows 的文件夹
   		c 字符设备文件，比如 鼠标、键盘
   		b 块设备，比如 硬盘
   		
   	第[1-3]位 确定 User 文件所有者 所拥有的文件权限 
   	第[4-6]位 确定 Group 所属组（用户组）所拥有的文件权限
   	第[7-9]位 确定 Other 其它用户 所拥有的文件权限
   
   # rwx 权限 rwx=4+2+1=7
   - r：read：r=4：文件可读：可查看目录
   - w：write：w=2：文件可写：可修改目录内容，创建删除等操作
   - x：execute：x=1：文件可执行：可进入目录
   
   # 1 是文件硬链接数 或 链接占用的节点数
   如果是一个文件，此时这一字段表示这个文件所具有的硬链接数
   如果是一个目录，则字段表示链接占用的节点数，且字段值最少是2，每个目录包含指向本身的子目录. 和指向上级目录的..
   此外，每新建一个子目录，该目录第2字段就递增 1，但是新建 文件 不会增加。
   
   # root 第3字段：文件（目录）所有者
   表示此文件/目录属于哪个用户，对于一个目录来说，只有拥有该目录的用户，或者具有写权限的用户才有在目录下创建文件的权利。
   
   # root 第4字段 文件（目录）拥有者所在的组
   一个用户可以加入很多个组，但是其中有一个是主组，就是显示在第4字段的组名。
   
   # 1614 文件所占用的空间(以字节为单位)
   第5字段表示文件大小，如果是一个文件夹（目录），则表示该文件夹的大小。但不是文件夹以及它下面的文件的总大小。
   
   # Jul 29 17:29 文件（目录）最近访问（修改）时间
   文件创建的时间可以通过touch命令来修改。
   
   # conn_eip_clickhouse.py 文件名 目录名
   如果是一个符号链接，那么会有一个 [->] 箭头符号，后面说明它指向的文件或目录
   ```

2. 修改权限

   ```shell
   # chmod 
   指令 chmod 可修改 文件 或者 目录 的权限
   
   # 方式一：+ - = （增减 或 赋予权限）
   u：所有者 g：所有组 o：其他人 a：所有人（a=u+g+o）
   chmod u=rwx,g=rx,o=x [文件名]
   chmod o+w [文件名]
   chmod a-x [文件名]
   
   # 方式二：通过数字 1234567 变更权限
   chmod 751 [文件名] 等价于 chmod u=rwx,g=rx,o=x
   ```

3. 修改所有者

   ```shell
   # 改变所有者
   chown [新的所有者] [文件名]
   
   # 改变所有者和所在组
   chown [新的所有者]:[新的所在组] [文件名/目录]
   
   # -r 目录下所有子目录 子文件生效
   chown [新的所有者]:[新的所在组] -r [文件名/目录]
   ```

4. 修改所在组

   ```shell
   # 改变所在组
   chgrp [新的所在组] [文件名/目录]
   
   --reference=<参考文件或目录> 　把指定文件或目录的所属群组全部设成和参考文件或目录的所属群组相同。
   chgrp --reference=log2020.log log2021.log
   ```



### 定时任务

#### crontab

1. crontab 用于设置定时任务，通过  `cat /etc/crontab`  可查看 crontab 示例

   ```shell
   $ cat /etc/crontab 
   SHELL=/bin/bash
   PATH=/sbin:/bin:/usr/sbin:/usr/bin
   MAILTO=root
   
   # For details see man 4 crontabs
   
   # Example of job definition:
   # .---------------- minute (0 - 59)
   # |  .------------- hour (0 - 23)
   # |  |  .---------- day of month (1 - 31)
   # |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
   # |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
   # |  |  |  |  |
   # *  *  *  *  * user-name  command to be executed
   ```

2. 执行身份

   ```shell
   # 以什么身份运行脚本，可通过 crontab 的 -u 参数指定
   sudo crontab -u [用户名] -e
   
   # 在 /etc/crontab 中编辑指定 执行用户
   1 1 * * * user001 user001 /data/home/user001/test.sh
   ```

3. job 的管理

   ```shell
   crontab -e: 编辑或创建 job，配合 -u 可操作指定用户的 job
   crontab -l: 列出 job，配合 -u 参数可查看指定用户的 job
   crontab -r: 删除 job 文件，慎用，没有确认过程直接就删了
   crontab -i: 同 -r，但删除前会先确认
   ```

4. 循坏周期

   ```shell
   30 5 11 12 * echo "hello"
   
   5 个 * 的位置分别代表了不同时间单位，由左至右依次为，
   
   分，取值范围 0 ~ 59
   时，0 ~23
   天，1 ~ 31
   月，1 ~ 12，部分实现支持使用名称 jan, feb, mar …
   周，0 ~ 6，其中星期天为 0，部分实际支持使用名称，sun, mon, tue …
   ```

5. 非标准字符串

   | 关键字                 | 描述         | 等价于    |
   | ---------------------- | ------------ | --------- |
   | @yearly (or @annually) | 每年一月午夜 | 0 0 1 1 * |
   | @monthly               | 每月首日午夜 | 0 0 1 * * |
   | @weekly                | 每周天午夜   | 0 0 * * 0 |
   | @daily (or @midnight)  | 每天午夜     | 0 0 * * * |
   | @hourly                | 每小时       | 0 * * * * |
   | @reboot                | 重启时       | N/A       |

   ```shell
   # 重启后启动 redis
   @reboot /usr/local/bin/redis-server /path/to/redis.conf
   ```

6. 应用实例

   ```shell
   # 每隔1分钟，将当期日期信息，追加到 /tmp/mydate 文件中
   */1 * * * * date >> /tmp/mydate
   
   # 每天凌晨2点钟 将 mysql 数据库 testdb 备份到文件中
   0 2 * * * mysqldump -u root -proot testdb > /home/hombin/test/db.bak
   ```

   

#### at

1. 在 linux 中 crontab 用于处理周期性的任务而 at 则是处理仅执行一次的任务（用于指定时间执行命令）

2. at 服务

   ```shell
   # 查看是否安装 at
   whereis at
   
   # yum 查询及安装 at
   yum provides at
   yum -y install [指定版本的at安装包]
   
   # 启动 atd
   service atd start
   # 查看 atd
   service atd status
   ```

3. 访问控制

   ```shell
   时间规范的确切定义可以在 /usr/share/doc/at-3.1.10/timespec 中查看
   root 用户可以在任何情况下使用at命令，而其他用户使用at命令的权限定义在 /etc/at.allow 和 /etc/at.deny 文件中
   如果 /etc/at.allow 文件存在，只有在该文件中的用户名对应的用户才能使用at
   如果 /etc/at.allow 文件不存在，/etc/at.deny 存在，所有不在 /etc/at.deny 文件中的用户可以使用 at
   如果 /etc/at.allow 和 /etc/at.deny 文件都不存在，只有 root 用户能使用 at
   一个空内容的 /etc/at.deny 表示任何用户都能使用 at 命令，这是默认的配置
   ```

4. at 命令

   ```shell
   # 在特定的时间执行一次性的任务
   at
   # 列出用户的计划任务，如果是超级用户将列出所有用户的任务，结果的输出格式为：Job number, date, hour, queue, and username
   atq
   # 根据 Job number 删除 at 序号为 7 的任务
   atrm 7
   # 在系统负荷允许的情况下执行 at 任务，换言之，就是在系统空闲的情况下才执行 at 任务
   batch
   
   -m：当指定的任务被完成之后，将给用户发送邮件，即使没有标准输出
   -M：不发送邮件
   -l：atq的别名
   -d：atrm的别名
   -r：atrm的别名
   -v：显示任务将被执行的时间,显示的时间格式为：Thu Feb 20 14:50:00 1997
   -c：打印任务的内容到标准输出
   -V：显示版本信息
   -q：后面加<队列> 使用指定的队列
   -f：后面加<文件> 从指定文件读入任务而不是从标准输入读入
   -t：后面<时间参数> 以时间参数的形式提交要运行的任务
   
   CC 指定年份的前两位数字
   YY 指定年份的后两位数字
   MM 指定一年的哪一月（从 01 到 12）
   DD 指定一月的哪一天（从 01 到 31）
   hh 指定一天中的哪一小时（从 00 到 23）
   mm 指定一小时的哪一分钟（从 00 到 59）
   ss 指定一分钟的哪一秒（从 00 到 59）
   ```

5. 应用实例

   ```shell
   # 三天后的下午 5 点执行 /bin/ls
   [yahobin@localhost ~]$ at 5pm+3 days
   at> /bin/ls
   at> <EOT>
   job 7 at 2013-01-08 17:00
   
   # 明天 17 点钟，输出时间到指定文件内
   [yahobin@localhost ~]$ at 17:20 tomorrow
   at> date >/root/2021.log         
   at> <EOT>
   job 8 at 2021-08-16 17:20
   ```



### 磁盘分区 & 挂载

#### 分区

1. 查看 分区信息

    ```shell
    fdisk -l
    ```

    ```shell
    fdisk [必要参数][选择参数]
    
    # 必要参数
    -l 列出素所有分区表
    -u 与 -l 搭配使用，显示分区数目
    
    # 选择参数
    -s<分区编号> 指定分区
    -v 版本信息
    
    # 菜单操作说明
    m ：显示菜单和帮助信息
    a ：活动分区标记/引导分区
    d ：删除分区
    l ：显示分区类型
    n ：新建分区
    p ：显示分区信息
    q ：退出不保存
    t ：设置分区号
    v ：进行分区检查
    w ：保存修改
    x ：扩展应用，高级功能
    ```

2. 分区界面 操作（初始化）

    ```shell
    fdisk /dev/vdb
    
    # m打印菜单
    # n新建分区
    # 		p选择主分区
    # 		e选择扩展分区
    #        1选择分区号
    #        		选择初始位置，默认为1
    #        		选择结束为止，默认为磁盘结尾
    ```

3. 格式化分区

    ```shell
    # xfs 文件系统
    mkfs.xfs /dev/vdb1
    # ext4 文件系统
    mkfs.ext4 /dev/vdb1
    ```

4. 查看可用 分区

    ```shell
    blkid
    ```

    

#### 挂载

1. 挂载

    ```shell
    mount [-hV]
    mount -a [-fFnrsvw] [-t vfstype]
    mount [-fnrsvw] [-o options [,...]] device | dir
    mount [-fnrsvw] [-t vfstype] [-o options] device dir
    
    -V：显示程序版本
    -h：显示辅助讯息
    -v：显示较讯息，通常和 -f 用来除错。
    -a：将 /etc/fstab 中定义的所有档案系统挂上。
    -F：这个命令通常和 -a 一起使用，为每一个 mount 的动作产生一个行程负责执行。在系统需要挂上大量 NFS 档案系统时加快挂上的动作。
    -f：通常用在除错的用途。它会使 mount 并不执行实际挂上的动作，而是模拟整个挂上的过程。通常会和 -v 一起使用。
    -n：一般而言，mount 在挂上后会在 /etc/mtab 中写入。但在系统中没有可写入档案系统存在的情况下可以用这个选项取消这个动作。
    -s-r：等于 -o ro
    -w：等于 -o rw
    -L：将含有特定标签的硬盘分割挂上。
    -U：将档案分割序号为 的档案系统挂下。-L 和 -U 必须在/proc/partition 这种档案存在时才有意义。
    -t：指定档案系统的型态，通常不必指定。mount 会自动选择正确的型态。
    -o async：打开非同步模式，所有的档案读写动作都会用非同步模式执行。
    -o sync：在同步模式下执行。
    -o atime、-o noatime：当 atime 打开时，系统会在每次读取档案时更新档案的『上一次调用时间』
    -o auto、-o noauto：打开/关闭自动挂上模式。
    -o defaults:使用预设的选项 rw, suid, dev, exec, auto, nouser, and async.
    -o dev、-o nodev-o exec、-o noexec允许执行档被执行。
    -o suid、-o nosuid：允许执行档在 root 权限下执行。
    -o user、-o nouser：使用者可以执行 mount/umount 的动作。
    -o remount：将一个已经挂下的档案系统重新用不同的方式挂上。例如原先是唯读的系统，现在用可读写的模式重新挂上。
    -o ro：用唯读模式挂上。
    -o rw：用可读写模式挂上。
    -o loop=：使用 loop 模式用来将一个档案当成硬盘分割挂上系统。
    ```

    ```shell
    # 挂载
    mount /dev/hda1 /mnt
    # 只读模式 挂载
    mount -o ro /dev/hda1 /mnt
    ```

    

2. 卸载

    ```shell
    umount [-ahnrvV][-t <文件系统类型>][文件系统]
    -a 卸除/etc/mtab中记录的所有文件系统。
    -h 显示帮助。
    -n 卸除时不要将信息存入/etc/mtab文件中。
    -r 若无法成功卸除，则尝试以只读的方式重新挂入文件系统。
    -t<文件系统类型> 仅卸除选项中所指定的文件系统。
    -v 执行时显示详细的信息。
    -V 显示版本信息。
    [文件系统] 除了直接指定文件系统外，也可以用设备名称或挂入点来表示文件系统。
    ```

    ```shell
    # 卸载磁盘设备
    umount /mnt/data1 /dev/hdc
    ```

    

#### 扩容

1. 磁盘分区 扩容

    ```shell
    # 卸载已挂载的需要扩容的磁盘 data
    umount /data
    # 数据盘分区
    fdisk /dev/xvdc
    # 依次输入: 
    # "n"(新建分区) 
    # "p"(新建扩展分区)
    # "1"(使用第1个主分区)
    # 两次回车(使用默认配置)
    # 输入 "wq"(保存分区表)，开始分区。
    
    # 检查扩容后的分区
    fdisk –l
    # 检查分区指数
    e2fsck -f /dev/xvdc1
    # 扩容
    resize2fs /dev/xvdc1
    # 挂载扩容后的分区
    mount /dev/xvdc1 /data
    ```

    

2. 支持 LVM 扩容

    ```shell
    fdisk /dev/sdb
    # 创建物理卷
    pvcreate /dev/sdb
    # 检查物理卷
    pvdisplay
    # 扩展卷组
    vgextend centos /dev/sdb
    # 查看卷组
    vgdisplay
    # 扩展 LVM
    lvextend -l +100%FREE /dev/mapper/centos-root
    # 查看扩容结果
    df -h
    ```



### 网络配置

#### 查看网络

1. netstat 网络状态

    ```shell
    # 查看系统中监听的 tcp 端口，且将进程信息也一并打印
    netstat -ntpl
    # 查看系统中所有 tcp 链接
    netstat -natp
    
    # 参数说明
    -n 不将IP、端口等解析为名称
    -a 拉取所有socket（包括tcp、udp、unix sock等）
    -p 显示连接关联的进程信息（可能需要root权限）
    -t 只显示tcp连接
    -u 只显示udp连接
    -x 只显示unix sock连接
    -l 只显示系统监听端口信息
    ```

    ```shell
    # 查找较多 time_wait  连接
    netstat -n|grep TIME_WAIT|awk '{print $5}'|sort|uniq -c|sort -rn|head -n20
    
    ```

    

2. ss（netstat 升级版，需要 psutil 提供）

    ```shell
    # 查看系统中监听的 tcp 端口，ss 的 -p 会将子进程信息一并打印
    ss -ntpl
    # 列出所有网络连接
    ss -a
    # 查看 TCP Socket
    ss -ta
    # 查看 UDP Socket
    ss -ua
    # 查看 RAW Socket
    ss -wa
    # 查看 UNIX Socket
    ss -xa
    ```

    

3. proc 文件（针对部分容器环境，没有 netstat 或 ss 命令）

    ```shell
    # tcp 连接信息文件
    /proc/net/tcp
    # ipv6 tcp 连接信息文件
    /proc/net/tcp6
    # udp 连接信息文件
    /proc/net/udp
    # ipv6 udp 连接信息文件
    /proc/net/udp6
    # unix sock 连接信息文件
    /proc/net/unix
    ```

    以 80 端口为例查看

    ```shell
    5: 00000000:0050 00000000:0000 0A 
    22: 89428609:9F9B A5428609:0050 08
    26: 89428609:9FA6 A5428609:0050 08
    |      |      |      |      |   |--> connection state（套接字状态）
    |      |      |      |      |------> remote TCP port number（远端端口，主机字节序）
    |      |      |      |-------------> remote IPv4 address（远端IP，网络字节序）
    |      |      |--------------------> local TCP port number（本地端口，主机字节序）
    |      |---------------------------> local IPv4 address（本地IP，网络字节序）
    |----------------------------------> number of entry
    
    # connection state (套接字状态)，不同的数值代表不同的状态，参照如下：
    TCP_ESTABLISHED:1   
    TCP_SYN_SENT:2
    TCP_SYN_RECV:3      
    TCP_FIN_WAIT1:4
    TCP_FIN_WAIT2:5     
    TCP_TIME_WAIT:6
    TCP_CLOSE:7         
    TCP_CLOSE_WAIT:8
    TCP_LAST_ACL:9      
    TCP_LISTEN:10
    TCP_CLOSING:11
    ```

    另：查询系统监听tcp端口连接信息

    ```shell
    awk -F '[ :]+' '{if ($7 = "0A") print $3":"strtonum("0x"$4)"\\t\\t"$5":"strtonum("0x"$6)" "$7}' /proc/net/tcp
    ```

    

4. ifconfig 显示或者设置网络设备（Windows 系统 ipconfig 命令）

    ```shell
    ifconfig [网络设备][down up -allmulti -arp -promisc][add<地址>][del<地址>][<hw<网络设备类型><硬件地址>][io_addr<I/O地址>][irq<IRQ地址>][media<网络媒介类型>][mem_start<内存地址>][metric<数目>][mtu<字节>][netmask<子网掩码>][tunnel<地址>][-broadcast<地址>][-pointopoint<地址>][IP地址]
    
    # 参数说明
    add<地址> 设置网络设备IPv6的IP地址。
    del<地址> 删除网络设备IPv6的IP地址。
    down 关闭指定的网络设备。
    <hw<网络设备类型><硬件地址> 设置网络设备的类型与硬件地址。
    io_addr<I/O地址> 设置网络设备的I/O地址。
    irq<IRQ地址> 设置网络设备的IRQ。
    media<网络媒介类型> 设置网络设备的媒介类型。
    mem_start<内存地址> 设置网络设备在主内存所占用的起始地址。
    metric<数目> 指定在计算数据包的转送次数时，所要加上的数目。
    mtu<字节> 设置网络设备的MTU。
    netmask<子网掩码> 设置网络设备的子网掩码。
    tunnel<地址> 建立IPv4与IPv6之间的隧道通信地址。
    up 启动指定的网络设备。
    -broadcast<地址> 将要送往指定地址的数据包当成广播数据包来处理。
    -pointopoint<地址> 与指定地址的网络设备建立直接连线，此模式具有保密功能。
    -promisc 关闭或启动指定网络设备的promiscuous模式。
    [IP地址] 指定网络设备的IP地址。
    [网络设备] 指定网络设备的名称。
    ```

    ```shell
    # 启动关闭指定网卡
    ifconfig eth0 down
    ifconfig eth0 up
    
    # 给 eth0 网卡配置 IP 地址
    ifconfig eth0 192.168.1.52 
    # 给 eth0 网卡配置 IP 地址,并加上子掩码
    ifconfig eth0 192.168.1.52 netmask 255.255.255.0 
    # 给 eth0 网卡配置 IP 地址,加上子掩码,加上个广播地址
    ifconfig eth0 192.168.1.52 netmask 255.255.255.0 broadcast 192.168.1.255
    ```

5. lsof （系统文件命令，也可用来查看网络信息）

    ```shell
    lsof -i [protocol][@hostname|hostaddr][:service|port]
    ```

    ```shell
    # 显示所有打开的端口
    lsof -i
    # 查看 8080 端口占用的进程
    lsof -i:8080
    # 显示所有打开的端口和 UNIX domain 文件
    lsof -i -U
    # 显示那些进程打开了到 www.winter.com 的 UDP 的 7700 端口的链接
    lsof -i UDP@[url]www.winter.com:7700
    # 查看远程主机连接的套接字
    lsof –i@192.168.31.109
    ```

    

#### 测试网络

1. ping

    ```shell
    ping [选项][参数]
    
    # 参数说明
    -d 使用Socket的SO_DEBUG功能；
    -c <完成次数> 设置完成要求回应的次数；
    -f 极限检测；
    -i <间隔秒数> 指定收发信息的间隔时间；
    -I <网络界面> 使用指定的网络界面送出数据包；
    -l <前置载入> 设置在送出要求信息之前，先行发出的数据包；
    -n 只输出数值；
    -p <范本样式> 设置填满数据包的范本样式；
    -q 不显示指令执行过程，开头和结尾的相关信息除外；
    -r 忽略普通的 Routing Table，直接将数据包送到远端主机上；
    -R 记录路由过程；
    -s <数据包大小> 设置数据包的大小；
    -t <存活数值> 设置存活数值TTL的大小；
    -v 详细显示指令的执行过程。
    ```

    ```shell
    # 测试远程地址连通性
    ping www.github.com
    ```

    

2. telnet

    ```shell
    telnet [选项][参数]
    
    # 参数说明
    -8 允许使用 8 位字符资料，包括输入与输出；
    -a 尝试自动登入远端系统；
    -b <主机别名> 使用别名指定远端主机名称；
    -c 不读取用户专属目录里的.telnetrc 文件；
    -d 启动排错模式；
    -e <脱离字符> 设置脱离字符；
    -E 滤除脱离字符；
    -f 此参数的效果和指定 "-F" 参数相同；
    -F 使用 Kerberos V5 认证时，加上此参数可把本地主机的认证数据上传到远端主机；
    -k <域名> 使用 Kerberos 认证时，加上此参数让远端主机采用指定的领域名，而非该主机的域名；
    -K 不自动登入远端主机；
    -l <用户名称> 指定要登入远端主机的用户名称；
    -L 允许输出8位字符资料；
    -n <记录文件> 指定文件记录相关信息；
    -r 使用类似 rlogin 指令的用户界面；
    -S <服务类型> 设置telnet连线所需的 ip TOS 信息；
    -x 假设主机有支持数据加密的功能，就使用它；
    -X <认证形态> 关闭指定的认证形态。
    ```

    ```shell
    # 连接实例
    telnet 192.168.2.12 8090
    ```

    ```shell
    # 重启 telnet
    service xinetd restart
    # 查看 xinetd 配置参数
    man xinetd.conf
    # 常用 telnet 配置
    service telnet
    {
        disable = no  # 启用
        flags = REUSE  # socket可重用
        socket_type = stream  # 连接方式为TCP
        wait = no  # 为每个请求启动一个进程
        user = root  # 启动服务的用户为root
        server = /usr/sbin/in.telnetd  # 要激活的进程
        log_on_failure += USERID  # 登录失败时记录登录用户名
    }
    ```

    

3. nc

    ```shell
    nc [-hlnruz][-g<网关...>][-G<指向器数目>][-i<延迟秒数>][-o<输出文件>][-p<通信端口>][-s<来源位址>][-v...][-w<超时秒数>][主机名称][通信端口...]
    
    # 参数说明
    -g <网关> 设置路由器跃程通信网关，最多可设置 8 个。
    -G <指向器数目> 设置来源路由指向器，其数值为 4 的倍数。
    -h 帮助。
    -i <延迟秒数> 设置时间间隔，以便传送信息及扫描通信端口。
    -l 使用监听模式，管控传入的资料。
    -n 直接使用 IP 地址，而不通过域名服务器。
    -o <输出文件> 指定文件名称，把往来传输的数据以 16 进制字码倾倒成该文件保存。
    -p <通信端口> 设置本地主机使用的通信端口。
    -r 乱数指定本地与远端主机的通信端口。
    -s <来源位址> 设置本地主机送出数据包的 IP 地址。
    -u 使用 UDP 传输协议。
    -v 显示指令执行过程。
    -w <超时秒数> 设置等待连线的时间。
    -z 使用 0 输入/输出模式，只在扫描通信端口时使用。
    ```

    ```shell
    # 网络层路由监听 8090 端口
    nc -lk 8090
    # 扫描 192.168.21.11 的端口 范围是 1-100
    nc -vz -w2 192.168.21.11 1-100
    # 查看从服务器到目的地的端口 443 是否被防火墙阻止
    nc -vz -w2 acomo.api.crypt.org 443
    ```



#### DNS

1. DNS 与 /etc/hosts 优先级

    ```shell
    DNS缓存 > hosts > DNS服务
    ```

2. 主机信息

    ```shell
    # 查看 hostname
    hostname
    # 临时修改
    hostname test123
    # 查看本机 IP
    hostname -i
    ```

3. 配置文件 /etc/hosts（包含地址和主机名之间的映射，还包括主机名的别名）

    ```shell
    # 修改 hosts 配置
    vim /etc/hosts
    
    # 默认本机信息
    127.0.0.1 localhost.localdomain localhost
    
    # 写入新的映射
    192.168.1.195  debian.localdomain debian
    ```

4. DNS 文件 /etc/resolv.conf （用来指定系统中DNS服务器的IP地址和一些相关信息）

    ```shell
    # vim /etc/resolv.conf
    search abc.com.cn
    nameserver 10.1.6.250
    nameserver 192.168.1.254
    ```

5. Order 文件 /etc/host.conf （决定进行域名解析时查找host文件和DNS服务器的顺序）

    ```shell
    # vim /etc/hosts.conf
    order hosts,bind
    ```

6. Bind 文件 /etc/name.conf （）

    ```shell
    # vim /etc/name.conf
    options {
    listen-on port 53 { 127.0.0.1; };      //设置named服务器监听端口及IP地址
    listen-on-v6 port 53 { ::1; };
    directory       "/var/named";    //设置区域数据库文件的默认存放地址
    dump-file       "/var/named/data/cache_dump.db";
    statistics-file "/var/named/data/named_stats.txt";
    memstatistics-file "/var/named/data/named_mem_stats.txt";
    
    allow-query     { localhost; };   //允许DNS查询客户端
    allow-query-cache { any; };
    };
    logging {
    channel default_debug {
    file "data/named.run";
    severity dynamic;
    };
    };
    view localhost_resolver {
    match-clients      { any; };
    match-destinations { any; };
    recursion yes;                  //设置允许递归查询
    include "/etc/named.rfc1912.zones";
    };
    ```

    



### 进程管理

#### 查看进程

1. ps 命令
2. 



#### 终止进程





#### 进程树

1. 查看进程树

    ```shell
    pstree [选项]
    
    # 参数说明
    -a 显示每个进程的完整指令，包括路径、参数
    -A 使用 ascii 码显示树形
    -c 关闭精简表示法
    -G 使用 VT 100 线条绘制字符
    -h 高亮显示正在执行的程序
    -H 类似 "-h"，但是突出显示指定的进程。与 -h 不同，如果高亮显示不可用，pstree 在使用 -H 时会失败。
    -l 长格式显示
    -n 以进程号排序，默认以名字排序
    -p 显示 pid
    -u 显示用户
    -U 以 utf-8 显示字符
    -V 显示命令版本信息
    -Z 每个 SELinux 的上下文
    ```

2. 实例

    ```shell
    # 显示树形结构
    pstree -a
    init
      ├─NetworkManager --pid-file=/var/run/NetworkManager/NetworkManager.pid
      │   ├─dhclient -d -4 -sf /usr/libexec/nm-dhcp-client.action -pf /var/run/dhclient-eth0.pid ...
      │   └─{NetworkManager}
      ├─VBoxClient --clipboard
      │   └─VBoxClient --clipboard
    
    # 显示进程号
    pstree -p
    init(1)─┬─NetworkManager(6362)─┬─dhclient(6377)
            │                      └─{NetworkManager}(6379)
            ├─VBoxClient(7869)───VBoxClient(7870)───{VBoxClient}(7872)
            ├─VBoxClient(7882)───VBoxClient(7883)
            ├─VBoxClient(7890)───VBoxClient(7891)───{VBoxClient}(7894)
            ├─VBoxClient(7898)───VBoxClient(7899)─┬─{VBoxClient}(7901)
            │                                     └─{VBoxClient}(7903)
            ├─VBoxClient(7306)───VBoxClient(7308)
            ├─VBoxClient(7312)───VBoxClient(7314)───{VBoxClient}(7317)
            ├─VBoxClient(7318)───VBoxClient(7320)─┬─{VBoxClient}(7323)
            │                                     └─{VBoxClient}(7325)
    ```

    



### 包管理

#### RPM



#### YUM





## Linux 系统

### 防火墙

### 日志控制

### 备份与恢复













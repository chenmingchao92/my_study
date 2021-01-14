# linux实战100讲

## 1.帮助命令

- man 帮助
- help 帮助
- info 帮助
- 使用网络资源（搜索引擎和官方文档）

### 1.man

- 是manual的缩写
- man帮助用法- man ls（查看 ls命令的用法 就是 man+命令的名称）
- man也是一条命令 我们可以通过 man man的方式获取man命令的帮助。
- man man 获取到man的帮助命令以后，我们发现左上角的man的后边有一个1  就是 man(1) 这个1就代表是第几个篇章的帮助命令，        
- 我们可以用 man 7 man的方式来确认，一共有几个这样的篇章
- 同理不仅是man man的命令这样，我们使用 man ls的命令的时候，实际上是man 1 ls的缩写，就是默认查看的第一篇章

### 2.引申：man手册八章目录。

1. Commands：用户可以从 shell 运行的命令
2. System calls 必须由内核完成的功能
3. Library calls 大多数libc函数（linux库函数），例如 qsort（3）
4. special files： /dev目录中的文件
5. File formats and conventions：/etc/passwd等人类可读的文件的格式说明
6. games
7. macro packages and conventions：文件系统标准描述，网络协议，ASCII和其他字符集，还有你眼前这份文档以及其他东西等附件和变量
8. System Management commands：
9. 类似 mount(8) 等命令，大部分只能由root执行

 但我们想获取某个关键字（例如passwd）但是我们不知道他是命令，还是文件，还是啥其他的东西的时候，我们可以使用 man -a passwd 这种方式。这样系统会一项一项的给你列出来，所有有关于这个关键字的命令，当你发现第一个不是的时候，就q退出，这时候系统会提示是否查看下一个和这个关键字有关的帮助文档，按照提示走就可以了

### 3.help帮助

什么是内部命令和外部命令：

内部命令：也称shell内嵌命令;

外部命令：存放在一个文件中，使用时需要去文件中查找，这些文件被定义在$PATH

type命令可以查看命令类型，以区别是内部命令还是外部命令

[root@centos7 ~]# type cd

cd is a shell builtin

[root@centos7 ~]# type ls

ls is aliased to `ls --color=auto'

[root@centos7 ~]# type ifconfig

ifconfig is /usr/sbin/ifconfig

可以看到，cd为shell内嵌命令，ls命令为ls --color=auto的别名，ifconfig命令为外部命令在文件/usr/sbin/ifconfig中。

1. 内部命令使用 help帮助： help cd
2. 外部命令使用help帮助 ：ls --help

### 4.info帮助

> info帮助比help更详细，作为help的补充

eg：info ls

## 2.文件管理

> 在linux中一切皆文件

- pwd  显示当前的目录名称。
- cd 更改当前的操作目录
- ls 查看当前目录下的文件 
  - -l 长格式显示文件 ，显示文件详细信
  - #### -a 显示隐藏文件
  - -r 逆序显示（时间逆序）
  - -t 按照时间顺序显示
  - -R 递归显示

### 1.pwd

> 显示当前说咋目录的名称

/ 和/root是不同的目录 /是根目录，/root是root用户的家目录

我们可以用 man pwd来查看pwd的详细帮助命令信息。

### 2.ls

> 查看当前目录下，有哪些文件

 终端提示符如果是#就代表是root用户，如果是$就代表是非root用户

su -root 切换root 用户，su -用户名，切换到那个用户。

ls可以同时查看多个文件或目录，写法是 ：ls  文件或目录名  文件或目录名 ....

例如 ls  /   /home  /usr 就可以同时查看这三个目录

ls -l 列出的格式如下：

| drwxrwxr-x | 5    | potal | potal | 47       | Jun 28 16:33 | a       |
| ---------- | ---- | ----- | ----- | -------- | ------------ | ------- |
| -rw-r--r-- | 1    | potal | potal | 40632103 | Jun 28 17:06 | lib.zip |

*drwxrwxr-x  5 potal potal        47 Jun 28 16:33 a*

*-rw-r--r--  1 potal potal  40632103 Jun 28 17:06 lib.zip*

第一列中最前的一个字符 是文件类型的表示，如果是-标识是一个普通文件，比如图片，class类等，如果是字母d开头，那么表示这是一个目录，后边的是目录或者文件的权限

第二列表示，如果是普通文件，那么默认为1 如果是目录，那么表示这个目录有多少个子目录不包含文件。

第三列表示，这个文件的所属人是谁

第四列表示，这个所属人属于哪个组

第五列表示，文件的大小

第六列表示，文件的创建日期

第七列表示，文件的名称



ls -la  和 ls -l  -a  的结果是一样的，他们都是正确的。

### 3.cd命令

当我们使用cd命令进入了很深的目录结构中，但是不小心 输入了 cd / 进入了根目录，这时候，我们肯定很崩溃，不过 我们 可以用 **cd -**  这个命令来返回到我们上一步输入cd命令时候所在的目录，来返回到我们输入的很深的那层目录当中，

**cd - ** 他可以让我们在两个工作目录之间来回切换。比如我们当前在 /home/test 这时候我们进入了 cd /home/test/dist  ，此时，如果我们输入 cd - 就会回到/home/test,然后当我们再次输入 cd -的时候，就会再次回到 /home/test/dist  这样 我们可以通过输入 cd - 在这里两个工作目录中交替执行。

### 4.文件和目录操作

mkdir ： 建立一个空的目录。 他可以同时建立多个目录。 mkdir  a b c 建立 这三个目录

如果目录已存在，那么久就会创建失败。

mkdir可以创建多级目录：mkdir -p /a/b/c  我们用-p参数就可以了 同时 -p也会忽略到这个目录已存在的报错（只是不会报错，其实文件还是原本已存在的那个文件，并没有将原本已存在的文件覆盖掉，创建新的文件）。

rmdir:删除文件夹 他只能删除空白的目录，目录中有任何的内容（目录和文件），那么就不能删除，我们这个命令用的比较少

我们都用 rm命令  rm -rf 可以删除多个多级目录， 使用方法是 rm -rf  /home/test  /usr/local  可以一次性删除这两个多级目录。

### 5.复制和移动目录

#### 复制

cp是 copy的缩写

cp   源文件 目标文件  例如 cp /home/aaa.txt  /home/bbb.txt  就是复制一份aaa.txt 为bbb.txt，并指定了文件的放置位置。

cp不带参数的使用情况下是只能复制文件，不能复制目录的，如果我们需要复制目录，那么需要加上参数  **-r**   

cp **-r**  /home/test  cp /home/tests

#### 如何创建文件：

touch /home/filea  就是创建一个filea文件。

因为复制会改变文件原有的创建时间

cp  **-p**   复制的时候可以保留被复制文件的原有的时间。

cp **-a**  既可以保留文件的权限，也可以保留文件的所属主和所属组，以及文件的时间，总之他可以让文件的基本信息不发生变化，和原来一样。

#### 移动

**mv命令就是移动的命令**

mv /home/test/aaa.txt  /home/bbb.txt 这样这个文件就发生了移动，同时也可以进行重命名（操作完后a.txt消失），注意他是剪切，而不是复制，原文件不会保留，只会保留新生成的文件

当然如果我们操作的是目录，操作方式和上边相同。

#### 通配符

*：匹配当前目录下所有的文件和目录 mv * 就是移动当前目录下所有的文件和目录

cp file*  /   就会把所有 以file开头的文件复制到 / 中

cp **-v** 可以查看那些文件复制到了那里.

```shell
[potal@localhost tests]$ mv -v test* ../
‘testa’ -> ‘../testa’
‘testb’ -> ‘../testb’
‘testss’ -> ‘../testss’
[potal@localhost tests]$ 

```

这里通配符只能进行移动，不能进行重命名

?: *号是可以匹配多个字符，但是?只能匹配一个字符，例如fileaaa和filea     file? 就只能匹配filea，不能匹配fileaaa ,  但是   *     就是两个都可以匹配。 

### 6.文件查看命令

- cat 文本内容显示到终端（全部内容）
- head  查看文件开头 head -5  a.txt  查看 前五行
- tail 查看文件结尾 tail -3 a.txt 查看后三行信息
- tail -f 如果文件会持续更新的话，用-f命令可以显示同步更新的内容  tail -10f a.xtx 查看后十行，同时如果文件更新了内容，tail中也会实时更新。  
- wc  统计文件内容信息 wc -l  可以查看文件总共有多少行。
- more  可以分行显示  按空格键
- less  可以分页显示

more命令功能：让画面在显示满一页时暂停，此时可按空格健继续显示下一个画面，或按Q键停止显示。

less命令功能：less命令的用法与more命令类似，也可以用来浏览超过一页的文件。所不同的是less命令除了可以按空格键向下显示文件外，还可以利用上下键来卷动文件。当要结束浏览时，只要在less命令的提示符“: ”下按Q键即可。
它们能上相近，只是从浏览习惯和显示方式上有所不同

### 7.打包压缩和解压缩

- 最早的linux备份介质是磁带，使用的命令是tar
- 可以打包后的磁带文件进行压缩存储，压缩的命令是gzip和bzip2
- 经常使用的扩展名是 .tar.gz   .tar.bz2

tar打包命令常用参数

1. c 打包
2. x 解包
3. f指定操作类型为文件
4. z表示用 gzip格式压缩或者解压缩
5. j表示用 bzip2格式压缩或者解压缩



因此 linux的打包压缩实际上是两个命令。

.conf结尾的一般都是linux的配置文件.

参数 **c**（注意tar命令的参数是没有 **-** 的）标识我们要进行打包操作

参数 **f**表示要打包成一个文件

注意这个命令的目标文件在前面，源目录在后边

tar **cf**  目标包（也是文件）的名字   源目录/文件    （他打包出来以后，是一个文件，不是一个目录）

tar cf yasuo test



ls -h 会将文件和目录以最合适的大小的单位显示，如果达到了M就用M的单位，如果达到了G就用G的单位展示。

我们可以用 gzip 和bzip2 进行压缩和解压缩。

但是tar命令已经将 压缩和解压缩的命令合并到里边了。 

tar **czf** 就是打包加压缩  例如

tar czf /tmp/etc-backup.tar.gz etc

用.tar.gz这种双扩展名就是为了表达出这个文件已经进行了打包和压缩，并且 .gz说明了是用 gzip来压缩的。

tar **cjf** /tmp/etc-backup.tar.bz2  etc

这里 bz2就是表示的用的bzip2来进行压缩的。

gz（gzip）的压缩速度快，但是 压缩出来的文件可能稍大一些

bz2（bzip2）的压缩速度稍慢，但是压缩出来的包的体积更小。

下边的命令就是解压命令，注意  解压的时候，被解压的文件要放到前面，解压到的目标目录放到后边。注意    

**-C**参数表示指定这个压缩包解压后放到哪里，这个-C参数后边接目标目录名字

tar xzf  yasuo.tar.gz  **-C** test/testss/

.tbz2 是 .tar.bz2的一个缩写。

.tgz是 .tar.gz的一个缩写

## 3.vim命令

vim是多模式编辑器

- 正常模式
- 插入模式
- 命令模式
- 可视模式

### 1.正常模式

最一开始进入vim的时候，就是正常模式

#### 1.进入插入模式

- i  光标位于你在正常模式下放置的位置。
- I（大写i）进入光标所在行的开头
- a 光标位于正常模式光标所在位置的下一位
- A 光标位于正常模式光标所在位置的结尾
- o 光标来到所在行的下一行（类似于在这一行的行尾敲了回车），同时该行下边的行自动往后移动一行（例如在第三行按了o 那么会在第四行插入空行，同时原本第四行以及以后的内容自动后移一行
- O 同理如小写的o 只不过 是在上一行

#### 2.进入可视模式

按v进入可视模式

#### 3.复制，粘贴，前进，后退，光标移动

- h:向左移动光标
- j:向下移动· 
- k：向上移动
- l（是L）:向右移动

在字符终端，输入上下左右的时候，会产生乱码

- y(copy):复制命令,
  - yy:复制一整行
  - 3yy：复制三行（从上到下）
  - y$:从光标所在位置到行尾巴（包括光标所在位置字符）
- p(paste)：粘贴命令（对于整行复制，从光标所在行的下一行开始粘贴，并且原本下一行的数据自动往后 移动，比如你的光标在第三行，你要粘贴三行，那么原本第四行的内容会自动往后移动三行，然后在第四行开始粘贴。）（对于y$命令，他会在光标所在行就开始粘贴，并且光标在粘贴前所在位置的字母以及后续的字符，会自动后移放到粘贴后的内容的尾部）
  - p粘贴， 多次粘贴多次按p即可
- d:剪切命令
  - dd 剪切一整行
  - d$:剪切光标所在位置到行尾包括光标所在位置字符）
- u：撤销命令，每按一次u键，就在撤销一次
- ctrl+r:前进

#### 3.其他命令

- x：单个字符的删除命令，删除光标所在位置的单个字符
- r: 替换光标所在位置的字符，先按r键，再输入要替换的字符，即可
- G：移动到指定的行（在命令模式输入 set nu  可以显示行号） 100G 移动到一百行，如果直接输入G  那么会到行尾
- g:移动的行首
- ^：来到一行的开头
- $:来到一行的结尾

### 2.命令模式

主要进行保存，退出，查找，替换等相关操作

输入：即可进入我们的命令行模式

- w:保存命令，如果是对文件进行的修改，那么直接*w* 就是对修改的进行保存，如果是新建的一个文件，那么输入 *w   保存的路径和文件名* 保存这个新文件  *例如 w  /root/a.txt 就是保存在root目录下的a.txt这个文件中。*如果是修改一个文件，但是依然适用了w   保存的路径和文件名    这种方式进行保存，那么就是*另存为*了

- q:不进行保存，直接退出

- !(感叹号):可以在我们打开vim的同时 临时执行一个命令，例如不记得我们建立的这个文件的位置是哪里了，我们就可以用     !pwd 来查看我们当前的路径

- /:查找命令   */x*  就是查找字符x ，如果有多个匹配的字符x那么 我们就可以按n键来向下遍历，按N时向上遍历

- 单个替换指令 s/old/new  old指的就是要被替换掉的字符（应该叫字符串），new就是我们新的要放上去的字符，但是这里命令只会对我们的光标所在行的第一个匹配的字符进行替换。

- 每一行第一个匹配替换命令%s/old/new:  他会对全文的每一行的第一个匹配的内容进行替换。

- 全文匹配替换命令 %s/old/new/g :这样才会对全文进行匹配替换

- 对指定行进行替换 ：如果是对单个行进行替换 只需要 行号+s/old/new  那么就对这一行的第一个匹配的内容进行替换，如果对这一行的全部匹配的内容进行替换，就用  行号+s/old/new/g 即可 ，例如对第五行的内容进行替换 输入 5s/x/#/g  即可

- 对多行进行替换：如果指出都要替换那些行， 用行号,行号来进行 3,6s/x/# 就是替换第三行到第六行的第一个匹配的x为#  全部替换的话命令是：3,6s/x/#/g

- set nu 显示行号

- set nonu 不显示行号-----------set命令只能对单次修改生效，退出文件，再打开就是失效了，就是他是一个临时的命令

  如果要想让配置永久生效，那么我们需要修改vim的配置文件，  vim /etc/vimrc   我们对这个文件进行修改，我们vim的配置就永久的发生了变化。我们只需要在这个文件的尾部，另起一行，输入 set  nu  就可以了

### 3.可视模式

> 对文件进行重复的大量工作，一次性执行完成

v，V(shift+v)，ctrl+v可以进入可视模式,

- v  进入可视模式，就像我们在window中进行选中一样，通过键盘移动光标，他会把你光标走过的字符都进行高亮标记，像左移动光标，就会一个一个字符的高亮标志，如果右边的字符是已经被选择的，那么向右就是将高亮标记的光标还原，向下就是直接高亮标记一整行，向上相反
- shift+v 可以直接高亮标记一整行，并且左右键不在起作用，只能一整行一整行的高亮标记（向下按键）或者一整行一整行的取消高亮（向上按键）
- ctrl+v 就是高亮标记一整块，就像我们的矩阵数据一样，例如当前光标在第三行第五个字符，那么我们先向右移动两个字符，在向下移动3行，那么行为5-7，列为3-5之间的所有字符都被高亮选中  ，就是选择你光标走过的块区域，我们选择完内容后，我们按 shift+i（或大写的I）  输入我们要插入的字符 ，然后在按两下esc  那么每一行被我们选中的区域的前面都会被插入相同的内容。如果我们在选中一个块区域以后，按d键，就可以剪切掉我们所有选中的位置的数据了
- 在可视模式下 按c可以删除掉高亮的文本，然后进入编辑模式



## 4.用户和用户管理

### 1.用户

- useradd 新建用户   useradd  用户名称   （例如 useradd cmc  创建一个叫cmc的用户）
- 通过id命令可以查看当前用户是否存在  （id cmc 就可以查看cmc这个用户是否存在）

用户都有自己的家目录  root的家目录是 /root

cmc的家目录在/home/cmc这里

我们的用户会被记录到 /etc/passwd这个文件当中，同时 用户密码相关的文件会被记录到/etc/shadow中

我们通过id查询账号是否存在的时候，如果用户存在，那么可以看到三个属性值

- uid  用户创建成功以后，系统为改用户分配的唯一标识，root用户的id是0，如果我们把普通用户的id改为0，那么系统就会把普通用户按照root用户对待
-  groups 就是用户所属的组。组是用来对资源访问进行控制，如果我们创建一个用户的时候，没有指定用户所属组，那么系统会创建一个和账号名称一样的组名称
- gid：就是用户所属的组id。组是用来对资源访问进行控制，如果我们创建一个用户的时候，没有指定用户所属组的id，那么系统会创建一个和账号id一样的组id值

passwd命令：为用户设置密码，passwd cmc就是为cmc账号设置密码，就是他提示你密码过于简单，但是还是会更新成功， 如果我们直接输入passwd 那么就是更改当前用户的密码

userdel ：删除用户  userdel  cmc  就删除掉了 cmc这个账号，但是此时cmc的家目录还是保留的（此时这个家目录变成了没有主人的家目录，除了root以外，别人是无法访问的），如果我们同样不在需要cmc的家目录，那么我们可以用 userdel  -r  cmc 这个命令来进行操作。

usermod：修改用户账号相应的信息：usermod -d  可以修改用户家目录的位置， usermod -d /home/ccc  cmc 就是修改cmc的用户家目录的位置为ccc文件夹，

usermod -g cmcGroup cmc，这就将cmc的用户组又cmc改成了cmcGroup

chage:对用户的生命周期进行修改，例如用户密码的过期时间，修改时间，用户账号的过期时间



### 2.用户组

groupadd ：建立用户组  groupadd  cmcGroup  建立一个cmcGroup的用户组

useradd -g cmcGroup user 这个命令就是新建user这个账号的同时，指定他的组为cmcGroup

groupdel：删除用户组



### 3.切换用户

su - cmc 这个命令就是 从root用户切换到cmc这个普通用户(因为root切换到普通用户不需要输入密码)，**-** 的意思就是同时环境（就是我们当前所在的位置，比如原本我们在root的家目录，如果我们加了- 我们就进入了 home目录，如果不加，那么我们就还在root的家目录下）也同时跟着发生改变

root 用户切换到普通用户是不需要输入密码的，  普通用户切换到普通用户或者root用户都是需要输入密码的

## 5.文件和目录权限

文件类型：

- **-**  普通文件
- d 目录文件 
- b 块特殊文件  比如插入一个移动硬盘 就是一个块特殊设备
- c 字符特殊文件  类似于我们的终端（shell中会讲解）
- l 符号链接 类似于window中的快捷方式   后边会将
- f 命名管道
- s 套接字文件

普通文件的权限：

- r 可读   对应数字 4
- w 可写   对应数字 2 
- x 可执行  对应数字 1

<!--vim的写入并不是真的最一开始就往你编辑的那个文件中进行写，他会往一个.开头的临时隐藏文件中先写入，然后的你保存的时候，在写入到你真正想要编辑的那个文件中-->

创建新文件有默认权限，根据umask值计算，属主和属组根据当前进程的用户来设定



目录权限的表示方法

- x   表示可以进入到目录
- rx  显示目录内的文件名（这其实是r的权限，不过想要查看需要有x的权限）
- wx  增删改目录内的文件（同理如上）



chmod修改文件或目录的权限

- chmodu+x /tmp/testfile  例如(chmod u+x  test  给test文件的所属主增加x的权限)
- chmod755/tmp/testfile

chown 修改文件的属组和属主 chown  用户名 文件名  修改文件的属主（例如 chown potal /test 修改test目录的属主） 用chown :组名 文件名  修改文件的属组（例如 chown  :potal /test 修改test的属组为potal)



+好表示增加权限，-号表示减少权限，=标识赋予权限（会把之前的权限抹除掉，然后赋上=后边赋予的新权限）

chmod中  如果用 u+/(- =)   命令 表示修改所属主的权限，g+/(- =)   命令标识修改所属组的权限，o+/(- =)   命令标识修改其他用户的权限，a+/(- =)   命令标识修改在该文件或目录 的所有权限

- chmod u=rwx   文件名  给主账号rwx的权限
- chmod u+x  文件名   给所属主增加x的权限
- chmod g-r 文件名    给所属组减少r的权限
- chmod a+r 文件名 给所有该文件账号增加r权限

chgrp：可一旦出修改属组，不常用



<!--ls -ld /目录名  只查看这个目录的详情，不查看这个目录里边的东西-->

<!--root不受权限的限制，权限只能限制非root用户-->

<!--ctrl+r 可以搜索我们之前执行的过得命令，只需要 输入部分命令字符，他会自动匹配-->

用数字来更改权限，我们知道r用4表示，w用2表示，x用1表示

所以 如果所属主拥有rwx权限 那么他的数字就是7（4+2+1） 同理所属组和其他账号

如果我们想要该文件的所有账号（就是u,g,o）都拥有rwx的权限 我们需要些为 chmod  777  文件名

第一个7表示所属主的rwx权限（4+2+1) 第二个7表示所属组的rwx权限(4+2+1)，同理另一个

所以我们在用数字给文件用户赋予权限的时候，一个数字代表所属组的权限，以此类推  

chmod  644 文件名  644表示：rw-r--r-- 

创建一个默认的普通文件（不是目录）的时候 他的默认权限时666 ，目录的默认权限时777，但是他还要减去 umask的值，才是真正的默认权限

echo命令是显示我们的信息，直接用echo 123  会在终端打印一个123，echo  123 > a.txt  就是把123输入到a.txt中  > 号这个是 输出重定向符号，如果只是一个>会吧文件中的所有内容都清空 

当属主的权限和属组（属主属于这个属组）冲突的时候，以属主的权限为主，例如a.txt 的所属主是 cmc  所属组是 czmc，同时czmc中包含了 cmc这个用户，如果这个时候这个文件的所属主的权限是 0（没有权限 - - -）  所属组的数字权限是2（-w-），那么属主以权限0为准，就是属主自己的权限而不是属组的权限2。



<!--如果我们从root用su命令切换到普通用户，那么我们在命令行中直接输入exit退出该普通用户的时候，会回到root用户-->



特殊权限：

- SUID 用于二进制可执行文件，执行命令时获取到文件属主权限（这个权限的表示是在文件权限中用s替换掉x），例如 /usr/bin/passwd，这个命令是修改用户密码，用户密码存在/etc/shadow 文件中，这个文件的权限是000 也就是只有root用户才能查看修改等，但是普通用户想改密码的时候就不行了，因此才有了这个SUID权限，让改密码的命令在执行的时候临时自动的获取到所属主（或者是root）权限，去操作
- SGID：用于目录，在该目录下创建新的文件和目录，权限自动个改为该目录的属组，如做文件共享的时候
- SBIT：用于目录（在权限位的最后，用t来表示该权限），该目录下新建的文件和目录，仅ROOT和自己可以删除 如/tmp上有这个权限位，因为tmp中都是 每个用户创建的临时文件等，所以这个tmp目录的权限是放到最大的777 ，这样为了防止其他用户删除别的用户的东西，因此产生了这个权限命令

chmod 命令同样可以更改特殊权限，只需要在我们普通权限的数字前面在加以为特殊权限的数字（4777 第一位的4就是表示SUID这个权限     同理1777  表示增加了SBIT权限，那么 2777就是SGID的了，他的权限符号表示也是s，不过这个s是放在了用户组的x的位置上的） 特殊权限不建议个人去设定，采用系统默认即可 

## 5.网络管理

网络状态查看工具

centos7以前，我们都用 net-tool工具包它包含的命令有

- ifconfig 查询ip地址 普通用户可能需要使用完整的路径名称（/sbin/ifconfig)
- route
- netstat

7以后，用的工具为iproute(或他的第二个版本 iproute2),他的命令有

- ip
- ss

ifconfig命令

- eth0 第一块网卡（网络接口）
- 你的第一个网络接口可能叫做一下的名字
  - eno1  板载网卡
  - ens33 PCI-E网卡
  - enp0s3 无法获取物理信息的PCI-E网卡
- CentOS7使用了一致性网络设备命名，以上都不匹配则使用eth0

有时候我们可能要对网卡进行批量的操纵，如果网卡的数量很大，整百上千，那么网卡的名字的不同便会很大影响操作的效率，这时候，我们可以将网卡的命名转换成eth0的命字

网卡命名规则受 biosdevname和net.ifnames两个参数影响

1.编辑/etc/default/grub文件，增加biosdevname=0  net.ifname = 0 

```properties
GRUB_TIMEOUT=5
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=saved
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL_OUTPUT="console"

#注意这里 在这里添加
GRUB_CMDLINE_LINUX="crashkernel=auto rd.lvm.lv=centos/root rd.lvm.lv=centos/swap rhgb quiet 我们在这个添加这两个参数，中间用逗号隔开"


GRUB_DISABLE_RECOVERY="true"
```

2.更新 grub   : #grub2-mkconfig -o /boot/grub2/grub.cfg 更新这个刚刚修改的文件

经过上边的两个步骤，就可以将名字改回来。

总结网络接口命名修改步骤：

1. 网卡命名规则受biosdevname和net.ifnames两个参数影响

2. 编辑/etc/default/grub文件，增加biosdevname=0 net.ifnames=0

3. 更新grub   : #grub2-mkconfig -o /boot/grub2/grub.cfg

4. 重启服务器：#reboot

   

   

   组合模式：

    

   |       | biosdevname | net.ifnames | 网卡名 |
   | ----- | ----------- | ----------- | ------ |
   | 默认  | 0           | 1           | ens33  |
   | 组合1 | 1           | 0           | em1    |
   | 组合2 | 0           | 0           | eth0   |

查看网卡物理链接情况：mii-tool eth0  ->  查看网卡的链接状态

查看网关命令

- route -n
- 使用 -n 参数不解析主机名（因为可能解析主机名会很耗时间）



ifconfig 后 大体上得到的输出信息如下：

```
ens192: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.8.37  netmask 255.255.255.0  broadcast 192.168.8.255
        inet6 fe80::250:56ff:feb5:db8a  prefixlen 64  scopeid 0x20<link>
        ether 00:50:56:b5:db:8a  txqueuelen 1000  (Ethernet)
        RX packets 430542565  bytes 130345652395 (121.3 GiB)
        RX errors 0  dropped 25991  overruns 0  frame 0
        TX packets 447516297  bytes 940190918651 (875.6 GiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 0  (Local Loopback)
        RX packets 10335131  bytes 67331300544 (62.7 GiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 10335131  bytes 67331300544 (62.7 GiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0


```

ens192就是表示PIC-E网卡，其中 inet 就是表示本机的ip地址  netmask：子网掩码

ether：网卡的mac地址   RX是接受的包的数量和大小 TX是发送的数据包的数量和大小，以及错误丢包等信息。



第二个数据信息的开头 lo表示环回：他的地址是127.0.0.1  lo就是本机访问的时候，输入127.0.0.1 来访问自己的端口

修改网络配置

- ifconfig<接口> <ip地址> [netmask 子网掩码] 修改接口ip地址
  - ifconfig eth0 10.211.55.4 将eth0这个网卡的ip地址改为 10.211.55.4（如果用的是云主机或者远程主机，他会造成连接立刻断开，可能会发生修改不上的问题，这时候最好用网页版的）
  - ifconfig eth0 10.211.55.4 netmask 255.255.255.0  设置这个网卡的ip的同时，设置他的子网掩码
- ifup<接口> 启动网卡 （启动和关闭网卡通常是用在我们对网卡的设置很混乱了，需要回复默认的样子，才使用这个命令）
- ifdown<接口> 关闭网卡

网关配置命令

*默认的ip地址和网关就是，如果你的配置里边没有指定访问那个ip地址的时候要走那个固定的网关，那么他就会统一走那个默认的网关（就是ip地址是0.0.0.0的*

第一行就是默认网关，所有没有具体指定的都默认走172.16.1.10这个网关

*0.0.0.0         172.16.1.10     0.0.0.0         UG    100    0        0 ens192
172.16.1.0      0.0.0.0         255.255.255.0   U     100    0        0 ens192
172.17.0.0      0.0.0.0         255.255.0.0     U     0      0        0 docker0
172.18.0.0      0.0.0.0         255.255.0.0     U     0      0        0 br-13e3baca01af*



- route add default gw<网关ip> 添加默认网关
  - route add -host 10.0.0.1 gw 10.211.55.1 指定某个ip地址的网关
- route add -host<指定ip> gw<网关ip>
- route add -net<指定网段> netmask<子网掩码> gw<网关ip>
- route del default gw 10.211.55.1 删除默认的网关（ip为：0.0.0.0）
- route add default gw 10.211.55.2 添加新的默认网关

route add -net 192.168.0.0 netmask 255.255.255.0 gw 10.211.55.3 :当我们访问192.168.0.0 这个网段的时候，我们不走默认的网关，而是走指定的10.211.55.3这个网关来访问

**网络故障排除**

- ping ：这是支持域名的

- traceroute 检测当前主机和目标主机之间的网络状态（检查丢包率）

  - traceroue 的 -w 1 参数表示，如果目标主机超时的话，那么只等待1秒钟  traceroute -w 1 www.baidu.com  ，如果你的主机和目标主机之间不支持追踪的话，那么他就会打印出来* * *，这里应该打印的是，中间经过的路由，ip地址，以及时延

- m r：检查丢包（更详细）

- nslookup：查看域名对应的ip： nsloolup www.baidu.com  打印百度对应的域名

- telnet：检查端口的连接状态：telnet  www.baidu.com 80

- tcpdump ： -i   any:抓取所有网卡的数据包，-n:将域名解析成ip的形式   port 指定抓取的端口号  命令使用凡是如下：  tcpdump -i any   -n  port 80   

  - -n   host 10.0.0.1 :捕获某个主机的包
  - -n  host 10.0.0.1 and  port 80  表示捕获 主机地址为10.0.0.1   端口号为80的数据包
  - -w   /tmp/filename  将捕获的bao保存到某个指定的文件中

- netstat ： 查看自己提供的服务的监听地址：  

  - netstat  -n 将域名解析成ip的形式 
  - -t 用tcp的协议形式展示内容，udp啥的就不要了
  - -p 显示端口对应的进程
  - -l  tcp的状态，抓捕状态为listen的

  Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
  tcp        0      0 127.0.0.1:6479          0.0.0.0:*               LISTEN      -                   
  tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -            

  tcp        0      0 192.168.8.37:6379       0.0.0.0:*               LISTEN      22394/redis-server  

  127.0.0.1:6479 表示提供服务的端口是6479，其中 只有通过ip地址为127.0.0.1访问主机的程序，才可以访问，其他的ip地址不能访问这个服务

  00.0.0.22 表示提供服务的端口是22，并且0.0.0.0表示只要能访问到这个主机的，都可以使用这个服务

- ss  ss-ntpl 和 netstat的效果一样，只不过展示格式不同

**网络管理和配置文件**

网络服务管理程序分为两种，分别是 SysV和systemd

- service network start|stop|restart
- chkconfig --list network 检查network这个网络管理命令是否启动或者关闭，如下所示，它和NetworkManager.service这个网络管理命令最好不要同时存在，只有一个启动就好，另一个禁用。

network        	0:off	1:off	2:on	3:on	4:on	5:on	6:off   数字表示级别





- chkconfig --level 2345 network off 将级别2,3,4,5中的network进行关闭

- systemctl list-unit-files NetworkManager.service

- sstemctl start|stop|restart NetworkManger

- systemctl enable|disable NetworkManger :启动或者禁用 NetworkMange这个网络管理命令

  

网络配置文件

ifcfg-eth0（eth0是指的网卡的名字）   /etc/hosts

查看网卡状态：

service network status

结果：

Configured devices:
lo ens192
Currently active devices:
lo ens192

service network restart :还原网卡配置

一般在服务器上使用的时候，还是会沿用network 的。

网卡的配置文件在 /etc/sysconfig/network-scripts/   这里边有ifcfg开头的很多文件，每一个文件对应一个网络接口。 ifcfg-eth0的内容大体如下：

```
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static    static表示是静态地址， 如果是dhcp  那么就是动态分配的ip地址
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens192   网卡的名字 如果你的网卡名字是eth0 那么这里就显示的是eth0
UUID=da638e08-d3b4-44fd-82c7-9cc0bab55543
DEVICE=ens192  网卡的设备 如果你的网卡是eth0 那么这里就显示的是eth0
ONBOOT=yes  在开机的时候这个网卡是否被启用
IPADDR=172.16.1.66  静态地址设置的ip地址
NETMASK=255.255.255.0 子网掩码
GATEWAY=172.16.1.10 网关
DNS1=114.114.114.114 
DNS2=8.8.8.8

```

hostname：查看 主机名

临时修改：hostname  xxxxxxx  

永久修改：hostnamectl  set-hostname xxxxx

很多服务是需要依赖主机名进行服务的 我们 通过vim  /etc/host 来将新的主机名写到对应的ip地址的后边  

/etc/host的内容如下：

```
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
192.168.8.36    node1
192.168.8.37    node2
192.168.8.38    node3
192.168.8.39    node4
192.168.8.40    node5

```

## 软件包管理器的使用。


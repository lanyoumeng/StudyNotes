

# 下载解压文件

在Linux中，下载和解压文件通常需要使用命令行工具。常用的下载命令是`wget`和`curl`，解压命令则依赖于文件的格式，常见的压缩格式包括`.zip`、`.tar.gz`、`.tar.bz2`等。

### 下载文件：
- **wget**：使用`wget`命令下载文件，语法如下：
    ```bash
    wget [URL]
    ```
    例如：
    ```bash
    wget https://example.com/file.zip
    ```

- **curl**：使用`curl`命令也可以下载文件，语法如下：
    ```bash
    curl -O [URL]
    ```
    例如：
    ```bash
    curl -O https://example.com/file.zip
    ```

### 解压文件：
- **解压`.zip`文件**：
    ```bash
    unzip file.zip
    ```
    解压到指定目录：
    ```bash
    unzip file.zip -d /path/to/directory
    ```

- **解压`.tar.gz`文件**：
    ```bash
    tar -xzvf file.tar.gz
    ```
    解压到指定目录：
    ```bash
    tar -xzvf file.tar.gz -C /path/to/directory
    ```

- **解压`.tar.bz2`文件**：
    ```bash
    tar -xjvf file.tar.bz2
    ```
    解压到指定目录：
    ```bash
    tar -xjvf file.tar.bz2 -C /path/to/directory
    ```

这些命令中的参数含义：
- `-x`：解压文件
- `-z`：使用gzip格式解压（仅针对`.tar.gz`文件）
- `-j`：使用bzip2格式解压（仅针对`.tar.bz2`文件）
- `-v`：显示详细信息
- `-f`：指定文件名

记住，下载文件时要小心，确保从可信任的源获取文件。解压命令根据文件格式选择相应的命令进行解压。

### 示例

```
mkdir /usr/local/zookeeper
wget https://codechina.csdn.net/weixin_44624117/software/-/raw/master/software/zookeeper-3.4.14.tar.gz
tar -zxvf zookeeper-3.4.14.tar.gz -C /usr/local/zookeeper/

添加环境变量,打开/etc/profile 文件
vim /etc/profile

最后插入一下内容
#ZOOKEEPER_HOME
export ZOOKEEPER_HOME=/usr/local/zookeeper/zookeeper-3.8.3
export PATH=$ZOOKEEPER_HOME/bin:$PATH

重新加载/etc/profile配置文件
source /etc/profile
```



## java下载

**yum install java会导致只下载jre没有jdk**

**/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.312.b07-2.el8_5.x86_64/目录下只有jre**

在某些 Linux 发行版中，OpenJDK 的安装路径可能只包含 JRE（Java Runtime Environment），缺少 JDK（Java Development Kit）所需的开发工具。如果你需要编译 Java 代码，你需要安装 JDK。

你可以通过 `yum` 安装 OpenJDK 的开发工具包，它通常被称为 `java-1.8.0-openjdk-devel` 或 `java-1.8.0-openjdk`。以下是相应的步骤：

```
sudo yum install java-1.8.0-openjdk-devel
```

这将安装 OpenJDK 的开发工具包，使你能够在系统上编译和开发 Java 程序。安装完成后，你可以检查 `/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.312.b07-2.el8_5.x86_64/` 目录，确保包含了 `bin`、`lib` 等开发工具目录。设置 `JAVA_HOME` 时，请使用包含 JDK 的目录。





# 路径

在Linux和Unix系统中，`./`表示当前目录，`/`表示根目录，而不加`/`表示相对路径。让我举个例子：

- `./gradlew` 表示当前目录下的 `gradlew` 文件。
- `/home/user/Documents` 表示根目录下的 `home` 文件夹中的 `user` 文件夹中的 `Documents` 文件夹。
- `gradlew`（不带任何前缀）表示相对于当前位置的 `gradlew` 文件。







# 基本linux命令



![基本linux命令](D:\蓝梦悠\自学\编程\linux\基本linux命令.png)



### 1.1    su命令

：切换用户

su 【用户名】

①  su root   #切换到root用户

②  su     #切换到root用户。①和②等同

③  su rjxy   #切换到rjxy用户

### 1.2    cd命令

：更改工作目录路径

①  cd /etc #切换到“/etc”目录

②  cd ..  #更改至当前目录的父目录（上一级）

③  cd ~rjxy #更改至用户rjxy的宿主目录（宿主目录，即用户的个人目录）

★  非root用户下达“cd ~root”命令是否有意义？

### 1.3    ls命令

**ls** **【选项】 【目录或文件】**

①  ls /home     #查看/home目录下的文件（不包括隐藏文件）

②  ls –a /home   #显示/root目录下所有文件（包括隐藏文件，隐藏文件前面带“.”）

③  ls –l /etc   #长格式显示所有内容（相当于ll命令）

### 1.4    touch

：创建空文件，更改文件的时间

touch【-r<参考文件或目录>】【-t<日期时间>】【文件】

u -r：使用参考档的时间

u -t：设定（修改）文件的修改时间，日期格式为“MMDDHHmm”

①  touch file       #创建文件file

②  touch file1 file2 file3 #创建文件“file1”、“file2”和“file3”

### 1.5    mkdir

：创建目录

mkdir 【选项】 【目录名】

①  mkdir a     #在当前目录下创建新目录“a”

②  mkdir /home/b #在“/home”目录下创建新目录“b”

【注意】绝对路径和相对路径

★  绝对路径：从根目录出发，以根目录为参考，如/home，表示根目录下的home子目录；

★  相对路径：从当前目录出发，以当前目录为参考，如home，表示当前目录下的home子目录。

### 1.6    rm

：删除文件和目录

u -f：强制删除，不再询问

u -r：删除全部文件、目录和子目录

①  touch file1       #在当前目录下创建文件file1

rm file1        #交互式地删除文件file1

②  mkdir m1        #在当前目录下创建目录m1

③  rm –r m1        #交互式地删除目录m1

④  touch file2  file3   #在当前目录下创建文件file2和file3

rm file2         #不带-f选项，交互式删除

rm -f file3       #带-f选项，非交互式，不提示，直接删除

⑤  mkdir m2 m3      #在当前目录下创建目录m2和m3

rm -r m2         #不带-f选项，交互式删除

rm -rf m3        #带-f选项，非交互式，不提示，直接删除

 

### curl

 **http://localhost:8081 在bash上打出完整网站就可以通过鼠标访问网站**

用于向网络服务器发送 HTTP 请求并获取响应。它支持多种协议，包括 HTTP、HTTPS、FTP、FTP上传、SCP、SFTP、LDAP、LDAPS、DICT、TELNET、FILE、IMAP、SMTP、POP3 和 RTSP。

以下是一些常见的 `curl` 命令示例：

1. **发起 GET 请求**：
   
   ```shell
   curl https://www.example.com
   ```
   
2. **指定请求方法（POST）和数据**：
   ```shell
   curl -X POST -d "key1=value1&key2=value2" https://www.example.com
   ```

3. **保存响应到文件**：
   ```shell
   curl -o output.html https://www.example.com
   ```

4. **跟随重定向**：
   ```shell
   curl -L https://www.example.com
   ```

5. **自定义请求头**：
   ```shell
   curl -H "Authorization: Bearer your_token" https://www.example.com
   ```

6. **上传文件**：
   ```shell
   curl -F "file=@/path/to/your/file" https://www.example.com/upload
   ```

7. **基本身份验证**：
   ```shell
   curl -u username:password https://www.example.com
   ```

8. **设置请求超时**：
   ```shell
   curl --max-time 10 https://www.example.com
   ```

9. **显示响应头信息**：
   ```shell
   curl -I https://www.example.com
   ```

10. **显示请求和响应的详细信息**：
    ```shell
    curl -v https://www.example.com
    ```

这些是 `curl` 命令的一些常见用法示例。您可以根据需要使用不同的选项和参数来定制 HTTP 请求。`curl` 是一个功能强大且灵活的工具，用于与 Web 服务器进行通信，执行各种操作，如获取数据、上传文件、测试 API 端点等。



### lsof

（List Open Files）是一个用于列出当前系统上打开的文件和进程的命令行工具。它提供了关于系统资源的详细信息，包括文件、网络连接、目录等，以及与它们相关联的进程。

以下是一些常见用法示例和选项：

1. **列出所有打开的文件和目录**：
   ```shell
   lsof
   ```

2. **列出指定用户的打开文件**：
   ```shell
   lsof -u username
   ```

3. **列出特定进程的打开文件**：
   ```shell
   lsof -p pid
   ```

4. **列出某个目录下被打开的文件**：
   ```shell
   lsof /path/to/directory
   ```

5. **列出网络连接**：
   ```shell
   lsof -i
   ```

6. **列出监听某端口的进程**：
   ```shell
   lsof -i :port
   ```

7. **列出 Unix 域套接字**：
   ```shell
   lsof -U
   ```

8. **显示文件描述符（File Descriptors）信息**：
   ```shell
   lsof -d fd
   ```

9. **列出文件被哪个进程打开**：
   ```shell
   lsof /path/to/file
   ```

10. **列出被删除的文件**：
    ```shell
    lsof +L1
    ```

11. **输出以某种格式显示**：
    ```shell
    lsof -F format
    ```

12. **以伪终端方式显示**：
    ```shell
    lsof -t
    ```

`lsof` 的输出通常包括文件名、文件描述符（File Descriptor，通常是整数）、进程 ID（PID）、用户、文件类型、文件模式（如读、写、执行）、以及其他信息。

`lsof` 是一个强大的工具，通常用于系统管理员、开发人员和调试人员在分析系统资源使用和问题排查时。请注意，执行 `lsof` 命令通常需要具有足够的权限，因为它会访问系统的敏感信息。







### ln

软链接（Symbolic Links）和硬链接（Hard Links）是文件系统中创建链接的两种方式，它们有一些关键区别：

1. **软链接**：
    - 软链接是一个指向文件或目录的路径的引用，它创建了一个新的文件实体，该实体指向原始文件或目录。
    - 软链接可以跨越文件系统，并且即使目标文件不存在也可以创建。
    - 当原始文件被删除或移动时，软链接会失效。
    - 软链接有自己的 inode 和数据块。
    - 使用 `ln -s` 命令创建软链接。

2. **硬链接**：
    - 硬链接是原始文件数据块的另一个目录项，它与原始文件共享相同的 inode。
    - 硬链接只能在同一文件系统中创建。
    - 即使原始文件名被删除，只要还有硬链接存在，文件数据仍然存在。
    - 删除原始文件并不会影响硬链接的数据，因为它们共享相同的数据块。
    - 硬链接没有自己的 inode，只是 inode 的另一个入口。
    - 使用 `ln` 命令创建硬链接。

总的来说，软链接是一个指向文件路径的引用，而硬链接是直接指向文件数据的另一个目录项。软链接可以跨文件系统，但对于原始文件的删除或移动更敏感，而硬链接必须在同一文件系统内创建，但对于原始文件的维护更加稳固。



# vi/vim编辑器

vi是Linux操作系统的一个编辑器，vim是vi的升级版，不仅兼容vi编辑器的所有指令，而且在vi编辑器的基础上增加了颜色显示，便于程序开发人员编写程序，同时vim也加入了很多额外的功能，如支持多文本编辑、多窗口显示等。

##   **vi/vim**命令

① vi file1     #用vi打开文件file1进行编辑

② vim file1    #用vim打开文件file1进行编辑

##  **vi/vim**的三种模式

vi/vim编辑器共分为三种工作模式：普通模式、编辑模式和命令模式。

![img](file:///C:/Users/to'm/AppData/Local/Temp/msohtmlclip1/01/clip_image002.jpg)

\1)  普通模式：用vim打开一个文档后，直接见到的就是普通模式，在该模式下，用户可以通过“←↑↓→”按键来移动光标，可以删除字符或删除整行，也可以复制和粘贴数据。

u 复制一行-yy；

u 复制多行-nyy（n为数字，是要复制的行数）；

u 粘贴-p或P（p是粘贴到光标所在行的下一行，P是粘贴到光标所在行的上一行）；

u 删除一行-dd；

u 删除多行-ndd（n为数字，是要删除的行数）；

u 撤销-u；

u 恢复-Ctrl+r。

\2)  编辑模式：在普通模式中可以进行删除、复制、粘贴等操作，但是却无法向文档中输入字符。此时按下字母“i”、“I”、“o”、“O”、“a”、“A”、“r”或“R”，编辑器将从普通模式转入编辑模式，同时在屏幕左下方会出现“INSERT”或“REPLACE”的字样，此时才可以向文档中输入字符。在整个输入过程结束后，按“Esc”键，即可返回普通模式，此时光标将处于刚才输入的最后一个字符的位置。

\3)  命令模式：在普通模式中，输入“：”、“/”或“？”，编辑器将从普通模式转入命令模式，此时屏幕左下角将出现“：”、“/”或“？”的标志。在命令模式中，用户可以完成搜索、替换、高亮显示、行号显示、保存、退出甚至执行shell 指令等操作。

u 显示行号-set nu；（取消显示行号-set nonu）

u 存盘-w；

u 退出-q；

u 存盘退出-wq；

u 不存盘强制退出-q！。

【注意】如果编辑过程中用ctrl+z组合键，则vi编辑器会转入后台，此时再用vim编辑文件时会有警告信息，因为vim编辑文件时生成一个交换文件，如果vim没有正常退出，该交换文件还存在，会影响再次编辑，解决方法是删除交换文件“.ceshi.swp”。交换文件和原文件在同一个目录下，是一个隐藏文件，可用“ls -a”查看到。

 

# 2.2    gcc编译器

### 简介

gcc （GNU Compiler Collection，GNU编译器套件）是GNU 项目的编译器组件之一，也是GNU 软件产品家族具有代表性的作品。

gcc 是一个交叉平台的编译器，目前支持几乎所有主流CPU 处理器平台，它可以完成从C、C++、objective-C等**源文件向运行在特定**CPU硬件上的目标代码的转换**，gcc不仅功能非常强大，结构也异常灵活，便携性（portable）与跨平台支持（cross-platform support）特性是gcc的显著优点。

在gcc 设计之初，仅仅是作为一个C语言的编译器，可是经过十多年的发展，gcc已经不仅仅能支持C语言；它现在还支持Ada、C++、Java、Objective C、Pascal、COBOL，以及支持函数式编程和逻辑编程的Mercury语言等等

gcc 是一组编译工具的总称，其软件包里包含众多的工具，按其类型，主要有以下的分类：

①  C编译器 cc, cc1, cc1plus, gcc

②  C++编译器 c++, cc1plus, g++

③  库文件 libgcc.a, libgcc_eh.a, libgcc_s.so, libiberty.a, libstdc++.[a,so], libsupc++.a

用gcc编译程序生成可执行文件有时候看起来似乎仅通过编译一步就完成了，但事实上，使用gcc编译工具由C语言源程序生成可执行文件的过程并不单单是一个编译的过程，完整的编译流程要经过下面的几个过程：

l 预处理（Pre-Processing）：gcc首先调用cpp命令进行预处理，主要实现对源代码编译前的预处理，比如将源代码中指定的头文件包含进来；

l 编译（Compiling）：调用cc1 命令进行编译，将源代码翻译生成汇编代码；

l 汇编（Assembling）：调用as 命令进行工作，将汇编代码生成扩展名为.o 的目标文件；目标文件包含机器代码（可直接被CPU执行）以及代码在运行时使用的数据，如[重定位](https://baike.baidu.com/item/重定位)信息、用于链接或调试的程序符号（[变量](https://baike.baidu.com/item/变量)和函数的名字）等； 

l 链接（Linking）：调用链接器ld处理可重定位文件，把它们的各种符号引用和符号定义转换为可执行文件中的合适信息(一般是虚拟内存地址)的过程。

## 安装

gcc使用时需安装如下软件：

①  ppl-0.10.2-11.el6.i686.rpm

②  cloog-ppl-0.15.7-1.2.el6.i686.rpm

③  mpfr-2.4.1-6.el6.i686.rpm

④  cpp-4.4.5-6.el6.i686.rpm

⑤  glibc-headers-2.12-1.25.el6.i686

⑥  glibc-headers-2.12-1.25.el6.i686.rpm

⑦  glibc-devel-2.12-1.25.el6.i686.rpm

⑧  gcc-4.4.5-6.el6.i686.rpm

⑨  libstdc++-devel-4.4.5-6.el6.i686.rpm

⑩  gcc-c++-4.4.5-6.el6.i686.rpm

由于所需安装软件包较多，可以只记住gcc和gcc-c++，由于其他的软件包都是这两个安装包的依赖文件，所以只需在安装gcc和gcc-c++的过程中根据提示再安装相应的依赖包即可。

最简便的方式是使用yum安装：

yum install gcc

yum install gcc-c++。

【注意】yum 是基于rpm 包管理工具，能够从指定的安装源（服务器，本地目录等）自动下载目标 rpm包并且安装，可以自动处理依赖性关系并进行下载、安装，无须繁琐地手动下载、安装每一个需要的依赖包。但是yum的rpm包来源于安装源，所以要使用yum，必须先设置yum安装源，安装源由/etc/yum.repos.d/目录中的.repo文件配置指定。安装源可以是一个网络服务器地址，也可以是本地的安装光盘。

用yum安装gcc方法如下：

\1.    用RHEL安装光盘建立本地yum安装源

①  “虚拟机”→“设置”→“CD/DVD（IDE）”→“连接”→“使用ISO镜像文件”，通过“浏览”，选择Redhat安装光盘，并确保“设备状态”中的“已连接”选项被勾选；

②  把光驱挂载到/aa目录（也可以是其他目录）

【说明】挂载：在Linux操作系统中，挂载是指将一个设备（通常是存储设备）挂接到一个已存在的目录上。我们要访问存储设备中的文件，必须将文件所在的分区挂载到一个已存在的目录上，然后通过访问这个目录来访问存储设备。

a)  mkdir /aa           #创建目录（挂载点）

b)  mount /dev/sr0 /aa      #挂载光驱

c)  mount -s            #查看是否挂载成功

③  vim /etc/yum.repos.d/rhel-source.repo   #rhel-source.repo文件是系统自带的yum源文件，可以直接在该文件上添加安装源的信息，但该文件只能由root用户修改。如果没有该文件，也可以自行创建一个新的文件，文件名可以任意，只需要以“.repo”结尾即可，如dvd.repo。

【向配置文件里面添加新如下新内容】

[dvd]

name = install dvd

baseurl = file:///aa

enabled = 1             #yum源生效

gpgcheck = 0            #不对文件进行校验

\2.  安装gcc

①  yum install gcc        #安装c编译器，安装时可以加上“-y”选项，在安装时就不再需要确认

②  yum install gcc-c++      #安装c++编译器

【注意】如果安装gcc时正常，继续安装gcc-c++时提示如下错误： 

![img](file:///C:/Users/to'm/AppData/Local/Temp/msohtmlclip1/01/clip_image004.jpg)

这种情况说明yum安装源有问题，查看/etc/yum.repos.d目录，发现该目录下除了dvd.repo外多了一个文件packagekit-media.repo

![img](file:///C:/Users/to'm/AppData/Local/Temp/msohtmlclip1/01/clip_image006.jpg)

将该文件删除，删除后再次运行yum install gcc-c++重新安装gcc-c++即可。

![img](file:///C:/Users/to'm/AppData/Local/Temp/msohtmlclip1/01/clip_image008.jpg)

##  常用编译选项及编译流程分析

#### 基本语法

【基本语法】  gcc [option | filename ]…

对于编译C++的源程序，其基本的语法如下。

【基本语法】  g++ [ option | filename ]…

其中option 为gcc 使用时的选项，而filename 为需要用gcc 作编译处理的文件名。就gcc 来说，其本身是一个十分复杂的命令，合理地使用其命令选项可以有效提高程序的编译效率、优化代码。



gcc有超过100 个的编译选项可用，这里仅介绍最常用的几种：

#### **基本编译选项**

①  **无选项：**不带任何选项，可以将指定源文件编译成名为a.out的可执行程序

  #gcc test.c     #在当前目录下生成一个名为a.out的可执行文件  

②  **-o选项：指定生成的可执行程序的文件名**

在默认的状态下，如果gcc指令没有指定编译选项的情况下会在当前目录下生成一个名位a.out的可执行程序，例如：执行# gcc Test.c命令之后会生成一个a.out的可执行程序。因此，为了指定生成的可执行程序的文件名，就可以采用-o选项，比如下面的指令：

**![img](file:///C:/Users/to'm/AppData/Local/Temp/msohtmlclip1/01/clip_image010.png)**

执行该指令会在当前目录下生成一个名为Test的可执行文件。

【注意】使用-o选项时，-o后面必须带有可执行文件的文件名（可以任意指定）



其他编译选项

①  **-E选项：处理选项**，预处理的结果直接打印输出；如果加上-o选项，可以生成.i为后缀的预处理文件。

②  ***-S*选项：编译选项**，该选项会生成一个后缀名为.s的汇编语言文件，不会生成可执行的程序。

③  **-c** **选项：汇编选项**，把.s为后缀的汇编文件生成以.o为后缀的目标文件

这是gcc 命令的常用选项，仅把源程序编译为目标代码而并不做链接的工作，所以采用该选项的编译指令同样不会生成最终的可执行程序，而是生成一个与源程序文件名相同的以.o为后缀的目标文件。

一个Test1.c的源程序经过下面的编译之后会生成一个Test1.o的文件。

**![img](file:///C:/Users/to'm/AppData/Local/Temp/msohtmlclip1/01/clip_image012.png)**

【注意】目标文件包含机器语言代码，但目标文件不能运行，它还缺少系统的启动代码和库代码。目标文件、启动代码和库代码由链接器结合在一起，放在一个文件里，这个文件才是可执行文件。

④  **-v**选项：**在Shell的提示符号下键入gcc -v，屏幕上就会显示出目前正在使用的**gcc的版本信息。**

⑤  **-x language**：**强制编译器用**指定的语言编译器**来编译某个源程序。

![img](file:///C:/Users/to'm/AppData/Local/Temp/msohtmlclip1/01/clip_image014.png)

该指令表示强制采用C++编译器来编译C程序P1.c。

⑥  **-static**选项：gcc在默认情况下链接的是动态库，有时为了把一些函数静态编译到程序中，而无需链接动态库就采用-static选项，它会**强制程序链接静态库**。

 

------



#### 示例

【例1】 下面举一个简单的例子来说明gcc的编译过程。首先用vi编辑器编辑一个简单的c程序test.c：

![img](file:///C:/Users/to'm/AppData/Local/Temp/msohtmlclip1/01/clip_image016.png)

根据前面讲到的内容，使用gcc命令来编译该程序：

![img](file:///C:/Users/to'm/AppData/Local/Temp/msohtmlclip1/01/clip_image018.png)

可以从上面的编译过程看到，编译一个这样的程序非常简单，一条指令即可完成，事实上，这一条指令掩盖了很多细节。我们可以从编译器的角度来看上述的编译过程，这对于更好理解gcc编译工作原理有很好的帮助。

①  预处理阶段：生成后缀名为“.i”的文件

gcc编译器首先做的工作是预处理：调用-E 参数可以让gcc在预处理结束后停止编译过程。

**![img](file:///C:/Users/to'm/AppData/Local/Temp/msohtmlclip1/01/clip_image020.png)**

编译器在这一步调用cpp工具来对源程序进行预处理，此时会生成test.i文件，下面部分列出了test.i文件中的内容。

**![img](file:///C:/Users/to'm/AppData/Local/Temp/msohtmlclip1/01/clip_image022.png)**

查看代码会发现stdio.h的内容都被加入到该文件里去了，而且被预处理的宏定义也都作了相应的处理。

②  编译阶段

编译器在预处理结束后，gcc首先要检查代码的规范性、是否有语法错误等，以确定代码实际要做的工作。在检查无误后，就开始把代码翻译成汇编语言。

gcc的选项“-S”能使编译器在进行完汇编之前就停止。

\# gcc -S test.i -o test.s

以下列出了test.s的内容，有兴趣的同学可以分析一下这个简单的C语言小程序用汇编代码是如何实现的。

![img](file:///C:/Users/to'm/AppData/Local/Temp/msohtmlclip1/01/clip_image024.jpg)

可以看到，这个C语言小程序在汇编中已经复杂很多了，这也是C语言作为中级语言的优势所在。

③  汇编阶段

汇编阶段是把编译阶段生成的“.s”文件生成目标文件，通过使用-c参数来完成。

\# gcc -c test.s -o test.o

④  链接阶段

成功编译后，即进入链接阶段，这个阶段需要用到函数库。

在程序中没有定义“printf”的函数实现，在预编译中包含的“stdio.h”头文件中也只有该函数的声明，而没有定义函数的实现，那么究竟在哪里实现“printf”函数呢？

系统已经把这些函数实现放入名为lib.so.6的库文件中了，在没有特别指定时，gcc会到系统默认的搜索路径/usr/lib下进行查找，也就是链接到libc.so.6库函数中去，这样就能实现函数“printf”了，这也就是链接的作用。

完成链接后，gcc就可以生成最终的可执行文件：

**![img](file:///C:/Users/to'm/AppData/Local/Temp/msohtmlclip1/01/clip_image026.png)**



# systemctl

是Linux系统中用于管理系统服务的命令行工具。它允许您启动、停止、重启、启用或禁用系统服务，以及查看服务的状态和日志。以下是一些常用的`systemctl`命令：

1. 启动一个服务：

   ```
   systemctl start <service-name>
   ```

   例如，要启动Redis服务，您可以运行：

   ```
   systemctl start redis
   ```

2. 停止一个服务：

   ```
   systemctl stop <service-name>
   ```

   同样，要停止Redis服务，您可以运行：

   ```
   systemctl stop redis
   ```

3. 重启一个服务：

   ```
   systemctl restart <service-name>
   ```

   这将停止并重新启动指定的服务。

4. 启用一个服务（使其在系统启动时自动启动）：

   ```
   systemctl enable <service-name>
   ```

   若要启用Redis服务，以便在系统启动时自动启动，您可以运行：

   ```
   systemctl enable redis
   ```

5. 禁用一个服务（使其在系统启动时不自动启动）：

   ```
   systemctl disable <service-name>
   ```

   这将禁用Redis服务，使其在系统启动时不会自动启动。

6. 查看服务的状态：

   ```
   systemctl status <service-name>
   ```

   这将显示有关服务的当前状态信息，包括是否正在运行以及最近的日志消息。

7. 查看服务的日志：

   ```
   journalctl -u <service-name>
   ```

   这将显示与指定服务相关的系统日志消息。您可以使用不同的选项来限制日志的数量和详细程度。

`systemctl`是一个非常有用的工具，可以帮助您管理系统上运行的服务，确保它们按照预期工作并在需要时启动或停止。请注意，执行`systemctl`命令通常需要超级用户（root）或具有适当权限的用户。



# 进程控制

**一、**      **Linux** **进程概述**

\1.    进程的标识符（PID）

在Linux 中最主要的进程标识有进程号（PID，Process Idenity Number）和它的父进程号（PPID，parent processID）。其中PID 惟一地标识一个进程。PID 和PPID 都是非零的正整数。

在Linux 中获得当前进程的PID 和PPID 的系统调用函数为getpid()和getppid()，通常程序获得当前进程的PID 和PPID 之后，可以将其写入日志文件以做备份。getpid()和getppid()系统调用过程如下所示：

  /* pid.c */  #include<stdio.h>  int main()  {  /*获得当前进程的进程ID 和其父进程ID*/  printf("The PID of this process is %d\n", getpid());  printf("The PPID of this process is %d\n", getppid());  getchar();  }  

该程序编译执行后结果如下，该值在不同的系统上会有所不同：

​    $ ./pid                                                                       

​    The PID of this process is 78                                                    

​    THe PPID of this process is 36                                                  

在另一个终端中可以通过ps -e或者ps -u查看上述PID所对应的进程。

\2.    Linux 下的进程管理

| 表1 Linux中进程调度常见命令 |                      |
| --------------------------- | -------------------- |
| 选项                        | 参数含义             |
| ps                          | 查看系统中的进程     |
| top                         | 动态显示系统中的进程 |
| kill                        | 杀死进程             |
| jobs                        | 查看后台运行的进程   |

​    \# ps -e                           //查看所有进程                             

​    \# ps -u                           //显示当前用户和终端进程                  

​    \# ps -l                           //长格式显示当前用户进程                  

​    \# ps -le                          //长格式显示所有进程                       

 

**二、**      **Linux** **进程控制编程**

\1.    fork()基本介绍

在Linux 中创建一个新进程的惟一方法是使用fork()函数。fork()函数是Linux 中一个非常重要的函数，和以往遇到的函数有一些区别，因为它看起来执行一次却返回两个值。

\1)   fork()函数说明。

**fork()****函数用于从已存在的进程中创建一个新进程**。新进程称为子进程，而原进程称为父进程。**使用****fork()****函数得到的子进程是父进程的一个复制品**，它从父进程处继承了整个进程的地址空间，包括进程上下文、代码段、进程堆栈、内存信息、打开的文件描述符、信号控制设定、进程优先级、进程组号、当前工作目录、根目录、资源限制和控制终端等，而子进程所独有的只有它的进程号、资源使用和计时器等。

实际上，**在父进程中执行****fork()****函数时，父进程会复制出一个子进程**，而且**父子进程的代码从****fork()****函数的返回开始分别在两个地址空间中同时运行**。从而**两个进程分别获得其所属****fork()****的返回值，其中在父进程中的返回值是子进程的进程号，而在子进程中返回****0**。因此，可以通过返回值来判定该进程是父进程还是子进程。

同时可以看出，**使用****fork()****函数的代价是很大的**，它复制了父进程中的代码段、数据段和堆栈段里的大部分内容，使得fork()函数的系统开销比较大，而且执行速度也不是很快。

**fork()****函数可以让父子进程执行不同的代码段。**

\2)   fork()函数语法。

表2列出了fork()函数的语法要点：

| 表2 fork()函数语法要点           |                                                              |
| -------------------------------- | ------------------------------------------------------------ |
| 所需头文件                       | #include  <sys/types.h> // 提供类型pid_t 的定义  #include  <unistd.h> |
| 函数原型                         | pid_t[[y1\]](#_msocom_1)  fork(void)                         |
| 返回值                           | 0：子进程                                                    |
| 子进程ID（大于0 的整数）：父进程 |                                                              |
| -1：出错                         |                                                              |

\3)   fork()函数使用实例。

  /* fork.c */  #include <sys/types.h>  #include <unistd.h>  #include <stdio.h>  #include <stdlib.h>  int main(void)  {  pid_t result;  /*调用fork()函数*/  result = **fork()**;  /*通过result 的值来判断fork()函数的返回情况，首先进行出错处理*/  if(result == -1)  {  printf("Fork error\n");  }  else if (result == 0) /*返回值为0 代表子进程*/  {  printf("The returned value is %d\n In child  process!!\n My PID is %d\n",result,getpid());  }  else /*返回值大于0 代表父进程*/  {  printf("The returned value is %d\n In father  process!!\n My PID is %d\n",result,getpid());  }  sleep(60); /*让进程休眠，以便通过ps可以查看到当前进程的PID*/  return result;  }  

运行结果如下所示：

  **# ./fork**                          

  The returned value is 76 /* 在父进程中打印的信息 */       

  In father process!!                     

  My PID is 75                        

  The returned value is :0 /* 在子进程中打印的信息 */       

  In child process!!                     

  My PID is 76                        

  **# ps -e        /\*** **查看父进程和子进程的****PID\*/**      

从该实例中可以看出，使用fork()函数新建了一个子进程，其中的父进程返回子进程的PID，而子进程的返回值为0。

  【小知识】  由于fork()完整地复制了父进程的整个地址空间，因此执行速度是比较慢的。为了加快fork()的执行速度，有些UNIX 系统设计者创建了vfork()。vfork()也能创建新进程，但它不产生父进程的副本，它是**通过允许父子进程可访问相同物理内存从而伪装了对进程地址空间的真实拷贝，当子进程需要改变内存中数据时才复制父进程**。这就是著名的“写操作时复制”（copy-on-write）技术。  

\2.    exec 函数族

\1)   exec 函数族说明。

fork()函数是用于创建一个子进程，该子进程几乎复制了父进程的全部内容，但是，如何在进程中执行其他程序呢？exec函数族就提供了一个在进程中启动另一个程序执行的方法。

**exec** **函数族可以根据指定的文件名或目录名找到可执行文件，并用它来取代原调用进程的数据段、代码段和堆栈段，在执行完之后，原调用进程的内容除了进程号外，其他全部被新的进程替换了**。另外，这里的可执行文件既可以是二进制文件，也可以是Linux下任何可执行的脚本文件。

**在进程中使用****exec****函数族的作用是使运行结果替换原有进程，所以为了避免父进程被替换掉，需要先在父进程中通过****fork()****函数创建子进程，然后在子进程中使用****exec****函数**。**在****shell****命令行执行****ps****命令，实际上是****shell****进程调用****fork****复制一个新的子进程，再利用****exec****系统调用将新产生的子进程完全替换成****ps****进程**

在Linux 中使用exec 函数族主要有两种情况。

l 当进程认为自己不能再为系统和用户做出任何贡献时，就可以调用exec函数族中的任意一个函数让自己重生。

l 如果一个进程想执行另一个程序，那么它就可以调用fork()函数新建一个进程，然后调用exec函数族中的任意一个函数，这样看起来就像通过执行应用程序而产生了一个新进程（这种情况非常普遍）。

\2)   exec 函数族语法。

实际上，在Linux中并没有exec()函数，而是有6个以exec开头的函数，它们之间语法有细微差别。

表3列举了exec 函数族的6 个成员函数的语法。

| 表3 exec函数族成员函数语法                                   |                                                    |
| ------------------------------------------------------------ | -------------------------------------------------- |
| 所需头文件                                                   | #include  <unistd.h>                               |
| 函数原型                                                     | int execl(const  char *path, const char *arg, ...) |
| int execv(const  char *path, char *const argv[])             |                                                    |
| int execle(const  char *path, const char *arg, ..., char *const envp[]) |                                                    |
| int execve(const  char *path, char *const argv[], char *const envp[]) |                                                    |
| int execlp(const  char *file, const char *arg, ...)          |                                                    |
| int execvp(const  char *file, char *const argv[])            |                                                    |
| 函数返回值                                                   | -1：出错                                           |

这6个函数在函数名和使用语法的规则上都有细微的区别，下面就可执行文件查找方式、参数表传递方式及环境变量这几个方面进行比较。

l 查找方式。

表3中的前4 个函数的查找方式都是完整的文件目录路径，而最后2 个函数（也就是以p 结尾的两个函数）可以只给出文件名，系统就会自动按照环境变量“$PATH”所指定的路径进行查找。

l 参数传递方式。

exec 函数族的参数传递有两种方式：一种是逐个列举的方式，而另一种则是将所有参数整体构造指针数组传递。

在这里是以函数名的第5 位字母来区分的，字母为“l”（list）的表示逐个列举参数的方式，其语法为char *arg；字母为“v”（vector）的表示将所有参数整体构造指针数组传递，其语法为char *const argv[]。

**这里的参数实际上就是用户在使用这个可执行文件时所需的全部命令选项字符串（包括该可执行程序命令本身）。要注意的是，这些参数必须以****NULL** **表示结束****，****如果使用逐个列举方式，那么要把它强制转化成一个字符指针，否则****exec** **将会把它解释为一个整型参数**，那么exec 函数就会报错。

l 环境变量。

exec 函数族可以使用系统默认的环境变量，也可以传入指定的环境变量。这里以“e”（environment）结尾的两个函数execle()和execve()就可以在envp[]中指定当前进程所使用的环境变量。

表4是对这4 个函数中函数名和对应语法的小结，主要指出了函数名中每一位所表明的含义，希望大家结合此表加以记忆。

| 表4 exec函数名对应含义        |                           |                       |
| ----------------------------- | ------------------------- | --------------------- |
| 前4位                         | 统一为：exec              |                       |
| 第5位                         | l：参数传递为逐个列举方式 | execl、execle、execlp |
| v：参数传递为构造指针数组方式 | execv、execve、execvp     |                       |
| 第6位                         | e：可传递新进程环境变量   | execle、execve        |
| p：可执行文件查找方式为文件名 | execlp、execvp            |                       |

![img](file:///C:/Users/to'm/AppData/Local/Temp/msohtmlclip1/01/clip_image002.jpg)![IMG_256](file:///C:/Users/to'm/AppData/Local/Temp/msohtmlclip1/01/clip_image003.gif)

 

\3)   exec 使用实例。

下面的第一个示例说明了如何使用文件名的方式来查找可执行文件，同时使用参数列表的方式。

l execlp()函数

【函数语法】

execlp(file，arg1，arg2，……，NULL)

execlp()会从PATH 环境变量（可通过env命令查看$PATH）所指的目录中**查找符合参数****file****的文件名，找到后便执行该文件**，然后**将第二个以后的参数当做该文件的****argv[0]****、****argv[1]……**，**最后一个参数必须用空指针****(NULL)****作结束**。

  **/\*execlp.c\*/**  #include <unistd.h>  #include <stdio.h>  #include <stdlib.h>  #include <errno.h>     int main()  {  if (**fork()** == 0)  {  /*调用execlp()函数，这里相当于调用了"ps -u"命令*/  if ((**execlp("ps",  "ps", "-u", NULL)**) < 0)  {  perror("Execlp error\n");[[y2\]](#_msocom_2)   }  }  }  

在该程序中，首先使用fork()函数创建一个子进程，然后在子进程里使用execlp()函数。这里的参数列表列出了在shell 中使用的命令名和选项。并且当使用文件名进行查找时，系统会在默认的环境变量PATH 中寻找该可执行文件。

运行结果如下：

![img](file:///C:/Users/to'm/AppData/Local/Temp/msohtmlclip1/01/clip_image005.jpg)

  \# env           /* 查看环境变量，可看到PATH的值*/  

![img](file:///C:/Users/to'm/AppData/Local/Temp/msohtmlclip1/01/clip_image007.jpg)

此程序的运行结果与在shell 中直接键入命令“ps -u”是一样的。

l execl()函数

接下来的示例使用完整的文件目录来查找对应的可执行文件。注意目录必须以“/”开头，否则将其视为文件名。

  **/\*execl.c\*/**  #include <unistd.h>  #include <stdio.h>  #include <stdlib.h>  #include <errno.h>  int main()  {  if (**fork()** == 0)  {  /*调用execl()函数，注意这里要给出ps 程序所在的完整路径*/  if (**execl("/bin/ps","ps","-u",NULL)**  < 0)  {  perror("Execl  error\n");  }  }  }  

运行结果同上例。

l execle()函数

下面的示例利用函数execle()，**给新建的子进程指定环境变量**，这里的“env”是**查看当前进程环境变量的命令**，如下所示：

  **/\* execle.c \*/**  #include <unistd.h>  #include <stdio.h>  #include <stdlib.h>  #include <errno.h>  int main()  {  /*命令参数列表，必须以NULL 结尾*/  char  *envp[]={"PATH=/tmp","USER=david", NULL};  if (**fork()** == 0)  {  /*调用execle()函数，注意这里也要指出env 的完整路径*/  if (**execle("/usr/bin/env",  "env", NULL, envp)** < 0)  {  perror("Execle  error\n");  }  }  }  

下载到目标板后的运行结果如下所示：

  $./**execle**                              

  PATH=/tmp                              

  USER=david                              

l execve()函数

execve()函数，通过构造指针数组的方式来传递参数，注意参数列表一定要以NULL 作为结尾标识符。其代码和运行结果如下所示：

  #include <unistd.h>  #include <stdio.h>  #include <stdlib.h>  **#include <errno.h>**  int main()  {  /*命令参数列表，必须以NULL 结尾*/  char *arg[] =  {"env", NULL};  char *envp[] =  {"PATH=/tmp", "USER=david", NULL};  if (**fork()** == 0)  {  if (**execve("/usr/bin/env",  arg, envp)** < 0)  {  perror("Execve  error\n");  }  }  }  

下载到目标板后的运行结果如下所示：

  $ ./execve                              

  PATH=/tmp                              

  USER=david                              

l execv()函数

execv()函数，和execve()一样，通过构造指针数组的方式来传递参数，参数列表一定要以NULL 作为结尾标识符，区别在于，execv()使用系统默认环境变量，不需要传递新环境变量。

  #include <unistd.h>  #include <stdio.h>  #include <stdlib.h>  **#include <errno.h>**  int main()  {  /*命令参数列表，必须以NULL 结尾*/  char *arg[] = {"ls","-l","/",  NULL};  if (**fork()** == 0)  {  if (**execv("/bin/ls",  arg)** < 0)  {  perror("Execve  error\n");  }  }  }  

此程序的运行结果与在shell 中直接键入命令“ls -a /”是一样的。

\4)   exec 函数族使用注意点。

在使用exec 函数族时，一定要加上错误判断语句。exec 很容易执行失败，其中最常见的原因有：

l 找不到文件或路径，此时errno 被设置为ENOENT，对应数值为2；

l 没有对应可执行文件的运行权限，此时errno 被设置为EACCES，对应数值为13。

【例子】使用execl()函数测试errno的情况。

  **/\*execl_errno.c\*/**  #include <unistd.h>  #include <stdio.h>  #include <stdlib.h>  #include <errno.h>  int main()  {  if (fork() == 0)  {  /*调用execlp()函数，这里相当于调用了"ps -u"命令*/  if ((execl("/bin/ps", "ps", "-u", NULL))  < 0)  {  perror("Execl  error\n");  printf("Errno is : %d\n",errno);  }  }  }  

【测试一】将execl("/bin/ps", "ps", "-u", NULL)改为execl(**"ps"**, "ps", "-u", NULL)，执行程序时将找不到ps命令，此时errno为ENOENT，对应数值为2：

![img](file:///C:/Users/to'm/AppData/Local/Temp/msohtmlclip1/01/clip_image009.jpg)

【测试二】将测试一所做的修改恢复，然后将ps命令“组外其他用户”的可执行权去掉，再切换到rjxy用户执行程序。rjxy在执行程序过程中需要调用ps命令，而ps命令将rjxy用户的可执行权删掉了，所以在执行过程中缺少对应的权限，此时errno为EACCES，对应数值为13：

![img](file:///C:/Users/to'm/AppData/Local/Temp/msohtmlclip1/01/clip_image011.jpg)

![img](file:///C:/Users/to'm/AppData/Local/Temp/msohtmlclip1/01/clip_image013.jpg)

 

  【小知识】  事实上，这6 个函数中真正的系统调用只有execve()，其他5个都是库函数，它们最终都会调用execve()这个系统调用。  

\3.    exit()和_exit()

\1)   exit()和_exit()函数说明。

exit()和_exit()函数都是用来终止进程的。当程序执行到exit()或_exit()时，进程会无条件地停止剩下的所有操作，清除包括进程控制块（PCB）在内的各种数据结构，并终止本进程的运行。但是，这两个函数还是有区别的，这两个函数的调用过程如下图所示：

![img](file:///C:/Users/to'm/AppData/Local/Temp/msohtmlclip1/01/clip_image015.jpg)

从图中可以看出，_exit()函数的作用是：直接使进程停止运行，清除其使用的内存空间，并清除其在内核中的各种数据结构；exit()函数则在这些基础上做了一些包装，在执行退出之前加了若干道工序。

**exit()****函数与****_exit()****函数最大的区别就在于****exit()****函数在调用****exit****系统调用之前要检查文件的打开情况，把文件缓冲区中的内容写回文件，就是图中的“清理****I/O****缓冲”一项。**

由于在Linux 的标准函数库中，有一种被称作“缓冲I/O（buffered I/O）”操作，如fread，其特征就是对应每一个打开的文件，在内存中都有一片缓冲区。每次读文件时，会连续读出若干条记录，这样在下次读文件时就可以直接从内存的缓冲区中读取；同样，每次写文件的时候，也仅仅是写入内存中的缓冲区，等满足了一定的条件（如达到一定数量或遇到特定字符等），再将缓冲区中的内容一次性写入文件。

这种技术大大增加了文件读写的速度，但也为编程带来了一些麻烦。比如有些数据，认为已经被写入文件中，实际上因为没有满足特定的条件，它们还只是被保存在缓冲区内，这时用_exit()函数直接将进程关闭，缓冲区中的数据就会丢失。因此，若想保证数据的完整性，就一定要使用exit()函数。

\2)   exit()和_exit()函数语法。

表5列出了exit()和_exit()函数的语法规范：

| 表5 exit()和_exit()函数族语法 |                                                              |
| ----------------------------- | ------------------------------------------------------------ |
| 所需头文件                    | exit：#include <stdlib.h>                                    |
| _exit：#include <unistd.h>    |                                                              |
| 函数原型                      | exit：void exit(int status)                                  |
| _exit：void _exit(int status) |                                                              |
| 函数传入值                    | status 是一个整型的参数，可以利用这个参数传递进程结束时的状态。一般来说，0 表示正常结束；其他的数值表示出现了错误，进程非正常结束。  在实际编程时，**可以用****wait()****系统调用接收子进程的返回值，从而针对不同的情况进行不同的处理** |

\3)   exit()和_exit()使用实例。

这两个示例比较了exit()和_exit()两个函数的区别。由于printf()函数使用的是缓冲I/O 方式，该函数在遇到“\n”换行符时自动从缓冲区中将记录读出。示例中就是利用这个性质来进行比较的。以下是示例1 的代码：

  /* exit.c */  #include <stdio.h>  #include  <stdlib.h>  int main()  {  **printf("Using exit...\n");**  **printf("This is the  content in buffer");**  **exit(0)****;**  **}**  

  **# ./exit**                             

  Using exit...                             

  This is the content in buffer                     

大家从输出的结果中可以看到，调用exit()函数时，缓冲区中的记录也能正常输出。

以下是示例2 的代码：

  /* _exit.c */  #include <stdio.h>  #include  <unistd.h>  int main()  {  printf("Using  _exit...\n");  **printf("This is the  content in buffer");** /* 加上回车符之后结果又如何 */  **_exit(0);**  }  

  **# ./_exit**                            

  Using exit...                             

从最后的结果中可以看到，调用_exit()函数无法输出缓冲区中的记录。

  【小知识】  在一个进程调用了exit()之后，该进程并不会立刻完全消失，而是留下一个称为僵尸进程（Zombie）的数据结构。僵尸进程是一种非常特殊的进程，它已经放弃了几乎所有的内存空间，没有任何可执行代码，也不能被调度，仅仅在进程列表中保留一个位置，记载该进程的退出状态等信息供其他进程收集，除此之外，僵尸进程不再占有任何内存空间。  

\4.    wait()和waitpid()

\1)   wait()和waitpid()函数说明。

wait()函数是**用于使父进程（也就是调用****wait()****的进程）阻塞，直到一个子进程结束或者该进程接到了一个指定的信号为止。**如果该父进程没有子进程或者他的子进程已经结束，则wait()就会立即返回。

waitpid()的作用和wait()一样，但它并不一定要等待第一个终止的子进程，它还有若干选项，如可提供一个非阻塞版本的wait()功能，也能支持作业控制。实际上wait()函数只是waitpid()函数的一个特例，在Linux内部实现wait()函数时直接调用的就是waitpid()函数。

\2)   wait()和waitpid()函数格式说明。

表6列出了wait()函数的语法规范。

| 表6 wait()函数族语法 |                                                              |
| -------------------- | ------------------------------------------------------------ |
| 所需头文件           | #include  <sys/types.h>  #include  <sys/wait.h>              |
| 函数原型             | pid_t wait(int  *status)                                     |
| 函数传入值           | 这里的status 是一个整型指针，是该子进程退出时的状态  l status若不为空，则通过它可以获得子进程的结束状态（是否正常结束），子进程的结束状态可由Linux 中一些特定的宏来测定；  l status若为空，表示不关心子进程的结束状态。 |
| 函数返回值           | 成功：已结束运行的子进程的进程号  失败:-1                    |

表7列出了waitpid()函数的语法规范。

| 表7 waitpid()函数语法                                  |                                                              |                                                              |
| ------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 所需头文件                                             | #include  <sys/types.h>  #include  <sys/wait.h>              |                                                              |
| 函数原型                                               | pid_t  waitpid(pid_t  pid,  int   *status,  int  options)    |                                                              |
| 函数传入值                                             | pid                                                          | pid > 0：只等待进程ID 等于pid 的子进程，不管是否有其他子进程运行结束退出，只要指定的子进程还没有结束，waitpid()就会一直等下去 |
| pid = -1：等待任何一个子进程退出，此时和wait()作用一样 |                                                              |                                                              |
| pid = 0：等待其组ID 等于调用进程的组ID 的任一子进程    |                                                              |                                                              |
| pid < -1：等待其组ID 等于pid 的绝对值的任一子进程      |                                                              |                                                              |
| status                                                 | 同wait()                                                     |                                                              |
| options                                                | WNOHANG：若由pid 指定的子进程当前不可用，则waitpid()不阻塞，此时返回值为0 |                                                              |
| 0：同wait()，阻塞父进程，等待子进程退出                |                                                              |                                                              |
| 函数返回值                                             | 正常：已经结束运行的子进程的进程号                           |                                                              |
| 使用选项WNOHANG 且没有子进程退出：0                    |                                                              |                                                              |
| 调用出错：-1                                           |                                                              |                                                              |

进程组ID可通过“ps -A -o pgrp=”命令查看所有进程的组ID，或者通过“ps -p 进程PID -o pgrp=”查看PID所对应进程的组ID；在程序中可通过getpgrp()函数或者getpgid(0)获取当前进程的组ID。

3）wait()和waitpid()使用实例。

wait()函数的使用较为简单。本例中首先使用fork()创建一个子进程，然后让其子进程暂停5s（使用sleep()函数）。接下来对原有的父进程使用wait()函数，父进程阻塞。等子进程退出，则父进程继续执行。

该程序源代码如下所示：

  /* wait.c */  #include <sys/types.h>  #include <sys/wait.h>  #include <unistd.h>  #include <stdio.h>  #include <stdlib.h>  int main()  {  pid_t pc;  pc = fork();  if (pc < 0)  {  printf("Error fork\n");  }  else if (pc == 0) /*子进程*/  {  /*子进程暂停5s*/  sleep(5);  /*子进程正常退出*/  exit(0);  }  else /*父进程*/  {  /*调用wait，父进程阻塞*/  printf("before  wait\n");  wait(0);  printf("father  continue...\n");  sleep(5);  }  }     

该程序运行结果如下所示：

\#./**wait**

before wait

father continue...

waitpid()为例进行讲解。本例中首先使用fork()创建一个子进程，然后让其子进程暂停5s（使用sleep()函数）。接下来对原有的父进程使用waitpid()函数，并使用参数WNOHANG使该父进程不会阻塞。若有子进程退出，则waitpid()返回子进程号；若没有子进程退出，则waitpid()返回0，并且父进程每隔一秒循环判断一次。该程序的流程图如下图所示：

![img](file:///C:/Users/to'm/AppData/Local/Temp/msohtmlclip1/01/clip_image017.jpg)

该程序源代码如下所示：

  /* **waitpid.c** */  #include <sys/types.h>  #include <sys/wait.h>  #include <unistd.h>  #include <stdio.h>  #include <stdlib.h>  int main()  {  pid_t pc, pr;  pc = **fork()**;  if (pc < 0)  {  printf("Error  fork\n");  }  else if (pc == 0) /*子进程*/  {  /*子进程暂停5s*/  sleep(5);  /*子进程正常退出*/  exit(0);  }  else /*父进程*/  {  /*循环测试子进程是否退出*/  do  {  /*调用waitpid，且父进程不阻塞*/  pr = **waitpid(pc, NULL,  WNOHANG)**;  /*若子进程还未退出，则父进程暂停1s*/  if (pr == 0)  {  printf("The child  process has not exited\n");  sleep(1);  }  } while (pr == 0);  /*若发现子进程退出，打印出相应情况*/  if (pr == pc)  {  printf("Get child exit  code: %d\n",pr);  }  else  {  printf("Some error  occured.\n");  }  }  }  

该程序运行结果如下所示：

\#./**waitpid**

The child process has not exited

The child process has not exited

The child process has not exited

The child process has not exited

The child process has not exited

Get child exit code: 75

可见，该程序在经过5 次循环之后，捕获到了子进程的退出信号，具体的子进程号在不同的系统上会有所区别。

如果把“pr = waitpid(pc, NULL, WNOHANG);”这句改为“pr = waitpid(pc, NULL, 0);”或者“pr= wait(NULL);”，运行的结果为：

$./**waitpid**

Get child exit code: 76

可见，在上述两种情况下，父进程在调用waitpid()或wait()之后就将自己阻塞，直到有子进程退出为止。

------



Int类型



perror(s) 用来将上一个函数发生错误的原因输出到标准设备(stderr)。参数s所指的字符串会先打印出，后面再加上错误原因字符串。此错误原因依照全局变量errno的值来决定。








# 4. 系统目录导览

对于以下目录，你可以：
- `cd` 到目录
- `ls` 查看目录内容
- `file` 查看文件内容
- `less` 查看文本文件

## /
系统根目录，大部分情况下该目录中只有子目录。


## /boot
Linux 内核启动加载文件的目录。内核文件名为 *vimlinuz*


## /etc
包含系统的配置文件。所有文件都应该是文本文件。例如：

*/etc/passwd*
包含每个用户的信息。所有用户都定义在此。

*/etc/fstab*
系统启动时加载的设备列表。定义了你的磁盘驱动。

*/etc/hosts*
包含网络主机名和 IP 地址。

*/etc/init.d*
该目录包含了启动各种系统服务的脚本。


## /bin，/usr/bin
包含系统大部分的程序。*/bin* 目录下的为系统运行所必须的程序，*/usr/bin* 包含了系统用户的应用。


## /sbin，/usr/sbin
*sbin* 目录包含了系统超级管理员使用的程序。


## /usr
*/usr* 目录包含了各种支持用户应用的东西。

*/usr/share/X11*
X Window 系统支持文件

*/usr/share/dict*
拼写检查的字段。参考 [look](http://linuxcommand.org/lc3_man_pages/look1.html) 和 [aspell](http://linuxcommand.org/lc3_man_pages/aspell1.html)

*/usr/share/doc*
各种格式的各种文档文件。

*/usr/share/man*
帮助页面，1~9

*/usr/src*
源码文件。如果你安装了内核源码包，你会在该目录找到整个 Linux 内核的源码。


## /usr/local
*/usr/local* 及其子目录用于安装本地机器使用的软件和其他文件。非官方发行版的软件都在这里（官方的通常在 */usr/bin*）。
当你想安装你感兴趣的程序到系统时，安装到 */usr/local*。大部分情况下会安装到 */usr/local/bin*。


## /var
*/var* 目录包含当前系统运行变更的文件。包括：

*/var/log*
包含日志的目录。系统运行时更新。时不时观察一下该目录下的文件来监控系统的健康。

*/var/spool*
该目录保存一些进程的队列文件，例如邮件消息和打印任务。当本地系统收到用户的邮件时，消息会首先存储到 */var/spool/mail*


## /lib
存放共享库（类似 Windows 的 DDL）。


## /home
用户的个人工作空间。通常情况下只有用户自己允许写文件。


## /root
超级用户的家目录。


## /tmp
程序写入临时文件的目录。


## /dev
*/dev* 是一个特殊目录，它通常不包含文件。而是系统可用的设备。在 Linux 系统中，设备也是文件。你可以对设备进行读写。例如 */dev/fd0* 为第一个软盘的驱动，*/dev/sda*（老系统为 */dev/hda*）为第一个硬盘驱动。所有内核识别的设备都在此。


## /proc
*/proc* 目录也很特殊。它不包含文件。实际上，该目录并不存在。它是虚拟的。尝试查看 */proc/cpuinfo* 来获取你的 CPU 信息。


## /media，/mnt
*/media* 目录用于挂载点。添加设备到目录树的过程叫 **挂载**。如果设备要可用，那么必须先挂载。
当系统启动时，会先读取 */etc/fstab* 中的挂载指导，其中描述了每个设备挂载到哪个目录树的哪个点。有些如 CD-ROM、U 盘等设备是可移除的，不会一直挂载。*/media* 用于自动挂载。*/mnt* 用于设备的临时挂载。你会经常看到 */mnt/floppy* 和 */mnt/cdrom*。要查看哪些设备和挂载点被使用了，输入 [mount](http://linuxcommand.org/lc3_man_pages/mount8.html)。


## 奇怪的文件类型：符号链接
> 在以上目录中，你可能注意到有些奇怪的目录，特别是在 */boot* 和 */lib* 目录。当使用 `ls -l` 命令查看时，你可能看到：

```
lrwxrwxrwx 25 Jul 3 16:42 System.map -> /boot/System.map-2.0.36-3
-rw-r--r-- 105911 Oct 13 1998 System.map-2.0.36-0.7
-rw-r--r-- 105935 Dec 29 1998 System.map-2.0.36-3
-rw-r--r-- 181986 Dec 11 1999 initrd-2.0.36-0.7.img
-rw-r--r-- 182001 Dec 11 1999 initrd-2.0.36.img
lrwxrwxrwx 26 Jul 3 16:42 module-info -> /boot/module-info-2.0.36-3
-rw-r--r-- 11773 Oct 13 1998 module-info-2.0.36-0.7
-rw-r--r-- 11773 Dec 29 1998 module-info-2.0.36-3
lrwxrwxrwx 16 Dec 11 1999 vmlinuz -> vmlinuz-2.0.36-3
-rw-r--r-- 454325 Oct 13 1998 vmlinuz-2.0.36-0.7
-rw-r--r-- 454434 Dec 29 1998 vmlinuz-2.0.36-3
```

注意文件 *System.map*、*module-info* 和 *vmlinuz*。看到文件名后面的内容了吗？
这些都叫做 **符号链接**。符号链接是一种指向其他文件的特殊类型的文件。有了符号链接，一个文件就可以拥有多个名字了。它的工作原理是：当给定的文件是一个符号链接，它透明地将其映射到它指向的文件。
这有什么好处呢？这是一个很方便的功能。以上面为例，如果系统安装了多个版本的 Linux 内核，例如上面的 *vmlinuz-2.0.36-0.7* 和 *vmlinuz-2.0.36-3*，这时我们会很迷惑，不知道用的是哪个内核，而且我们可能想要简单的将内核命名为 *vmlinuz*。这时符号链接的美妙之处就显现出来了。通过将 *vmlinuz* 指向 *vmlinuz-2.0.36-3*，我们解决了这个问题。
要创建符号链接，使用 [ln](http://linuxcommand.org/lc3_man_pages/ln1.html) 命令。



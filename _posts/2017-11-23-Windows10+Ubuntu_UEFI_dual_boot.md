---
layout: post
title: Windows10 + Ubuntu16.04双系统安装教程
categories: 操作系统
description: 安装双系统
keywords: windows10，ubuntu16
---

**Windows10 加 Ubuntu16.04** 双系统 安装指南（UEFI启动）-- 血与泪的教训

## 经验必读
2017年的感恩节，不仅吃了火鸡🦃️，我还尝试了给👽做双系统。一切都来源于感恩节的礼物，一块nvme m.2 ssd, 想把已存在的win10系统migrate到ssd上，避免重新安装，经历的以下的的坑。
* 使用samsumg家提供的data migration软件，成功复制的系统，所以win10在ssd上
* 然后我开始安装ubuntu，由于还有一块hdd，将其格式化，并且分出了100G的unallocated space给ubuntu
* 按照网上某教程开始安装，（已制作了一个usb 启动盘，使用[Rufus](https://rufus.akeo.ie/))，在安装过程的第三步选择something else，自己使用上述unalloate space分配 ／；／home等挂在空间，然而，貌似分配方式并不对，而且system boot loader的选择也不正确，导致虽然安装成功了，出现了让人无比困惑的问题
* 问题是：开机需要用F12选择boot的系统。如果选择win10，载入速度非常慢；如果选择ubuntu，很大概率grub2无法加载到可boot的系统，进入黑屏command line模式，对于此模式，网上很多有关的fix教程，但是均不适用于我的情况，花费巨大的精力尝试和分析，无果；如果让系统自动boot，一定进入grub command line模式，绝望。。。。

## 正确安装前准备
* 电脑：一块256G ssd， 一块1TB hdd。 hdd并没有发挥作用，因为双系统都将安装在前者上。win10: 170G 空间； ubuntu：80G 空间, unallocated未分配状态.
* 已安装好window系统，使用usb安装，系统来源microsoft官网。切记！！！**同一块硬盘安装，至少我成功了**
* 给window存在的硬盘，即ssd，分配出额外的空间安装ubuntu


## 开始安装
此处参考：[亲测UEFI启动模式的电脑安装Win10和Ubuntu双系统](http://blog.csdn.net/ysy950803/article/details/52643737)，感谢作者，节省时间，豁然开朗。

#### 提示
最新提示：双硬盘（固态+机械，并且原Windows的引导盘在固态）要装双系统，此文不适用（否则会出现安装完Ubuntu后看不到grub菜单或者搞出来grub菜单后看不到Windows Boot Manager选项，因为你把Ubuntu的引导装在了机械硬盘，和Win的引导不在一个盘。
我们可以发现， ubuntu 16的iso文件中本身就含有efi文件夹，说明其支持UEFI引导。
![](/images/posts/ubuntu_efi.png)

#### 步骤
1. 关闭Windows的快速启动。在控制面板中的电池设置中.
2. 进入BIOS设置，关闭Security Boot
3. 使用usb启动，进入ubuntu安装界面
4. 设置swap交换空间，这个也就是虚拟内存的地方，选择 **主分区** 和 **空间起始** 位置。如果你给Ubuntu系统分区容量足够的话，最好是能给到你物理内存的2倍大小，像我8GB内存，就可以给个16GB的空间给它，这个看个人使用情况，太小也不好，太大也没用。（其实我只给了8GB，没什么问题）
5. 新建efi系统分区，选中 **逻辑分区**（这里不是主分区，请勿怀疑，老式的boot挂载才是主分区）和 **空间起始** 位置，大小最好不要小于256MB，系统引导文件都会在里面，我给的512MB，它的作用和boot引导分区一样，但是boot引导是默认grub引导的，而efi显然是UEFI引导的。不要按照那些老教程去选boot引导分区，也就是最后你的挂载点里没有“/boot”这一项，否则你就没办法UEFI启动两个系统了。
6. 挂载“/home”，类型为 **EXT4日志文件系统**，选中 **逻辑分区** 和 **空间起始** 位置，这个相当于你的个人文件夹，类似Windows里的User，如果你是个娱乐向的用户，我建议最好能分配稍微大点，因为你的图片、视频、下载内容基本都在这里面，这些东西可不像在Win上面你想移动就能移动的。
总的来说，最好不要低于8GB，我Ubuntu分区的总大小是64GB，这里我给了12GB给home。
（这里特别提醒一下，Ubuntu最新发行版不建议强制获取Root权限，因为我已经玩崩过一次。所以你以后很多文档、图片、包括免安装软件等资源不得不直接放在home分支下面。你作为图形界面用户，只对home分支有完全的读写执行权限，其余分支例如usr你只能在终端使用sudo命令来操作文件，不利于存放一些直接解压使用的免安装软件。因此，建议home分支多分配一点空间，32GB最好……。
7. 挂载“/usr”，类型为 **EXT4日志文件系统** ，选中 **逻辑分区** 和 **空间起始** 位置，这个相当于你的软件安装位置，Linux下一般来说安装第三方软件你是没办法更改安装目录的，系统都会统一地安装到/usr目录下面，因此你就知道了，这个分区必须要大，我给了32GB。
8. 最后，挂载“/”，类型为 **EXT4日志文件系统** ，选中 **逻辑分区** 和 **空间起始** 位置，
因为除了home和usr还有很多别的目录，但那些都不是最重要的，“/”就把除了之前你挂载的home和usr外的全部杂项囊括了，大小也不要太小，最好不低于8GB。如果你非要挨个仔细分配空间，那么你需要知道这些各个分区的含义（Linux(ubuntu)分区挂载点介绍）
不过就算你把所有目录都自定义分配了空间也必须要给“/”挂载点分配一定的空间。
9. 分配好各个挂载点后，还有一个至关重要的步骤，那就是选择“**安装引导启动器的设备**”，默认是错误的，既然我们为Ubuntu分配了efi系统引导分区，那么显然，这里应该把它改成刚刚第2步分配efi引导的那个分区（比如我安装时它是/dev/sda7，那么我就选这个）
10. 开始安装，等待安装完成。
11. 安装成功后，会提示你拔掉U盘并且重启，重启后记得进入BIOS改回UEFI Security Boot On模式，也就是重新开启Security Boot，然后再重启你就可以看到选择系统的启动引导界面了。
12. 可能会出现两个系统时间不同的情况，解决办法有两个，我用了第一个。参考 [怎样解决Windows10时间快和Ubuntu时间差问题？](https://www.zhihu.com/question/46525639?sort=created)
* 在Ubuntu中把计算机硬件时间改成系统显示的时间，即禁用Ubuntu的UTC。这又有另一个需要注意的地方：在 Ubuntu 16.04 版本以前，关闭UTC的方法是编辑/etc/default/rcS，将UTC=yes改成UTC=no， 但在Ubuntu 16.04使用systemd启动之后，时间改成了由timedatectl来管理，所以更改方法是:
```
timedatectl set-local-rtc 1 --adjust-system-clock
```
执行后重启到win10，可能时间还不对，可以手工修改一下
* 2.修改 Windows对硬件时间的对待方式，让 Windows把硬件时间当作UTC.
打开命令行程序，在命令行中输入下面命令并回车：
```
Reg add HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
```
13. cut，收工

#### 折腾是痛苦的，折腾完是畅快的。

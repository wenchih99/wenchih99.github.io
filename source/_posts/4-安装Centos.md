title: 安装Centos.md
author: WENCHIH CHANG
author_id: wenchih99@github.com
date: 2020-12-02 10:56:46
tags: linux
language:
archives:
categories: [linux,Linux规则与安装]
---
# 安装过程
<!--more-->
- qcow2虚拟磁盘格式:用多少记录多少
- 选择安装模式与启动
  - 强制使用gpt分区表,在内核参数下添加inst.gpt
  - 当笔记本安装系统失败时,在内核参数下加入nofb apm=off acpi=off pci=noacpi
    - nofb是取消显卡上面的缓存检测
    - APM(Advanced Power Management):早期电源管理模块
    - ACPI(Advanced Configuration and Power Interface):最近电源管理模块
    - 二者都是由硬件本身支持的
- 分区类型
  - 标准分区,直谈、常谈的分区
  - LVM:这是一种可以弹性增加或缩小文件系统容量的分区,分配固定的容量
  - LVM精简配置(Thin Provisioning):高级版,使用多少容量分配多少容量
- 文件系统
  - ext2/ext3/ext4:3、4文件系统多了日志功能,系统恢复比较快速,随磁盘容量越来越大,ext文件系统有点挡不住了
  - swap:磁盘模拟为内存的交换分区,主要存物理内存中较少使用的数据,将空间释放给真正需要的程序
  - BIOS Boot:gpt分区表可能会使用的东西
  - xfs:格式化较快,对大容量磁盘管理非常好
  - vfat:Linux与Windows系统均支持的文件系统类型,可进行数据交换
- 虚拟机40G分区配置
  - 制作gpt分区表,要有BIOS boot分区,2M即可,无挂载点
  - 建立/boot挂载点的文件系统,1G
  - /、swap、/home的设备类型均为LVM
    - 修改卷组大小策略为Fixed,30G
  - 建立/挂载点的根目录,10G
  - /home,5G
  - 要点选重新格式化,才能格式化成你所需要的文件系统
- 内核管理
  - 系统分类下KDUMP:一项当系统因为内核问题导致的宕机事件时会将该宕机事件的内存中的数据保存的功能,适合开发者除错
- 进行电脑前的初配置,包括分区、root密码等,会被记录到/root/anaconda-ks.cfg文件内
- ios镜像刻录的启动盘不仅可以装系统,还可以进行一些恢复、烧机等任务
  - 内存压力测试:memtest86
    - Troubleshooting->Run a memory test
  - 恢复MBR内启动引导程序
    - Troubleshooting->Rescue a CentOS system

# 多重引导(Linux引导Windows)
- 先分好区:可以用命令来创建分区,用`ctrl`+`alt`+`F2`进入命令行,使用parted命令
- 恢复MBR内的启动引导程序与设置多重引导
  - 恢复MBR
  - 在Linux系统下修改启动选项任务
- 多重引导后续维护
  - 在Windows系统中最好将根目录和swap取消挂载,以防不小心在Windows中将其格式化
  - Linux系统不能轻易删除,否则Windows也会挂掉的,因为是Linux引导Windows,grub会读取Linux系统的内容
title: Linux磁盘与文件系统管理
author: WENCHIH CHANG
author_id: wenchih99@github.com
date: 2020-12-02 10:56:46
tags: linux
language:
archives:
categories: [linux,Linux文件、目录与磁盘格式]
---
# 认识Linux文件系统
<!--more-->
- 文件系统特性
  - 超级区块(superblock):记录此文件系统的整体信息,包括inode与数据区块的总量、使用量、剩余量,以及文件系统的格式与相关信息等
  - inode:记录文件的属性,一个文件占用一个inode,同时记录此文件的数据所在的区块号码
  - 数据区块(block):实际记录文件的内容,若文件太大时,会占用多个区块
- Linux的ext2文件系统(inode)
  - 文件系统格式化时就将inode和数据区块规划好
  - inode与数据区块通通放置在一起不容易管理,从而ext2文件系统在格式化的时候就区分为多个区块群组(block group),每个区块群组都有独立的superblock,文件系统描述、区块对应表(block bitmap)、inode对应表(inode bitmap)、inode table,data block
  - 文件系统最前面有一个启动扇区(boot sector),这个启动扇区可以安装启动引导程序,便于制作出多重引导的环境
  - 查询ext系列超级区块信息的命令:`dumpe2fs`
- 与目录树的关系
  - 当建立一个目录时,文件系统会分配一个inode与至少一块区块给该目录
- ext2/ext3/ext4文件的存取与日志式文件系统的功能
  - inode对照表与数据区块称为数据存放区域,超级区块、区块对照表与inode对照表等区段被称为元数据(metadata)。因为超级区块、区块对照表及inode对照表的数据是经常变动的,每次新增、删除、编辑时都可能会影响到这三个部分的数据,因此称为元数据
  - sync:强制将内存中dirty文件写入磁盘
- 挂载点的意义(mount point)
  - 挂载点一定是目录,该目录为进入该文件系统的入口
- 其他Linux支持的文件系统
  - 传统文件系统:ext2、minix、FAT(用vfat模块)、iso9660(光盘)等
  - 日志式文件系统:ext3、ext4、ReiserFS、Windows'NTFS、IBM's JFS、SGI's XFS、ZFS
  - 网络式文件系统:NFS、SMBFS
- Linux VFS(Virtual Filesystem Switch)
  - 帮助我们读取、管理文件系统,便于管理、读取文件
- XFS文件系统介绍
  - 被开发用于高容量磁盘以及高性能文件系统之用
  - XFS文件系统在数据的分布:数据区(data section),文件系统活动登录区(log section),实时运行区(realtime section)
  - 数据区:与区块群组类似,分为多个存储区群组(allocation groups,AG),与ext系统区别就是inode与区块动态产生
  - XFS文件系统的描述数据观察:`xfs_info`

# 文件系统的简单操作
- 磁盘与目录的容量
  - `df`:列出文件系统的整体磁盘使用量
  - `du`:查看文件系统的磁盘使用量
  - 硬链接(Hard Link,硬式链接或实际链接)
    - 文件名只与目录有关,文件内容与inode有关
    - 硬链接只是在某个目录下新增一条文件名链接到某inode号码的关联记录而已
    - 限制:不能跨文件系统、不能链接目录
  - 符号链接(Symbolic Link,亦即是快捷方式)
    - 符号链接就是建立一个独立的文件,而这个文件会让数据的读取指向它链接的那个文件的文件名
  - ln:制作链接文件

# 磁盘的分区、格式化、检验与挂载
- 观察磁盘分区状态
  - `lsblk`:列出系统上所有磁盘列表
  - `blkid`:列出设备的UUID等参数(全局唯一标识符:universally unique identifier),UUID可以拿来作为挂载或是使用这个设备或文件系统
  - `parted`:列出磁盘的分区表类型与分区信息
- 磁盘分区:`gdisk/fdisk`
- 更新分区表:`partprobe`
- 查看内核的分区记录:`cat /proc/partitions`
- 磁盘格式化(创建文件系统):make filesystem(mkfs)
  - 格式化XFS文件系统:`mkfs.xfs`
- XFS文件系统for RAID性能优化(Optional)
  - 磁盘阵列:RAID
  - 分区区块:stripe
  - 校验磁盘:parity disk
  - 备用磁盘:spare disk
- 查询mkfs格式化哪种文件系统:`mkfs[tab][tab]`
- 文件系统检验
  - 被检查的硬盘分区务必不可挂载到系统上
  - `xfs_repair`处理XFS文件系统
  - `fsck.ext4`处理ext4文件系统
- 文件系统挂载与卸载
  - 挂载系统:`mount`
  - 卸载设备文件:`umount`
  - 挂载光盘时,光盘只有卸载后才能退出
  - 挂载系统不支持的文件系统时,需要下载相应驱动程序
  - 重新挂载根目录:`mount -o remount,rw,auto /`
  - 挂载不特定目录:`mount --bind 源目录 目标目录(这个就和硬链接一样)`
  - 卸载时可用卸载设备文件名、挂载点,卸载不特定目录时一定要用卸载挂载点
- 磁盘/文件系统参数的自定义
  - `mknod`:手动处理设备文件
  - `xfs_admin`:查询修改XFS文件系统的UUID与Label name
  - `tune2fs`:查询修改EXT4文件系统的UUID与Label name
  - `uuidgen`:产生新的UUID

# 设置启动挂载
- 启动挂载/etc/fstab及/etc/mtab
- fstab(filesystem table)
  - [设备/UUID等] [挂载点] [文件系统] [文件系统参数] [dump] [fsck]
  - 第五栏:能否被dump备份命令作用
  - 第六栏:是否以fsck检验扇区
- 特殊设备loop挂载(镜像文件不刻录就挂载使用)
- 挂载CD/DVD镜像文件
  - 挂载时需要mount的特殊参数,就是`-o loop`
  - 建立大型文件:`dd`
  - 大型文件格式化:`mkfs.xfs`必须加上[-f]参数

# 内存交换分区(swap)之创建(现在对于服务器来说比较重要)
- 使用物理分区创建内存交换分区
  - 分区:使用`gdisk`划分出一个分区,并设置一下system ID=8200
  - 格式化:`mkswap 设备文件名`
  - 使用:`swapon 设备文件名来启动设备`
  - 观察:`free`可查看物理内存与swap的使用情况;`swapon -s`可观察目前使用的内存交换分区的设备有哪些
- 使用文件创建内存交换文件(物理内存不足时)
  - 使用`dd`命令新建文件
  - 使用`mkswap`格式化上面产生的文件
  - 使用`swapon`来启动设备,`swapoff`来关闭设备
  - 注:系统仅会查询区块设备(block device)不会查询文件,所以一些配置文件时,不要用UUID,用设备名,例子:/etc/fstab

# 文件系统的特殊观察与操作
- 利用GNU的`parted`进行分区操作
  - 此命令同时支持MBR/GPT
  - `parted [设备] [命令 [参数]]`
  - 命令功能
    - 新增分区:`mkpart [primary|logical|Extended] [ext4|vfat|xfs] 开始 结束`
    - 显示分区:`print`
    - 删除分区:`rm [partition]`
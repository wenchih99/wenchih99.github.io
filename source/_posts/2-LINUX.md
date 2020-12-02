title: LINUX
author: WENCHIH CHANG
author_id: wenchih99@github.com
date: 2020-12-02 10:56:46
tags: linux
language:
archives:
categories: [linux,Linux规则与安装]
---
# Linux是什么
<!--more-->
- Linux就是一组软件,一组操作系统
- Linux是开源,具有可移植性
- 软件移植:参考硬件的功能函数并以此修改操作系统程序代码,经过改版后的操作系统能够在另一个硬件平台上运行的过程
- Linux之前,UNIX历史
  - 1969年以前(Bell、MIT、GE的Multics系统)
    - 批处理型操作系统:输入设备只有读卡机,输出设备只有打印机,用户无法与操作系统互动
    - 兼容分时系统(Compatible Time-Sharing System,CTSS)
      - 让大型主机通过提供数个终端以连接进入主机,利用主机的资源进行运算工作
      - 终端仅有输入输出功能,并无相关软件与计算能力
      - 为了强化此功能,1965年前后发起'Multics'计划
  - 1969年,Ken Thompson的小型file server system
    - UNIX原型:以汇编语言写出的一组内核程序,同时包括一些内核工具程序,以及一个小小的文件系统
    - 文件系统:
      - 所有程序或系统设备都是文件
      - 不管程序本身还是附属文件,所写的程序只有一个目的,且要有效的完成目标
  - 1973年,Ritchie等人用C语言写出第一个正式UNIX内核
    - C语言为高级语言,与硬件的相关性没有汇编语言大,因此容易被移植
    - UNIX强调多人多任务环境,UNIX为专利软件,但在架构上比较开放
  - 1977年,UNIX分支-BSD诞生(Berkeley Software Distribution)
    - 可安装在X86硬件架构上面的FreeBSD即是BSD改版而来
    - 早先UNIX只能与服务器或是大型工作站划上等号
  - 1979年,重要System V架构与版权声明
    - System V第七版UNIX可以支持X86架构的个人计算机
    - 目前被称为"纯种的UNIX"指的就是System V以及BSD两套软件
  - 1984年
    - X86架构的Minix操作系统开始编写并于两年后诞生
      - Minix用于教学,用户要求和需求可能无法上升到比较高的程度
    - GNU计划与FSF基金会的成立
      - 目的:建立一个自由、开放的操作系统,
      - 推出免费的GNU软件
        - GNU C Compiler(gcc)
        - 自由软件基金会(Free Software Foundation,FSF)
        - GNU C library
        - 运行操作系统的基本接口(Bash shell)
  - 1985年,草拟通用公共许可证(General Public License,GPL)
  - 1988年,图形用户界面模式XFree86计划
    - XFree86 = X Window System + Free + X86
    - 图形用户接口(Graphical User Interface,GUI)
  - 1991年,Linus Torvalds用bash,gcc等GNU工具写出一个内核程序
- 软件分类
  - 自由软件:用户可以自由的执行、复制、再发行、学习、修改与强化自由软件,但不可修改授权(GPL)或单纯销售.销售的话必须搭配售后服务与相关手册
  - 开源软件:无限制,自由软件是开源软件的一种
  - 闭源软件:仅推出可执行二进制程序
  - Freeware:免费软件,Free Ware:自由软件
  - Shareware:可试用的付费软件
- Linux发展(UNIX-like)
  - POSIX:可移植操作系统接口(Portable Operating System Interface)
  - Linux发展,还需要虚拟团队的帮助
  - Linux内核版本
    ```
    3.10.0-123.e17.x86-64
    主版本.此版本.发布版本-修改版本
    ```
    - 3.0版以前
      - 主、次版本为奇数:开发中版本
      - 主、次版本为偶数:稳定版本
    - 3.0版以后
      - 主线版本:MainLine
      - 旧版本
        - 结束开发(End of Live,EOL)
        - 长期维护版本(Longterm)
  - Linux发行版:内核+自由软件+文档(工具)+可完全安装程序
  - 不同Linux发行版的异同
    - 相同 
      - 标准规范:Linux Standard Base(LSB)
      - 目录结构标准:File system Hierarchy Standard(FHS)
    - 相异
      - 安装软件方式
        - RPM方式:Red Hat、Fedora、SUSE
        - DPKG方式:Debian、Ubuntu、B2D
  - 互联网服务提供商(ISP)
  - Tarball(源代码安装包)

# 如何学习
- **兴趣**
- **成就感**
- **实践加讨论**
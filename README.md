# MIT6.828 Operating System Engineering
在很久以前听说过这门操作系统的好课，但是迟迟没能开始，最近开始填坑了。花了一个多月的时间目前完成了前两个 lab, 不得不说这门课程非常硬核，能够学到的东西很多，一个雏形的操作系统构建过程会让你对操作系统的印象越来越深，这个过程是循序渐进的，一步一步教你如何建立或理解一个操作系统。学完这个雏形后，你也能够拥有一个属于你自己的完整内核雏形。


# 1. 课程需知
课程网址： [mit 6.828: Operating System Engineering](https://pdos.csail.mit.edu/6.828/2018/schedule.html), schedule 是课程安排，可以按照这个进度走。

xv6: 或许会在 mit6.828 官网看到它和 jos, 他们之间的关系是 xv6 是一个类 Unix 教学的操作系统， jos 是在这个 xv6 基础上改写的，并设置了一些 lab 让我们实验完成，当遇到不会的问题时，可以参考一下 xv6 相应部分的源码。

qemu: PC 模拟软件，使用 qemu 可以模拟一台电脑。我们用 qemu 装载 jos 就可以运行测试实验操作系统。

实验环境：
- Ubuntu (我使用的 Ubuntu 16.04， 虚拟机就可以满足要求)
- qemu, 官方安装教程参考 [官网教程](https://pdos.csail.mit.edu/6.828/2018/tools.html), 中文教程参考博客园的 [这篇博客](https://www.cnblogs.com/gatsby123/p/9746193.html)
- 编译工具链，官方安装教程参考 [官网教程](https://pdos.csail.mit.edu/6.828/2018/tools.html), 中文教程参考 qemu 提到的 [博客](https://www.cnblogs.com/gatsby123/p/9746193.html)

本课程会分为若干 lab, 每个 lab 对应代码的一个分支，每个分支对照着 [labs (进入链接后找到页面顶部菜单栏的 labs, 选择对应的 lab)](https://pdos.csail.mit.edu/6.828/2018/labguide.html) 做。如果对官方英文阅读有些困难，请参考我的`实验总结` (以中文记录)，位于 `report` 目录下，记录了 lab 具体要求和实验过程。

每在一个 lab 分支完成代码后, 使用如下命令查看自己的实验结果分数:
```make
make grade
```

拿到自己满意的分数后，切换到其他 lab 分支继续完成剩余的 lab。切换命令如下 (这里以做完 lab2 为例, 切换到 lab3)：
```git
git checkout lab3
```

# 2. 快速上手
本仓库是基于 [mit6.828 官方仓库](https://pdos.csail.mit.edu/6.828/2018/jos.git) 完成的，增添了自己的实验代码和实验总结
```txt
# 项目结构
labx
├── boot 
├── conf 
├── fs
├── inc 
├── kern 
├── lib 
├── report # 实验总结
└── user
```

详细参考手册：

1. [Lab1 Booting a PC](./lab1/report/lab1_summary.pdf)
2. [Lab2 Memory management](./lab2/report/lab2_summary.pdf)
3. User-Level Environments (正在完成中...)
4. Preemptive Multitasking (正在完成中...)
5. File system, Spawn and Shell (正在完成中...)
6. Network Driver (正在完成中...)

官方 lab 使用 gdb 调试，为了便于文档说明，本项目使用了 cgdb 完成实验总结。如果使用 gdb 调试则可以跳过` cgdb 使用`部分

---
`cgdb 使用`

需要使用 cgdb 调试，以 lab 1 为例，可以在 `lab1/GNUmakefile` 中 gdb 目标改为：
```
gdb:
#	gdb -n -x .gdbinit
	i386-jos-elf-cgdb -n -x .gdbinit
```
即可使用 cgdb 开始调试 qemu-jos

cgdb 在 ubuntu 安装推荐使用源码编译的方式，通过 apt 源安装的 cgdb 版本较低，同时 cgdb 安装后可能会出现 gdb 版本较低不兼容，这时再安装一个高版本的 gdb 就可以了
```
cgdb 源码安装过程：
1. 安装依赖
sudo apt install automake
sudo apt install ncurses-devel
sudo apt install flex
sudo apt install textinfo
sudo apt install libreadline-dev

2. 下载源码开始编译
git clone https://github.com/cgdb/cgdb.git
cd cgdb
./autogen.sh
./configure --prefix=/usr/local --target=i386-jos-elf --program-prefix=i386-jos-elf- --disable-werror
make all                 # 编译
make install             # This step may require privilege (sudo make install) 安装
```



# 3. Q&A
1. 零基础可以学这门课吗？
万事开头难，这门课要求一些汇编、C语言、计算机组成原理。如果想学好这门课程，肯定需要学习一下刚才提到的前置课程，当然只需要最基础的部分学会就好了，然后坚持跟着这个课程走就可以学到很多书本上不告诉你的知识。还有本课程的 lab 并不要求你完全把一个 lab 做完才能做下一个，因此遇到 lab 难度特别大的某个点，可以先跳过，因此本门课程并不要求学习者完全独立地完成整个课程，目的在于培养学习者对操作系统的整体认识。如果想要学习这门课就现在吧，因为学习的最好时候就是现在。

2. 6.828 和 6.s081 有什么区别？
2018 年前只有 6.828, 2018 年后出现了 6.s081。2018 年之后 mit6.828 主要面向研究生，6.s081 主要面向本科生
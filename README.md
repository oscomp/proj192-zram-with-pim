# proj192-zram-with-pim
## 题目
使用支持存内计算的内存设备，修改LINUX中内存压缩功能（ZRAM|ZSWAP）的实现。将压缩部分的工作从CPU执行转移到协处理器执行。

## 项目描述
### LINUX ZRAM内存压缩机制
zram，以前称为compcache，是一个Linux内核模块，用于在RAM中创建压缩块设备，即具有动态磁盘压缩的"RAM磁盘"。然后，使用zram创建的块设备可用于交换或用作通用 RAM 磁盘。zram 的两个最常见的用途是存储临时文件(/tmp)和作为交换设备。最初，zram只有后一个功能，因此原名“compcache”（压缩缓存）。

### PIM 存内计算
 process-in-memory(PIM)最早发源于计算机微体系结构的研究，通过在内存中集中部分计算资源，实现快速的数据访问，主要解决访存带宽占用过多、访存能耗等问题。并随着新型硬件(HBM、HMC)的提出，process-in-memory扩展到电路研究领域的compute-in-memory、logic-in-memory，我们认为compute-in-memory、logic-in-memory可以看作process-in-memory的子集。

在Onur等人的综述A modern primer on processing in memory中，也有诸如此类的分类划分，将pim进一步细分为两类，一类是pum，另一类时pnm：pum是process using memory，通过简单更改内存电路，来实现特定功能;还有一类是process near memory，主要思路是将计算单元放在靠近存储单元的地方，充分利用新型设备的内存特性进行加速。

本项目中使用的硬件内存单元UPMEM则属于PNM类型近内存计算单元：

UPMEM PIM 由数千个创新数据处理器 （DPU） 组成，这些处理器独立安置于DRAM内存芯片内，靠近数据，用于计算数据密集型操作，同时大幅减少片外数据移动。这些DPU受主CPU上运行的高级应用程序的控制，以确保任务编排。

单个UPMEM内存卡可安装于标准的内存插槽中。其上每个内存插条拥有128个可独立运行的RISC DPU核心，每个核心可执行最多24个线程，且单个DPU拥有1GB/s内存读取带宽。

现有的UPMEM研究主要集中于对特定应用（矩阵计算等）的优化。我们希望挖掘其在系统软件领域的潜力，使之作为协处理器协助CPU进行内存资源调度。

## 结合
基于原有zram实现，使用UPMEM内核驱动让PIM实现内存cache、压缩任务，实现节省CPU时间的目的。

## 所属赛道

2022全国大学生操作系统比赛的“OS功能设计”赛道

## 参赛要求
以小组为单位参赛，最多三人一个小组，且小组成员是来自同一所高校的本科生（2022年春季学期或之后本科毕业的大一~大四的学生）
如学生参加了多个项目，参赛学生选择一个自己参加的项目参与评奖
请遵循“2022全国大学生操作系统比赛”的章程和技术方案要求

## 难度
中等~高等

## 项目导师
宫晓利 gongxiaoli@nankai.edu.cn

## 特征
基于linux-6.1/6.2新版本内核新机制开发，修改内核实现
新硬件服务系统软件，优化内存使用量、zram带宽、减小CPU时间占用
使用实际硬件实际服务器运行
## 参考实现

# 预期目标
目标一 
    
    基于原有zram实现，使用UPMEM内核驱动让PIM实现内存cache、压缩任务

目标二 
        
    对新实现进行性能测试，是否有助于提升zram性能

补充目标 

    是否可以结合内核新加入的zram recompress机制，进一步优化其性能，实现分情况使用cpu或zram进行压缩/解压缩的混合计算机制，降低延迟


# 参考
* zram
https://wiki.gentoo.org/wiki/Zram
* zram recompress 
https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=443dd798062c

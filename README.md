# OS-Camp-Diary

参加2022开源操作系统训练营的学习日记。

[参考资料](#参考资料)

## 记录

## Day13 2022/7/18
目标：
- 完成第三章的代码。


## Day12 2022/7/17
把batch os那部分完成了。虽然那章的教程早就看完了，但是实际自己写了才知道还有很多没有完全理解的地方，比如：
- __restore是怎么同时作为trap_handler调用后的恢复和怎么作为app初始化的环境恢复的。
- 为什么trap_handler要返回一个&mut TrapContext。
- KERNEL_STACK和USER_STACK是怎么使用的，它们是在什么时候被设置上去的。

还有之前一个没答好的问题：
> 输出结果看上去有一些混乱，原因是用户程序的每个 println! 往往会被拆分成多个 sys_write 系统调用提交给内核。有兴趣的同学可以参考 println! 宏的实现。

有同学问这里为什么会拆成多个系统调用，实际上是因为write_fmt在有`{}`参数的时候会分别打印每个参数，每次都调用一下write_str, write_str里再调sys_write。

## Day11 2022/7/16
- 花了很多时间看riscv privileged spec，特别是把trap处理过程和相关的几个CSR仔细看了一遍。
- 自己写了batch os里用来生成link_app.S的build.rs，把加载APP那部分写了。

Q：为什么delegate到S-mode的trap的SPIE和SPP是往mstatus里写的？

A：因为sstatus是mstatus的子集部分。

## Day11 2022/7/15
- 重新弄了个跟rCore Tutorial的[repo](https://github.com/whfuyn/rcore-os)，一切按照自己的想法来做。
- 把gdb调试的环境弄好了，包括gdb-dashboard和相关的Makefile。

这里踩了一堆坑，freedom-tools预编译的那个riscv toolchain里的gdb执行gdb-dashboard的python脚本会报错，自己编译的时候Makefile里有个动态链接库的检查过不了，折腾了半天都不行，最后跟的[另一个教程](http://rcore-os.cn/rCore-Tutorial-deploy/docs/pre-lab/gdb.html)编了官方的gdb才可以了。

## Day10 2022/7/14
在rustsbi-qemu里看到它通过parse一个opaue获取设备信息，就去看了device tree spec，看完了前面的介绍和一些property的介绍。这部分需要一些硬件的知识，我不太熟悉。

看了riscv privileged spec前面的一部分。

trap handler相关的内容有点忘了，复习了一下。

### Day8-9 2022/7/12 - 2022/7/13
间歇性堕落，荒废几天，状态很糟糕。

But don't worry, I'll be fine.

### Day7 2022/7/11
早上和老师讨论了一下区块链放到内核里的想法。

看了riscv sbi spec, 跳过了PMU及以后的内容。rustsbi看起来只是一个interface而不是一个implementation。

于是又去看了rustsbi在qemu上的实现（rustsbi-qemu），在代码里看到了很多很有意思的细节，但是没有找到关于这些细节的qemu官方文档。

看到rustsbi-qemu的注释里提到了clint，就又去找了最新的aclint spec看了一遍。里面的关于timer、software interrupt还有Register Map的内容，解释了rustsbi-qemu里看到的一些代码。

### Day6 2022/7/10
- 看了看以往的操作系统选题，
- 翻了翻zCore、RustSBI的实现。
- 看完了rCore的第二章和第三章。

终于看到分时多任务系统了，这是之前就很感兴趣的内容，虽然实现的方法猜到了并不意外，能确定下来还是感觉不错。前几天看的riscv特权架构帮了大忙，得到了相互印证，理解起来很顺畅。

rCore教程的代码和练习都没跟着写，明天把这两天学的内容该写的代码全写了。

也应该迅速把lab给弄完，不然对lab作业里的坑不熟悉，有同学问的话现场研究就慢了。今天就有同学问test2报错asm找不到的问题，我跟着Makefile走了一路发现是ci-user把项目的riscv依赖重写成本地的很老的版本，虽然自己研究明白解决了，但这个问题已经有同学提过pr了，我还不知道。

前几天打印的两本risc (privileged) spec到了。

### Day5 2022/7/9
早上旁听了参加操作系统比赛的同学的交流会，感觉他们做的东西很厉害，自己不太懂。

中午在研究怎么把rCore-Tutorial/os跑起来。
main分支上的代码直接make run发现会卡在qemu运行里。

研究了一会儿没明白，切到v3.5.0，出了一堆global_asm/llvm_asm相关的编译错误，用新的asm语法重写了以后跑起来了。

usertests发现有一个测试panic unwrap none了，看了一下发现是allocator空间用完了，调大后运行正常。

回去继续研究main分支为什么跑不起来，后来发现是自己刚好有qemu就把装qemu那步跳了，但是运行要求qemu7.0……更新了就好了。

后面在看rCore的教程，第零章和第一章看完了，发现自己之前瞎看的时候的花了很多时间弄明白的疑惑在里面都有讲，比如rs/rd指啥、RV64GC指啥、执行环境的理解、exception/interrupt/trap的区别、链接器脚本（重要）什么意思。

加深了对object file链接过程的理解，收获很大。

感觉整体进度还是很慢，应该再加快一些。

### Day4 2022/7/8
早上在看rcore的教程，下午去参加急救培训，晚上和老师同学交流讨论。

同学们很强，我也要加快进度，赶快把blog_os和rcore过完，然后去选择一个有意思的题目做。
想做点新的东西出来而不是练习性质地造别人造过的轮子。

### Day3 2022/7/7
跟着[训练营教程](https://learningos.github.io/rust-based-os-comp2022/)的第一章讲的内容自己倒腾了一遍，
踩了一些坑，感觉和blog_os的开局差不多。

把《RISC-V手册》第十章剩下的内容看了：
- Page-based Virtual Memory
    > 这部分讲的东西，多级页表、地址翻译、TLB什么的，在CSAPP里Virtual Memory部分有更详细完整的介绍，
    > 还讲了在intel i7里的实现，非常推荐看看。

有同学问了一个late bound lifetime的问题，发现自己确实不知道，去看了相关的issue和Niko的[文章](https://smallcultfollowing.com/babysteps/blog/2013/10/29/intermingled-parameter-lists/#early--vs-late-bound-lifetimes)。也许我也应该去把Rust quiz做一遍？

看了riscv spec的introduction：
- 了解了riscv的设计目标和设计
- 知道了riscv的各种后缀IMAFDCG代表的含义，extensions
- Execution Environment Interface(EEI)，这个概念需要再多看两遍，理解得不是特别好
- Execeptions, Traps, and Interrupts，这节再看看

明天的任务：
- 继续看spec和privileged spec的intro
- P&H的第二章
- rcore的教程，尽快看，尽快参与到开发中


### Day2 2022/7/6
看《RISC-V手册》的英文版第十章，RV32/64 Privileged Architecture

对RISC-V的特权架构有了个大致的理解：
- Control and Status Registers(CSRs) 这些寄存器的作用
- exception的分类和处理流程（重要）
- Physical Memory Protection(PMP) 基于段的内存保护
- S mode的delegate机制（重要）

TODO: 写一篇文章总结riscv特权架构。

看了一些riscv汇编的资料。

### Day1 2022/7/5

看blog_os的paging部分。

浏览了一下训练营的资料，调试好github workspace环境。

参加训练营开幕式。


## 参考资料

[RISC-V ELF psABI Document](https://github.com/riscv-non-isa/riscv-elf-psabi-doc)

[RISC-V C Calling Convention](https://riscv.org/wp-content/uploads/2015/01/riscv-calling.pdf)

### spec
https://riscv.org/technical/specifications/

- [riscv spec](https://github.com/riscv/riscv-isa-manual/releases/download/Ratified-IMAFDQC/riscv-spec-20191213.pdf)
- [riscv privileged spec](https://github.com/riscv/riscv-isa-manual/releases/download/Priv-v1.12/riscv-privileged-20211203.pdf)
- [riscv aclint spec](https://github.com/riscv/riscv-aclint/releases/download/v1.0-rc4/riscv-aclint-1.0-rc4.pdf)
- [device tree spec](https://github.com/devicetree-org/devicetree-specification/releases/download/v0.4-rc1/devicetree-specification-v0.4-rc1.pdf)

### 汇编
https://msyksphinz-self.github.io/riscv-isadoc/html/rvi.html

https://shakti.org.in/docs/risc-v-asm-manual.pdf

https://github.com/riscv-non-isa/riscv-asm-manual/blob/master/riscv-asm.md

### QEMU

https://www.qemu.org/docs/master/about/index.html


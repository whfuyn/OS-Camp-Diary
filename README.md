# OS-Camp-Diary

参加2022开源操作系统训练营的学习日记。

## 记录

### Day2 2022/7/7
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

看了一些riscv汇编的资料。

### Day1 2022/7/5

看blog_os的paging部分。

浏览了一下训练营的资料，调试好github workspace环境。

参加训练营开幕式。



## 参考资料
### spec
https://riscv.org/technical/specifications/

- [riscv spec](https://github.com/riscv/riscv-isa-manual/releases/download/Ratified-IMAFDQC/riscv-spec-20191213.pdf)
- [riscv privileged spec](https://github.com/riscv/riscv-isa-manual/releases/download/Priv-v1.12/riscv-privileged-20211203.pdf)

### 汇编
- [riscv汇编参考](https://msyksphinz-self.github.io/riscv-isadoc/html/rvi.html)
- [riscv汇编参考](https://shakti.org.in/docs/risc-v-asm-manual.pdf)
- [riscv汇编参考](https://github.com/riscv-non-isa/riscv-asm-manual/blob/master/riscv-asm.md)


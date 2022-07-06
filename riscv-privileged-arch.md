# RISC-V 特权架构

Hart is a contraction of hardware thread.

三种模式：
- M模式(Machine mode)
    > run the most trusted code
- S模式(Supervisor mode)，
    > provide support for operating systems
- U模式(User mode)

RISC-V把Exceptions分为两类：
- Synchronous exceptions, arise as a result of instruction execution
- Interrupts, external events that are asynchronous with the instruction stream, like a mouse button click

RISC-V中产生和清除中断是平台相关的，但中断的处理和屏蔽(mask)方法是确定的。

Control and Status Registers(CSRs)
- `mtvec`, Machine Trap Vector, 出现exception时的跳转地址
    > ? 是中断处理表的地址还是直接就是中断处理函数的地址？如果是后者，它是怎么知道要用哪个处理函数的？
- `mepc`, Machine Exception PC，指向exception出现时的指令
- `mcause`, Machine Exception Cause, 出现了哪个exception
- `mip`, Machine Interrupt Pending, 当前处于pending状态的中断表
- `mie`, Machine Interrupt Enable, 可处理的和必须忽略的中断表
- `mstatus`, Machine Status, 包含global interrupt enable(MIE bit)和很多其它的状态
- `mtval`, Machine Trap Value, holds additional trap information: the faulting address for
address exceptions, the instruction itself for illegal instruction exceptions, and zero for
other exceptions.
- `mscratch`, Machine Scratch, 存储一个word的临时数据
    > 这里的word应该指的是XLEN

中断只在mstatus.MIE为1，并且mie里相应的bit（根据interrupt code）为1的情况下才会被处理。



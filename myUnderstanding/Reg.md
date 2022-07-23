# Reg.scala

## object RegEnable

### def apply[T <: Data](next: T, enable: Bool): T

调用了和之前一样看不懂的SourceInfoTransform相关库
根据要求的下一时刻寄存器值返回一带使能信号、无复位初始化的寄存器

### def apply[T <: Data](next: T,init: T, enable: Bool): T

调用了和之前一样看不懂的SourceInfoTransform相关库
根据要求的下一时刻寄存器值返回一带使能信号、有复位初始化的寄存器

## object ShiftRegister

### apply[T <: Data](in: T, n: Int, en: Bool): T

调用了和之前一样看不懂的SourceInfoTransform相关库
返回一个对输入信号n周期延迟的移位寄存器

### apply[T <: Data](in: T, n: Int, resetData: T, en: Bool): T

调用了和之前一样看不懂的SourceInfoTransform相关库
返回一个对输入信号n周期延迟的带复位初始值的移位寄存器

## object ShiftRegisters

### def apply[T <: Data](in: T, n: Int, en: Bool): Seq[T]

调用了和之前一样看不懂的SourceInfoTransform相关库
返回一个延迟从1到n的寄存器序列

### def apply[T <: Data](in: T, n: Int): Seq[T]

调用了和之前一样看不懂的SourceInfoTransform相关库
返回一个延迟从1到n的寄存器序列，无使能信号

### apply[T <: Data](in: T, n: Int, resetData: T, en: Bool): Seq[T]

调用了和之前一样看不懂的SourceInfoTransform相关库
返回一个对输入信号有1到n周期延迟的带复位初始值的移位寄存器序列

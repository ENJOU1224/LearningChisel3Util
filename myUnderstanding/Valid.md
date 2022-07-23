# Valid.scala

## class Valid[+T <: Data](gen: T) extends Bundle

对数据加上valid信号的Bundle

### def fire: Bool

就直接等于valid

### def fire: Bool(dummy: Int = 0): Bool

同上，因为scala3环境空参数调用函数不可用

## object Valid

### def apply[T <: Data](gen: T): Valid[T]

返回调用Valid类传入参数的新Valid子类

## object Pipe

### def apply[T <: Data](enqValid: Bool, enqBits: T, latency: Int)(implicit compileOptions: CompileOptions): Valid[T]

要求输入参数latency大于等于0
当latency等于0时，生成数据类型与enqBits类型相同的Valid接口返回
当latency大于0时，生成复位值为false.B的寄存器v用于存储enqValid，生成带使能，使能信号为enqValid的存储enqBits的寄存器b。并以v,b两寄存器以及lantency-1为参数自身调用。

### def apply[T <: Data](enqValid: Bool, enqBits: T)(implicit compileOptions: CompileOptions): Valid[T]

调用前一apply，将未指定的lantency设为1

### def apply[T <: Data](enq: Valid[T], latency: Int = 1)(implicit compileOptions: CompileOptions): Valid[T]

使用enq.valid 和 enq.bits调用前面的apply

## class pipe[T <: Data](val gen: T, val latency: Int = 1)(implicit compileOptions: CompileOptions) extends Module

### clas PipeIO extends Bundle

调用了Valid来生成了两个enq和deq两个接口
然后创建了PipeIO类型的io,将前面生成的接口接入

# Math.scala

## object log2Up

### def apply(in: BigInt): Int

直接调用了Chisel.log2Up(in),不知道啥情况，这不是自身调用吗

## object log2Ceil

### def apply(in: BigInt): Int

输入大于0的情况下，求in-1的位宽

### def apply(in: Int): Int 

类型转换完调用BigInt的apply

## object log2Down

### def apply(in: BigInt): Int

也是直接调用了chisel.log2Down,很怪，看不懂

## object log2Floor

### def apply(in: BigInt): Int

调用log2Ceil，然后调用isPow2函数判断输入是否为2的幂，若是，则不动，若不是，在log2Ceil基础上-1

### def apply(in: Int): Int 

类型转换完调用BigInt的apply

## object isPow2

### def apply(in: BigInt): Boolean

当输入大于零的时候，输入与输入-1位与的结果为零就是2的幂

### def apply(in: Int): Boolean

将输入参数类型转换完调用上面那个BigInt的apply完成

## object unsignedBitLength

### def apply(in: BigInt): Int

当输入大于0时，输出In.BitLength

## object signedBitLength

### def apply(in: BigInt): Int

将输入转换为scala的Int类型，0的时候位宽为0，-1的时候位宽为1，其他时候位宽为in.bitLength +1


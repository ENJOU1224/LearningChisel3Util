# GrayCode.scala

## object BinaryToGray

二进制转格雷码

### def apply(in: UInt): UInt

使用传入参数右移一位后异或传入参数得到返回值

## object GrayToBinary

格雷码转二进制

### def apply(in: UInt, width: Int): UInt

调用另一个apply

### def apply(in: UInt): UInt

如果宽度小于2返回传入参数
如果宽度大于等于2，从最高位开始向低位逐个遍历，每个遍历到的位置从最高位开始异或到当前位作为当前位的格雷码值
Scala高级特性看着头疼。。。。。还是不够熟悉，我看了快一小时才分析出来啥意思

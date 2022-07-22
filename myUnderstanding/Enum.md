# Enmu.scala

## trait Enum

定义一个UInt常数序列

### protected def createValues(n: Int): Seq[UInt]

返回从0到n的Bits子类序列

### def apply(n: Int): List[UInt]

调用上述createValues函数来返回一个UInt序列，使用toList转为序列

## object Enum extends Enum

# BitPat.scala

------
object 或 def 的private是否为作用域范围。是否可外部访问。implicit为隐式变量

## object BitPat

### private implicit val bitPatOrder

引入 scala.math.Orderd.orderingToOrdered 库实现

#### def compare(x: BitPat, y: BitPat): Int

引用上述库中compare函数用于比较两个BitPat类型的子类的三个参数是否相同,相同返回0，x大返回1，y大返回-1

### private def parse(x: String): (BigInt, BigInt, Int) //(value,mask,count)

用于根据传入字符串来生成bits,mask和count三个参数

* value(bits)的二进制表示与x等长,x中为1的位置在bits中为1,其余为0
* mask的二进制表示与x等长,x中为二进制串中0,1的位置在mask中为1,'?'位置在mask中为0
* count为x的二进制位数量统计
**以上三个参数均只计算或检测'0','1','?'这三个符号,'?'与'_'不计入内,其他符号非法**

### def apply(n: String): BitPat

用于将传入的字符串转为BitPat类型

### def dontCare(width: Int): BitPat

新建一个width位的全'?'的BitPat

### def Y(width: Int = 1): BitPat

新建一个width位的全'1'的BitPat,width若未给出,则默认为1

### def N(width: Int = 1): BitPat

新建一个width位的全'0'的BitPat,width若未给出,则默认为1

### def bitPatToUInt(x: BitPat): UInt

require(x.mask == (BigInt(1) << x.getWidth) - 1)
即要求原始字符串全部由'0','1'组成,不能有'?',在这个前提下返回转为UInt的BitPat.value,位宽为原BitPat位宽

### implicit class fromUIntToBitPatComparable(x: UInt) extends SourceInfoDoc

语法上不太能看得懂,但是大概这个类里面实现了对于一个UInt的值和BitPat的类的返回值的比较，包括===和=/=

## object BitSet

貌似是BitPat的序列,序列中的所有BitPat必须位宽相同

### def apply(bitpats: BitPat*): BitSet

输入一个bitpats序列，返回一个BitSet类flatMap这些也有点整不明白,但是大致能理解是在从不同类型的数据转成BitSet内应该存的数据

## sealed trait BitSet

定义了许多特性函数,属性(terms),获取宽度(getWidth),转为字符串(toString),是否为空(isEmpty),是否有匹配项(matches(input: UInt)),是否有重复(overlap),是否包括(cover(that: BitSet)),求交集(intersect(that: BitSet)),求补集(subtract(that: BitSet)),相加(union(that: BitSet))判断是否相等(equals(obj: Any))   

## sealed class BitPat(val value: BigInt, val mask: BigInt, val width: Int)  

extends util.experimental.BitSet with SourceInfoDoc
介绍了一堆BitPat类的字函数,如获得宽度之类，也有获得原串之类的，以及部分运算的重载。。。。我发现涉及到SourceInfoDoc相关的我就抓瞎了。。。。可能后续要找找那个看看。是不是chisel的自定的东西。不太懂。还没开始研究。

定义了一些函数和运算，有判断相等的(===),不等的(=/=),是否有重复(overlap),是否包括(cover(that: BitSet)),求交集(intersect(that: BitSet)),求补集(subtract(that: BitSet)),是否为空(isEmpty),原字符串(rawString)  

# MixedVec.scala

## object MixedVecInit

用指定的默认值和每个默认值的类型来创建一个混合向量线

### def apply[T <: Data](vals: Seq[T]): MixedVec[T]

创建了一个混合矢量线，然后把传入参数序列中所有的元素替换为他们的类型来初始化混合矢量线
然后再将传入的序列连接到对应生成的线上

### def apply[T <: Data](val0:T, vals: T*): MixedVec[T]

将变长参数转换为序列然后调用之前的apply实现

## object MixedVec

### def apply[T <: Data](eltsIn: Seq[T]): MixedVec[T]

调用使用传入参数创建新的MixedVec子类

### def apply[T <: Data](val0:T, vals: T*): MixedVec[T]

将变长参数转换为序列然后调用之前的apply实现

### def apply[T <: Data](MixedVec: MixedVec[T]): MixedVec[T]

使用传入参数MixedVec.elts调用apply

### def apply[T <: Data](vec: Vec[T]): MixedVec[T]

使用传入参数的长度和元素类型来调用最前面的apply

## final class MixedVec[T <: Data](private val eltsIn: Seq[T]) extends Record with collection.IndexedSeq[T]  

内部将输入参数序列每个元素的类型转为了IndexedSeq类型存入val elts中

### def apply(index: Int): T 

返回elts序列index位置的类型

### def :=(that: Seq[T]): Unit

要求当前序列(指elts)与符号后的类型序列(传入参数)等长，将两个序列的每个元素连接

### def length: Int

调用elts.length返回


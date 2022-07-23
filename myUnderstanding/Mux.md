# Mux.scala

## object Mux1H

### def apply[T <: Data](sel: Seq[Bool], in: Seq[T]): T  

将输入的两个序列zip然后调用别的apply

### def apply[T <: Data](in: Iterable[(Bool, T)]): T  

调用了SeqUtils使用Bool部分作为独热码返回输出

### def apply[T <: Data](sel: UInt, in: Seq[T]): 

生成序列，将UInt位置置true，然后调用序列的apply

### def apply(sel: UInt, in: UInt): Bool

返回sel与in的与结果的位或

## object PriorityMux

### def apply[T <: Data](in: Seq[(Bool, T)]): T  

直接调用SeqUtils.PriorityMux

### def apply[T <: Data](sel: Seq[Bool], in: Seq[T]): T  

将两个输入zip然后调用参数为Seq[(Bool, T)]的apply

### def apply[T <: Data](sel: Bits, in: Seq[T]): T  

生成序列，将UInt位置置true，然后调用序列的apply

## object MuxLookup

### def apply[S <: UInt, T <: Data](key: S, default: T, mapping: Seq[(S, T)]): T

如果输入的key有宽度，且输入的key 的所有可能性都被覆盖，则不使用default,将mapping的第一个元素的值作为default，如果输入的key无宽度或者没有完全覆盖所有可能性，则使用default,最后使用foldLeft遍历整个mapping比对key，若mapping的k与key相等，则返回值为该k对应的value，若全部不等则返回default

## object MuxCase

### def apply[T <: Data](default: T, mapping: Seq[(Bool, T)]): T  

将传入的mapping倒序遍历，循环迭代返回值，倒序遍历的目的是将本该最早被遍历到的元素变成最晚遍历到的，能够保留值不被覆盖。若遍历不到返回传入的default，遍历到则返回按正常顺序最早遍历到的

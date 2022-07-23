# OneHot.scala

## object OHToUInt

独热码转UInt值

### def apply(in: Seq[Bool]): UInt

调用Cat将输入倒转拼位，以及调用in.size得到宽度来调用别的apply

### def apply(in: Vec[Bool]): UInt

将 in内的Bool转为UInt，然后获取in.size得到宽度来调用别的apply

### def apply(in: Bits): UInt

直接用in以及in.getWidth调用别的apply

### def apply(in: Bits, width: Int): UInt

被前面调的函数，若宽度小于等于2，则直接调用Log2(in, width)
若宽度大于2，折半检查拼位，若高半部分有1则高位拼1后递归调用，否则高位拼0后递归调用

## object PriorityEncoder

优先编码器

### def apply(in: Seq[Bool]): UInt  

调用优先选择器(PriorityMux)传入输入和0到输入大小的UInt序列

## UIntToOH

### def apply(in: UInt): UInt  

直接使用移位来实现

### def apply(in: UInt, width: Int): UInt 

宽度为0或者1时，直接返回0\1(对应输入)
宽度为其他时，通过宽度来得到要用到的独热码宽度，通过宽度画出范围，然后再将输入的范围内数值作为1的左移量移动返回

## object PriorityEncoderOH

### private def encode(in: Seq[Bool]): UInt

创建了一个与输入大小相等的Seq名为outs，然后里面每个元素都是对应in位宽的的对应序号的独热码
然后使用优先选择器来将输入参数加上保底的true输入，以及outs加上对应保底true的0.U输出

### def apply(in: Seq[Bool]): Seq[Bool]

使用输入参数in调用前面的encode然后将对应的每个输入对应的独热码存入序列中返回

### def apply(in: Bits): UInt

生成从0到apply的传入参数(in)宽度的序列，其中每个位置放入对应位置的传入参数元素(in(i))然后以此为参数调用encode

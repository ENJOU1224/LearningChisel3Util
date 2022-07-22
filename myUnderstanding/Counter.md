# Counter.scala

## class Counter private (r: Range, oldN: Option[Int] = None) extends AffectsChiselPrefix

### def n: Int

当从0开始的步进为1的Range被传入时，若有oldN传入，则n就等于传入的oldN里面的原值

### def this(n: Int)  

创建n步的从0开始步进为1的计数器

### def range: Range

返回传入的参数r,即计数器范围

### def inc(): Bool  

实现步进，通过判断当前值是否等于范围终点值来实现是否结束计数,返回值为是否结束计数。根据步进正负情况来让计数值加减步进绝对值。由于判断是否结束后依然会再进行一次步进,所以即使计数完成,也会再进行一次步进这种情况导致了,若终点值加上步进并非2的幂，则会单独为这一步多形成电路,造成了资源的浪费。所以通过函数内通过判断将此时的value步进重写为起始值。

### def reset(): Unit

将计数值重设为初始值

## object Counter

### def apply(n: Int): Counter

使用new Counter(n)之间创建了一个从0到n,步进为1的计数器,Counter(n)的实现在前面有介绍到

### def apply(cond: Bool, n: Int): (UInt, Bool)

同样是实现一个计数器,相比于前面一个,这种实现引入了自增的条件,自增条件为true的情况下,计数器才开始进行自增(inc())。返回值为当前计数器计数值(value)以及是否计数完成的标志(warp)

### def apply(r: Range, enable: Bool = true.B, reset: Bool = false.B): (Uint, Bool)

使用范围r来创建了一个计数器,引入了复位和使能,复位时调用计数器的复位,使能时调用计数器的自增(inc()),返回计数器的值(value)以及是否计数完成的标志(warp)

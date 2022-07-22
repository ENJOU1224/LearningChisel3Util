# Decoupled.scala

## abstract class ReadyValidIo[+T <: Data](gen: T) extends Bundle

在这个抽象类当中定义了ready,valid和bits三个接口,ready是接收方准备好接收数据的信号,vaild是发送方在Bits中已送出有效数据的信号。bits为数据传输接口。其中ready为输入接口，valid和bits都为输出接口,valid和Bits的接口均为Bool型,bits的类型与传入的数据有关,有一个选项没看懂，另一个选项为与传入参数gen的类型相同.

## object ReadyValidIO

建了一个类,AddMethodsToReadyValid,如下

### implict class AddMethodsToReadyValid[T <: Data](target: ReadyValidIO[T])

#### def fire: Bool

由传入参数的ready和valid相与得出,标志接口正处于数据传输中

#### def fire(dummy: Int = 0): Bool

Scala3不允许无参数的函数调用,故使用此函数来替代以上函数被外部调用

#### def enq(dat: T): T

将传入参数dat从target.bits传出同时将valid置true.B 返回值为传入参数dat

#### noenq(dat: T): Unit

将target的valid置false.B,bits为DontCare

#### deq(): T

将target的ready置为 true.B,返回target.bits

#### nodeq(): Unit

将target的ready置为 false.B

## class DecoupledIO[+T <: Data](gen: T) extends ReadyValidIO[T](gen)

创建了一个ReadyValidIO的子类

## object Decoupled

将decoupled握手协议加入数据接口

### def apply[T <: Data](gen: T): DecoupledIO[T]

直接new DecoupledIO(gen)返回了一个新的DecoupledIO类，参数为传入参数gen

### private final class EmptyBundle extends Bundle

没说干啥的好像。。。。有注释说是一个快且脏的正确使用空数据的方法

### def apply(): DecoupledIO[Data]

直接调用前面的apply把这个空接口送进去了，也就是直接生成了Data类型bits的DecoupledIO

### def empty: DecoupledIO[Data]

调用了前面的生成Data类型bits的DecoupledIO的函数

### def apply[T <: Data](irr: IrrevocableIO[T]): DecoupledIO[T]

将IrrevocableIO转为DecoupledIO,放弃对不可撤销的保证,前提是当前DecoupledIO为输入方向
生成一个bits与传入函数中bits类型相同的DecoupledIO,同时将对应接口转接置DecoupledIO,返回此DecoupledIO

## class IrrevocableIO[+T <: Data](gen: T) extends ReadyValidIO[T](gen)

根据注释,IrrevocableIO当valid为高电平后就不会改变值,及时ready为低电平,并且valid被置为高电平后,只有ready被置为高电平后,valid才会恢复低电平

## object Irrevocable

### def apply[T <: Data](gen: T): IrrevocableIO[T]

根据传入参数创建新的IrrevocableIO

### def apply[T <: Data](dec: DecoupledIO[T]): IrrevocableIO[T]

将传入的DecoupledIO转为IreevocableIO,前提是当前DecoupledIO为输出方向

## object EnqIO

### def apply[T <: Data](gen: T) DecoupledIO[T]

返回一个gen类型bits的DecoupledIO(调用端向外输出)

## object DeqIO

### def apply[T <: Data](gen: T) DecoupledIO[T]

返回一个gen类型bits的DecoupledIO(调用端从外输入)

## class QueueIO[T <: Data](private val gen: T, val entries: Int, val hasFlush: Boolean = false) extends Bundle

为队列而建的IO类，内部变量名字都是相对于客户端的角度来说的。所以相反

## class Queue[T <: Data](val gen: T, val entries: Int, val pipe: Boolean, val flow: Boolean = false, val useSyncReadMem: Boolean = false, val hasFlush: Boolean = false)(implict compileOptions: chisel3.CompileOptions) extends Module()

生成gen类型的enetries数量的队列模型
io部分根据参数创建了前面的QueueIO
ram根据参数创造同步或异步读memory
使用两个计数器来记录控制队列进出指针,初始化为队列数量
有队列可能满标志(maybe\_full)，进出队列地址相同标志(ptr\_match),通过这两个标志组合得出队列空满情况
根据io的入队出队是否正在读取来得知入队出队
根据传入flush来标记是否冲刷队列
当入队时，将io上的数据传入memory中，自增入队指针
出队是自增出队指针，不同时出入队时，下一周期的队列可能满标志(maybe\_full)为当前入队情况(do\_enq)
当非空时io的出队valid保持高电平
当满时，io的入队ready保持高电平
若使用了同步读的存储，提前一周期取数据
如果输入参数flow为1，则允许队列为空情况下数据不写入存储直通输出
如果输入参数pipe为1，则队列类似流水线一样工作
输出的队列现有存储条目为:若队列满则为传入队列大小数量，否则为入队地址与出队地址差;

## object Queue

### def apply[T <: Data](enq: ReadyValidIO[T], entries: Int = 2, pipe Boolean = false, flow Boolean = false, useSyncReadMem: Boolean = false, flush: Option[Bool] = None): DecoupledIO[T]

传入entries为0时，使用传入参数新建DecoupledIO并将对应接口接上，就是普通的DecoupledIO,返回出队接口
传入entries不为0时，使用传入参数新建Queue Module，并连接对应的接口，返回出队接口

### def Irrevocable[T <: Data](enq: ReadyValidIO[T], entries: Int = 2, pipe: Boolean = false, flow: Boolean, useSyncReadMem: Boolean = false, flush: Option[Bool] = None): IrrevocableIO[T]

调用apply返回一个Queue，根据传入参数调用IrrevocableIO,接上对应接口,返回IrevocableIO



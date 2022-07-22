# Arbiter.scala

## class ArbiterIO[T <: Data](private val gen: T, val n: Int) extends Bundle

n个gen类型及宽度的带ready-valid输入，1个gen类型及宽度输出的独热码选择的仲裁器接口。

## private object ArbiterCtrl

仲裁控制器,用于选定哪些信号有访问权限

### def apply(request: Seq[Bool]): Seq[Bool]

输入序列宽度为0时返回空允许序列。宽度为1时返回单个true.B的允许序列。宽度大于1时返回序列中,第一个出现true的位置之前全部为true,其他为false
在我理解里这玩意和学过的中断屏蔽字的生成有点像，序列中越低中断等级越高，当中断请求发出时，允许更高等级的中断请求，屏蔽低等级的中断请求

## abstract class LockingArbiterLike[T <: Data](gen: T, n: Int, count: Int, needsLock: Option[T => Bool]) extends Module

抽象类,根据给定的count以及needsLock输入来判断是否锁定输出的通道号。如果count等于1，和普通的仲裁一样，大于1并且needsLock时,锁定仲裁，锁定时长为count,优先级和选择都是留给子类定义的。主要是定义了和锁定以及ready的发出逻辑

## class LockingRRArbiter[T <: Data](gen: T, n: Int, count: Int, needsLock: Option[T => Bool] = None) extends LockingArbiterLike[T](gen, n, count, needsLock)

带锁定的循环优先级仲裁器,LockingArbiterLike的子类，在 LockingArbiterLike的基础上补充了循环优先级相关的逻辑。
优先级部分，记录了上一输出许可(lastGrant)，基于上一输出位得到了许可掩码(grantMask)，再通过掩码与输入的valid信号得出valid掩码(validMask)。
新的许可(grant)上，ctrl(i) && grantMask(i) || ctrl(i+n)用于实现循环优先级，ArbiterCtrl会按编号来从高到低优先级遍历，将第一个遍历到的validMask及更高优先级的位置全部置为true,之后的全部置为false,由于我们需要循环优先级，在这里将ctrl(i)与grantMask(i)相与,这样就将曾经循环到过的优先级屏蔽。只保留未循环到的部分。但若当前循环到的最高优先级到按序号的最低优先级之间没有validMask为true的,则轮到在循环优先级中优先级更低的，在序号优先级中优先级更高的已循环到的部分优先级。或上ctrl(i+n)部分的用意在此。。。
新的选择(choice)上,默认为n-1,由优先级低到高进行两次循环,保证了循环优先级中的次序优先,按循环结果来说，若有任意一个validMask为true,则不需要考虑valid的循环情况，validMask循环保证了按循环优先级高优先级的部分能被优先考虑，当validMask全为false时，再去考虑valid循环的情况，valid循环是照顾了在循环优先级中优先级低的高次序优先级位置。

## class LockingArbiter[T <: Data](gen: T, n: Int, count: Int, needsLock: Option[T => Bool] = None) extends LockingArbiterLike[T](gen, n, count, needsLock)

直接将valid信号送入ArbiterCtrl得到次序优先级信号，然后从低次序优先级遍历到高次序优先级寻找最高优先级的valid信号输入，此valid信号的序号为新的choice

## class RRArbiter[T <: Data](val gen: T, val n: Int) extends LockingRRArbiter[T](gen, n, 1)

只有这一行，因为传入count参数为1的带锁定仲裁器的实现就是不带锁定的仲裁器

## class Arbiter[T <: Data](val gen: T, val n: Int) extends Module

不知道为啥这就从新写了一个，不继承Locking的了，是一个次序优先级的仲裁器的类。直接通过从低优先级到高优先级的遍历循环找到最高次序优先级的valid然后将该valid的序号及对应的数据至为io.chosen及io.out.bits.优先级直接调用了ArbiterCtrl.在io.out.ready为true的基础上,所有grant为true的位置对应的ready都为true。在grant非全true(即除了头尾外有valid输入) 与最低优先级位有valid 输入的情况下。输出的io.out.valid为true

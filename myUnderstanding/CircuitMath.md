# CircuitMath.scala

## object Log2

### def apply(x: Bits, width: Int): UInt

功能:返回能表示这width宽度的x的以2为底的Log值

* 输入宽度小于2时,返回0
* 输入宽度等于2时,返回1
* 输入宽度在2到4之间时,若最高位为1,则返回值就是位宽,若非1,则递归调用apply(x,width-1)
* 上述情况以外的进入二分，将传入宽度width以2为底取对数，向上取整，这即半位宽点的位置，MSB到半位宽点为高半部分，半位宽点-1到LSB为低半部分。使用(高半部分非0标志,(若高半部分非0则递归调用高半部分,否则递归调用低半部分))拼位组成最后的答案。注意的点是，此处选择的位宽为递归到最后一次的最小位宽反向推算。。。。最后一次递归的位宽即2和3结果的位宽,然后每层+2,真的强。。。纯看完全想不到这点，嵌套得有点深。。。我研究了三四个小时发现手算的答案不一样，最后经过朋友提醒生成Verilog研究了一下发现了位宽的问题。。。。。

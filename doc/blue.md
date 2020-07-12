<!--
 * @Author: your name
 * @Date: 2020-06-25 22:17:42
 * @LastEditTime: 2020-07-01 20:20:46
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edi
 * @FilePath: \MyCode\doc\.net\蓝桥\blue.md
--> 
## 整数的范围推算
八卦
```
---
- -
---
```
2*2*2=8
2^6=64
> 1byte 有8位
>>用二进制表示时,最高位符号位
> 公式(二进制转十进制)
```
2^0+2^1+....+2^n=2^(n+1)-1
2^0+2^1+....+2^6=2^(6+1)-1=128-1=127
```
> 表示有符号8位数字
> -128~127
>>-128(最小数)的由来
>
>> 最高位1代表负数,无法表示最小数-128<br>计算机通过用补码来使符号和数值统一起来<br>补码=（原数字）反码+1<br>对-128的取绝对值 求他的原码
```
128=2^7
10000000原(绝对值)
01111111反
      +1补
----------
10000000存储值
对存储值取反,再加补码又等于原码了
-128(十进制)对应的计算机存储值是-128
```
>>> 表示最小是在最高位填1
> 计算机通过用补码来使符号和数值统一起来<br>如果最小数是-129<br>
```
10000001(129的原码)
01111110 反码 
      +1 补码
-----------
01111111 存储值
-129的存储值是127
符号和数值无法统一起来
```

> 2 byte-16位   
2^15.....2^0  
最大正数（高位0，其余1）(14位1)  
01.....1(2)=2^15-1  
最小负数数（高位1，其余0）(14位0)    
10000...0(2)=(-2)^15
<br>
>> int 4 byte -32位  （-2)^31~2^31-1  
> 
>> long 8 byte -64位  (-2)^63~2^63-1 
 
## 位运算
> 左移和右移
>> 对于int型的数1<<35和1<<3是相同的
int型32位,大于32位的位移对位移数取模35%32=3


#### 异或
> 异或:可以理解为不进位加法:1+1=0 ,0+0=0,1+0=1
> 
>>性质
 1.交换律,结合律
 >>x^x=0,x^0=x<br>
 >>A^B^B=A^0=A

>>> *例子1.  判断奇偶数*
  + i%2=0或1
  + 2进制看最后一位是不是1 
>>> *例子2 获取二进制位是1还是0(86=x=01010 110 的第五位是1还是0 )*  
```
   00010000 (1<<4) 判定第五位 
 & 01010110 (86的二进制)
-----------
   10011001 (1>>4)
```
   

>>> *例子3.交换两个正数变量的值*

``` 
int A;
int B;
[A]原有的A
[B]原有的B
A=A^B
B=A^B=[A]^B^B=[A]
A=A^B=([A]^[B])^[A]=[B]
```
>>> *例子4不用判断语句,求整数的绝对值*
```
正数,例如(0右移31位)       000000
负数,例如(1带符号右移31位)  111111
11111111(最大的负整数-1)
定义未知数int A;
(A>>31)->x(0/-1)
未知数右移31位有两种结果0/-1
    A^0=A
    A^(-1)=
              111111(-1)
            ^ 011011(A假设是这个数)
              ------
              100100(正好是假设A的取反值)
```
 > (A^-1) 相当于A *取反* 但这里不是绝对值)

因为
```
01111111 反码 
      +1 补码
-----------
10000000 存储值
```
所以,A>>31为正数时(以0为例)A^0=A本身就是绝对值不用加补码
```
(A^(A>>31))
```
而A>>31为负数时(以1为例)A^-1=(A取反)需要补码,所以需要下面
> 无符号右移
```
A>>>31
A为负数时(以1为例)
1>>>31
00000001(相当于补码)
A为正数时(以为例)
0>>>31
00000000(等于啥也没加)
(A^(A>>31))+(A>>>31)
正数时以(0为例)
A^0
+0(0>>>31)0
----
A
负数时(1为例)
A^(-1)  A取反
+1(1>>>31) 补码
------
A
```
## 类型转换
> 自动类型转换 
>> byte->short->int
>>        char->int
>>              int->long
>>              int->double
取余高于加法和乘除同级
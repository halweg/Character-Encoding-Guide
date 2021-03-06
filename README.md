# 由一个关键字 char 引发的问题 : 一张导图带你摸清字符集的来龙去脉
![image](https://github.com/halweg/Character-Encoding-Guide/blob/master/image/example1.jpg)
![image](https://github.com/halweg/Character-Encoding-Guide/blob/master/image/example2.jpg)
![image](https://github.com/halweg/Character-Encoding-Guide/blob/master/image/example3.jpg)
![image](https://github.com/halweg/Character-Encoding-Guide/blob/master/image/example4.jpg)
![image](https://github.com/halweg/Character-Encoding-Guide/blob/master/image/example5.jpg)
## 基本组成和术语

### 字节：8bit，历史原因，最早的寄存器宽度只有8bit，和其它一些商业原因，内存的最小寻址单元，一次就取8bit,

### 字长：数据总线的宽度和寄存器的宽度,pc主流64bit

### 字：就是一串字节

### 字符集：

- ASCII
- Unicode
- GB系列
- ISO 8859
- BIG5
- UCS

### Encode, Decode

### 字符编码(Character Encoding)模型 5 大层次

- 支持哪些字符
- 这些字符的编号是什么
- 这些编号如何编码成一系列逻辑层面有限大小的数字，即码元序列；
- 这些码元序列如何转化为物理上的字节序列(字节流)
- 在某些特殊的传输环境中(比如Email)，再进一步将字节序列进行适应性编码处理

### 5大层次总结

- 抽象字符表

	- 无序集合
	- 确定了支持哪些符号
	- 可以封闭如Ascii，可以不封闭

- 编号字符集

	- 把符号和数字编号对应起来
	- 编号空间就是数字编号的范围

-  字符编号模式 ：character encoding form    CEF

	- 把码点对应为1个或多个码元
	-  需要可扩展的，矛盾
	- 码元和码元序列不一样，码元序列是一组码元
	- 码元序列就是逻辑上的10序列
	- 注意：这里的10序列还没有到物理层面，因为没有考虑大小端问题等

- 序列化格式：character encodeing  scheme    将逻辑上的码元序列映射为物理上的字节序

	- 0宽字符：历史原因，来指定大小端问题
	- 对应到物理存储单元

-  传输编码语法TES(Transfer Encoding Syntax)

	- 指定到指定阈值：如base64等
	- 一些压缩算法

## 字符编码的由来

### EBCDIC码，IBM推出，由于英文字符不连续，有坑被放弃

### ASCII，  成了工业标准

- 00000000 ~ 01111111
-  0x00 ~ 0x7F
- 为什么最高位是0:

	- 抽象字符集只有127个，够用了
	- 最高位也可以用来做奇偶数校验

- 0~31: 控制字符，如响铃，打印机针头换行并回退， 0x00表示字符串结束等
- 32~126:数字，字母，标点等
- 127：del

### EASCII及ISO 8859字符编码方案

- 来打 ASCII 最高位的主意，还剩了 128个怎么办？
- ISO/IEC 8859 -1到ISO/IEC 8859 -16有16种方案,  很杂乱，从欧洲字符到希腊犹太的字符解决方案都有
- EASCII很少用了，
- 8859-1方案用的多一些：128~256 后面主要用来表示了欧洲的一些字符（西欧，包括德法)

## GB/简体中文/cjk等方案

### GB2312的发展

- 解决 ascii支持的符号太少的问题
- 符号集早期不足1W个汉字：GB2312

	- -->发展到GBK

		- -->GB18030, 支持到少数民族，扩展到4字节

- 多字节码元，带来了一系列问题和更麻烦的字节序问题

### 全角/半角

- 全角字符是中文显示及双字节中文编码的历史遗留问题

	- 早期点阵显示器像素不行，显示汉字看不清，就用2倍像素宽度来显示汉字
	- 为了美观，? ! 。等标点为了和汉字保持宽度，所以就这样了，带来了全角/半角的标点
	- 中文输入法下 全角半角是一样的，西文输入法下全角才和中文输入法下一样
	- 发现有些很常用 ，。！？就倍被中日韩等统一使用，有些逐渐不用了

- Unicode 兼容了所有这些遗留问题， UTF8，UTF16中，全角半角的关系不在是简单的1字节2字节的关系

### GB1300

- GB13000.1-1993被 GB13000 2010 代替
- 再BMP上和 ISO/IEC 10646:2003(兼容Unicode)统一

### GBK

- 简单粗暴：认为高位大于127,就用双字节来表示
- 兼容老的 GB1300.1-1993, 但是有些生僻的没有包含，包含了片假名，希腊等一些字符
- 子主题 3

### GB1380

###  CJK：统一中日韩一些共用的字符

## 汉字的一些坑爹复杂方案/用GB2312展开

### 区位码

- 被分为94个区，每区94位，即94*94的大表
- 行区列位:如 “万” 就是 45(高)区82(低)位
- 1）01~09区(682个)：特殊符号、数字、英文字符、制表符等，包括拉丁字母、希腊字母、日文平假名及片假名字母、俄语西里尔字母等在内的682个全角字符；

2）10~15区：空区，留待扩展；

3）16~55区(3755个)：常用汉字(也称一级汉字)，按拼音排序；

4）56~87区(3008个)：非常用汉字(也称二级汉字)，按部首/笔画排序；

5）88~94区：空区，留待扩展。

### 国标码

- 为了避开Ascii 0~31,和空格
从而把区位号，各加32
- 区位码 + 32（20H）
- 为什么要这样：为了全角/半角的坑爹问题，沿用Ascii中的0~31和空格，重新再编一套全角的进去

### 内码

- 由于国标码是双字节
可能会和Ascii混淆
- 所以把每个字节的最高位置1

### 外码

- 解决键盘问题，没英文那么简单
- 目前常用的外码分为以下几类：

1）数字编码，比如区位码；

2）拼音编码，比如全拼、双拼、自然码等；

3）字形编码，比如五笔、表形码、郑码等。

### 字形码

- 汉字在显示器或打印机上输出的解决办法
- 16×16点阵或24×24点阵或48×48点阵
- 笔画经过点填1,反之0
- 16*16点阵，一个字形码需要32字节，即 16 * 16 / 8(想想byte的bit位长度)

## 怎样解决混乱：ANSI与代码页

### ANSI

- 各民族发明各种相互不兼容的字符集，带来混乱给 当时的微软带来难题，统一称他们为 ANSI
知道是ANSI,还要知道是哪个国家

### Code page

- 内码表，不影响unicode
- 代码页是IBM称呼计算机的BIOS所支持的字符集编码又叫OEM，后来图形化的革命，微软定义了一系列Code page
- 简体中文代码页在936
即GB系列， 称为CP936，修改系统文字就是修改CP页码
- 字符->字节，字节->字符间翻译，
如 读到了一个字节序列，去指定Code Page页里查表
- 再确定不用Unicode去跑程序时，才去CodePage查

## 实现统一： unicode/ISO/IEC 10646-1 （ucs） 字符集

### 字符编码方案的演变与字节序

- Unicode出世后，字符集和编码方式解耦合
- 同一字符编码方式CEF的码元序列，在计算机实际处理、存储和传输时，还需再次编码转换为字符编码模式CES的字节序列
- 再次讨论CEF,CES

### 相似/同类字符集

- ucs

	- Unicdoe2.0时代，基本同频了UCS
	- ucs-2 ： 双字节存储码点
	- ucs-4：4字节存储码点

### 码点

- unicode码点分布在17个平面上
所有码点的范围被称为码空间

	- 为每个符号定义了码点
	- 码点分布在17个平面
	- U+0000~U+FFFF  2的16次方-1, 65536个码点，基本平面（BMP）

		- BMP平面的专用区(Private Use Zone)：0xE000~0xF8FF，6400个码点

		  永远不会被分配给任何字符

		- 代理区(Surrogate Zone)的特殊区域：0xD800-0xDFFF(十进制55296~57343)，共2048个码点

		  目的是用基本平面BMP中的两个码点“代理”表示BMP以外的其他增补平面的字符、

	- U+01FFFF~U+10FFFF，16个辅助平面(SMP等)

	  #0平面：基本多文本平面 0000~ffff
	  #1平面：多文种补充平面 U+10000  ~U+1FFFF SMP
	  #2平面：表意文字补充平面 U+20000  ~U+2FFFF SIP
	  #3平面：表意文字第三平面 TIP
	  #4~#13平面：U+4FFFF~U+DFFFF，尚未使用
	  
	  #14平面：U+E0000 - U+EFFFF 特别用途补充平面 ssp
	  
	  #15平面：U+F0000~U+FEEEE 私人用途平面 pua-a
	  
	  #16平面 U+100000~U+10FFFF私人用途平面b pua-b

### UTF编码

- 字符编码方式CEF

	- 码点值进行编码处理时作为一个整体来看待的最小基本单元(基本单位)。
	- （最小基本单元）：1个或多个码元
	- 码点的具体实现方式
	- UTF8

- 码元序列

	- 就是一组码元

- 字节序列

	- CES:字节序列，码元序列在物理存储上的10序列，考虑到大小端等问题

		- 关于方向的维度

			- 人类读写数字的方向：从左到右的方向阅读显而易见： 大端/头端/高位--->小端/尾端/低位；或上--->下，大端/头端/高位--->小端/尾端/低位；
			- 内存地址的增长方向：、从左到右为的方向被定义为      低地址--->高地址；或上--->下，低地址--->高地址
			- 内存：从上到下依次增长，地位在上，高位在小
			- 端：对应人的读写顺序
上/下，低地址/高地址：对应机器
			- 注意内存的“上”是低地址位，一维方格的左边是低地址位，也称上

		- 又称端序，就是读取01时的顺序问题
		- 三种分类

			- 大端序

				- 大端，“从左--->到右”

					- 大端在上(大端在低地址位)，
也称高尾端字节序
					- 内存增长的角度看，左端是低地址位

				- 关于内存地址的上下：
上：低地址，高位
下：高地址，低位

			- 小端序

				- 小端。 521 ： 215  “从右-->到左边”

					- 小端（人类角度字节的低权位）在上（内督促你的低地址位），小端在上
					- 小端在上：即，512，在内存中的01序列从低到高排列为   215， 注意是从低到高
					- 大端在下（大端在高地址位）
					- 低尾端字节序
“ 端 ” 就是人类读写的顺序

			- 中间序

- UTF8

	- 单字节码元？怎么还用了3个字节表示某个汉字？

		- 需要分清字节与码元
		- UTF8一个字节一个字节的读取
		- 因为1个字节就可以表达完整意图

	- 编码方式

		- 变长编码1~6个字节
		- 单字节符号，字节的第一位为 0，后7位和unicode相同
		- n字节符号：第一个字节的前N位设为1，N+1位设为0, 后面所有字节的前两位设为10,剩下的就是unicode码点 110xxxxx 10xxxxxx，双字节，1110xxxx 10xxxxxx 10xxxxxx,三字节符号
		- 最长6字节，unicode到UTF-8最好先查个表，看看unicode分类在第几档里

			- 查表：就是查码点需要多少个单字节码元
			- 临界值算法

				- 单码元： 127个有效位
				- 双码元：110, 10 ,合计减去5个占位bit,有效位 2的11次方 -1 2047位
				- 以此类推： 65535
				- 2097151

	- 没有字节序问题：因为单个用码元来编码
	- 也有一些缺点

		- 使用正则表达式难以索引
		- 程序内部处理复杂

	- UFT-8，如果添加了BOM，则只用它来标示该文本是由UTF-8编码方式编码的，而不用来说明字节序

- UTF16

	- 双字节或者4字节存储
	- 一个或两个双字节码元
	- 为什么会采用变长存储

		- 初期unicode基本平面的符号足以用双字节码元表示
		- 增补平面的unicode码点，双字节码元不够用了
		- 采用代理对来表示增补平面的码点
		- 利用了unicode基本平面bmp中代理区的增补码点

	- 如何表示Unicode增补平面中的所有码点

		- 关于 代理区D800~DFFF的再讨论：

			- 即1101 1000 0000 0000 到
1101 11111 1111 1111
			- 2的11次方-1，2047个位置

		- 使用2个双字节码元来表示一个增补平面的码点
		- 算法： 
====代理码元1====     ====代理码元2====

 1101 10pp ppxx xxxx        1101 11xx xxxx xxxx
		- p,x是变数，去掉01,剩下的Bit刚好能全部表示完所有增补平面的码点， 2的20次方-1
		- 为什么1101 1后面的没有利用起来，
个人猜测：1.20个Bit已经足够用
                   2.最高位Bit用来表示顺序
想法2得到验证： 
1101 10   最后一位bit 为0时被称作引导代理，反之为1时被称作尾随代理  

- UTF32

	- 和码点一一对应
	- 真的很简单，但是浪费了空间
	- 优点也有，就是检索起来速度超快


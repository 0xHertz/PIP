# 基础知识
## 知识扫盲
>
SDK: 软件开发工具包, 包含各种API
API: 供C语言调用
MFC: 使用C++语言封装的API, 供C++语言使用
>
windows三大组件
kernel: 管理内存等等
user: 管理交互
GDI: 管理图形设备
## 帮助文档
[MSDN帮助文档](https://msdn.microsoft.com)
[小甲鱼帮助文档](https://fishc.com.cn/forum.php?mod=forumdisplay&fid=255&filter=typeid&typeid=420)
## windows编程命名
匈牙利命名法

许多 Windows 程序员都使用“匈牙利标记法”作为变量命名约定。这是为了纪念具有传奇色彩的微软程序员 Charles Simonyi。

这种标记法非常简单，即变量名以一个或者多个小写字母开始，这些字母表示变量的数据型态。例如：szCmdLine 中的 sz 代表“以0结尾的字符串（StringZero）”；在 hInstance 和 hPrevInstance 中的 h 前缀表示“句柄（Handle）”；在 iCmdShow 中的 i 前缀表示“整型（Integer）”。

当命名结构变量时，可以用结构名（或者结构名的一种缩写）的小写形式作为变量名称的前缀，或者用作整个变量名。例如：msg 变量是 MSG 型态的结构；wndclass 是 WNDCLASSEX 型态的一个结构；ps 是一个 PAINTSTRUCT 结构，rect 是一个 RECT 结构。

匈牙利表示法能够帮助程序写作者及早发现并避免程序中的错误。由于变量名既描述了变量的作用，又描述了其数据型态，就比较容易避免产生数据型态不合的错误。


下表列出了在本系列视频教程中经常用到的变量名前缀：
```
前缀	数据类型
c   	char 或 WCHAR 或 TCHAR
by	    BYTE （无符号字符）
n	    short（短整型）
i	    int（整型）
x, y	int，表示 x 坐标和 y 坐标
cx, cy	int，表示 x 或 y 的长度，c 表示“count”（计数）
B 或 f	BOOL（int）；f 表示“flag”
w	    WORD（无符号短整型）
l	    LONG（长整型）
dw	    DWORD（无符号长整型）
fn 	    函数
s	    字符串
sz	    以零结束的字符串
h	    句柄
p	    指针
```
## 看懂windows乱七八糟的定义
### typedef
定义别名
[typedef](https://fishc.com.cn/forum.php?mod=viewthread&tid=46645&extra=page%3D1%26filter%3Dtypeid%26typeid%3D405)
### define
这个你熟悉
哦, 高级的定义其实你也没见过
* 单行定义
    1. 值定义
    2. 函数定义
    3. 格式化定义
* 多行定义
### ifdef条件编译
```c
#ifdef  WINDOWS
    // 如果 WINDOWS 宏被定义了，就执行这里的内容
#endif

#ifdef  LINUX
    // 如果 LINUX 宏被定义了，执行这里的内容
#else
    // 如果 LINUX 宏没有被定义，执行这里的内容
#endif 

#ifndef KECHEN
    //如果KECHEN 宏没有定义,执行这里的内容
#endif
```
## Unicode编程
ASCII(8位)不够用啊
### 解决方案
#### DBCS双字节字符集
* ANSI编码标准
> 具体讲就是最多采用两个字节进行内容存放, 前一个字节和ASCII一样, 后一字节根据不同国家进行不同的编码, 缺点是编码长度不统一
#### Unicode编码
* 统一使用俩个字节进行内容存放
```c
//在C语言中, 在字符串前加入L强制为Unicode编码
//使用setlocale()函数设定使用的Unicode字符集
    setlocale(LC_ALL, "Chs");
//c语言后来加入wchar_t类型, 使用Unicode编码
    wchar_t c = L"爱";
//使用wprintf()函数进行输出
    wprintf(L"%lc\n", c);
```
```
使用`TCHAR`类型, 如果定义了Unicode宏, TCHAR就是Unicode编码(去翻定义, TCHAR-->WCHAR-->wchar_t), 否则就是char, 也就是说自动识别
在win32中, 提供下面的函数强制使用Unicode编码
`TEXT()`函数
```
## 逻辑单位和设备单位
在学习编程绘图时，童鞋们经常遇到的问题就是：什么是逻辑单位（logical unit），什么又是设备单位（device unit）？它们又有什么关系呢？

小甲鱼在这里统一帮大家解答一下：

所谓的逻辑单位是独立于设备的，与设备点的大小无关，是实现"所见即所得"的基础。
```c
TextOut(hdc, x, y, TEXT("I love FishC.com!"), 17);
```
执行以上代码，Windows 应用程序在其客户区的 (x, y) 坐标处绘制文本，如同其他的 GDI 函数一样，这个坐标使用的是一种逻辑单位。

当 GDI 函数将输出送到某个物理设备上时，Windows 必须将逻辑坐标转换成设备单位（如显示器的像素点）。

逻辑单位和设备单位的转换是由**映射模式**决定的，映射模式被储存在设备环境中。

每个逻辑单位的大小由映射模式决定，这个逻辑单位既可以与设备单位（显示器的一个像素点）相同，也可以是一种物理单位（如 1 毫米），还可以是用户自定义的一种单位。在 Windows 应用程序中，只要与输出有关系，都要使用映射模式。

GetMapMode 函数用于从设备环境得到当前的映射模式，SetMapMode 函数用于设置设备环境的映射模式
[设备坐标与逻辑坐标](https://fishc.com.cn/forum.php?mod=viewthread&tid=64684&extra=page%3D1%26filter%3Dtypeid%26typeid%3D405)
[设备坐标系统](https://fishc.com.cn/forum.php?mod=viewthread&tid=64686&extra=page%3D1%26filter%3Dtypeid%26typeid%3D405)
# 开始学习winSDK
## first.c第一个win32程序
```c
# include<windows.h>

int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, PSTR szCmdLine, int iCmdShow) {
	MessageBox(NULL, TEXT("欢迎,这是第一个win32程序"), TEXT("打招呼"), MB_OK);
	MessageBox(NULL, TEXT("欢迎,这是第一个win32程序"), TEXT("打招呼"), MB_OKCANCEL);
	MessageBox(NULL, TEXT("欢迎,这是第一个win32程序"), TEXT("打招呼"), MB_YESNO);
	MessageBox(NULL, TEXT("欢迎,这是第一个win32程序"), TEXT("打招呼"), MB_YESNOCANCEL);
	//a messagebox with icon
	MessageBox(NULL, TEXT("欢迎,这是第一个win32程序"), TEXT("打招呼"), MB_OK | MB_ICONERROR);//使用按位或操作符
	//a messageboc with prevdefine option
	MessageBox(NULL, TEXT("欢迎,这是第一个win32程序"), TEXT("打招呼"), MB_YESNO | MB_ICONHAND | MB_DEFBUTTON2);
	//use "L" to real Unicode
	MessageBox(NULL, L"欢迎,这是第一个win32程序", L"打招呼", MB_YESNOCANCEL);
	//use "TCHAR" to real Unicode
	TCHAR* szContent = "我喜欢你";//it 's wrong!!!!!!
	// TCHAR * szContent = TEXT("我喜欢你");
	MessageBox(NULL, szContent, TEXT("表白"), MB_YESNOCANCEL);
	return 0;
}
```
## 消息机制与回调函数
### 消息机制
#### 队列消息
1. 通过消息结构msg中的第一个参数`窗口句柄`分发系统消息队列中的消息到应用程序消息队列
2. 应用程序通过`getMessage()`函数从自己的消息队列中获取消息

	* 由于`GetMessage()`函数会已知从消息队列中获取消息, 但是我们有时希望程序在没有消息时做一些其他的操作, 这个时候就需要用到消息窥探函数

	```c
	PeekMessage();//窥探消息, 使用这个函数需要将消息循环重写, 具体看RangeRect.c文件
	```
3. 通过`TranslateMessage(&msg)`函数翻译消息
4. 通过`DispatchMessage(&msg)`通过OS分发消息到对应的回调函数
5. 我们还可以通过`SendMessage()`函数发送自定义消息
```
消息队列是FIFO
特别的:
    WM_PAINT, WM_TIMER, WM_QUIT三个消息, 始终放在队列最后
    一些消息不进入消息队列, 直接由OS发送给应用程序
```

* 键盘消息
	* 击键消息
       1. WM_KEYDOWN
       2. WM_KEYUP
       3. WM_SYSKEYDOWN
       4. WM_SYSKEYUP
    * 字符消息
	   
	   告诉程序, 用户输入的是哪个字符
	   1. WM_CHAR
       2. WM_DEADCHAR
       3. WM_SYSCHAR
       4. WM_SYSDEADCHAR

	相关的参数值通过lParam和wParam传递
	```c
	虚拟键代码: 微软统一标准
	```
	* 插入符号(就是一般意义上的光标)

		windows中的光标表示, 鼠标当前位置, 所以用插入符号来描述当前键盘输入位置
		
		一个消息队列只允许拥有一个插入符号

		```c
		//5个最常用的插入符号函数
		CreateCaret();//创建和窗口相关的插入符号
		SetCaretPos();//设置插入符号位置
		ShowCaret();//显示插入符号
		HideCaret();//隐藏插入符号
		DestotyCaret();//销毁插入符号

		//搭配输入焦点消息
		WM_SETFOCUS; //窗口获得输入焦点时发送
		WM_KILLFOCUS;//窗口失去输入焦点时发送
		```
* 鼠标消息

	这里就不写出来了, 程序里都用过
	
	相关的参数值, 通过lParam传递
#### 非队列消息
一些调用产生非队列消息
### 回调函数
不是由main函数调用, 而是通过windows操作系统进行调用的函数
## 窗口
![image-20210321003836392](https://gitee.com/l-1ertz/Typora/raw/master/img/image-20210321003836392.png)
### 获取窗口各种信息
`GetSystemMetrics()`返回系统各种图形项的尺寸信息, 单位为像素
### 滚动条

![image-20210321120717903](https://gitee.com/l-1ertz/Typora/raw/master/img/image-20210321120717903.png)

分离高低十六位

![image-20210321120822700](https://gitee.com/l-1ertz/Typora/raw/master/img/image-20210321120822700.png)

添加滚动条
```c
hwnd = CreateWindow(szAppName,                                  
		TEXT("KeChen"),                                         
		WS_OVERLAPPEDWINDOW | WS_VSCROLL | WS_HSCROLL,               // 第三个参数加上 WS_VSCROLL | WS_HSCROLL就好了
		CW_USEDEFAULT,
		CW_USEDEFAULT,
		CW_USEDEFAULT,
		CW_USEDEFAULT,                                            
		NULL,
		NULL,
		hInstance,
		NULL);
```
添加滚动条操作
通过`WM_VSCROLL`和`WM_HSCROLL`消息实现

![image-20210321124459667](https://gitee.com/l-1ertz/Typora/raw/master/img/image-20210321124459667.png)
上面的API不行，系统开销太大
用下面的
`SetScrollInfo()`和`GetScrollInfo()`
并且使用`ScrollWindow`作为显示窗口, 部分重绘窗口
### 窗口绘画


## GDI详解
### GDI 原理

在 Windows 98 和 Windows NT 中，图形显示主要由动态链接库 GDI32.DLL 中导出的函数来处理。在 Windows 98 中，许多函数是 GDI32.DLL 利用 16 位 GDI.EXE 动态链接库来实现的。在 Windows NT 中，GDI.EXE 只用于 16 位的程序。

小甲鱼解读：Win7 64位 开始已经不支持 16 位程序了，但核心的架构和技术不会改变

这些动态链接库会调用你安装的视讯显示器和打印机的设备驱动程序中的一些函数。视频驱动程序会直接访问视频显示器的硬件，而打印机驱动程序则将 GDI 命令转换为各种打印机所能理解的代码或者命令。所以，不同的显示适配器和打印机要求不同的设备驱动程序。

小甲鱼解读：起初所有的厂商都保持着自己的标准，所以要用哪家的产品就要装哪家的驱动。不过现在好了，逐渐开始有了统一的规则约束这些厂商不要特立独行（像 USB 接口）

各种各样的显示设备都可以与 PC 兼容机连接。因此，GDI 的一个主要目的就是支持与设备无关的图形。Windows 程序应当毫无问题地在 Windows 所支持的任何图形设备上输出。GDI 提供了一种特殊的机制来彻底隔离应用程序和不同输出设备的特性，这样就支持与设备无关的图形。

小甲鱼解读：意思是我们的 WinSDK 编程不需要考虑硬件的物理设备，利用 GDI 就可以很好的做到“与设备无关”编程

C 的闻名之处在于它在不同操作系统和环境之间的高度可移植性。还有就是允许程序员执行底层系统函数，这是其他高级语言做不到的。就如同 C 常被当成“高级汇编语言”一样，你可以把 GDI 当成图形设备硬件的一种高层接口。

小甲鱼解读：因为 C 语言可以轻松转换为汇编语言，汇编语言跟 CPU 机器指令一一对应

如前所述，Windows 默认使用以像素为单位的坐标系统。大多数传统的图形语言都使用一个“虚拟”坐标系统，它的横轴和纵轴的范围是 0 ~ 32767。尽管一些图形语言并不允许使用像素坐标，但是 Windows GDI 允许使用任何一种坐标系统（可以使用其他依据物理测量得到的坐标系统），所以你可以使用虚拟坐标系统来保证程序与硬件独立，也可以使用设备坐标系统来完全迎合硬件的需求。

小甲鱼解读：Windows GDI 很大的一个作用就是充当“翻译官”

一些程序员认为，以像素的形式进行程序设计时，即意味着放弃了设备独立性。通过前边课程的学习，我们知道这么说不完全正确。关键在于要以设备独立的方式来使用像素，这需要设备接口语言为程序提供一种机制来查询设备硬件的特性，以便程序做出合适的调整。例如，在 SYSMETS 程序中，我们使用标准字体字符的像素大小在屏幕上安排字符。这种方法允许程序根据不同的显示适配器来调整分辨率、文本大小以及纵横比。你可以在接下来的视频教程中看到确定显示区域大小的其他一些方法。

小甲鱼解读：大家还记得 GetSystemMetrics 函数吧？

在不同的计算机上运行 C 语言程序时总会有一些细小的移植性问题。同样地，在设计 Windows 程序时多少也会带入一些设备依赖性。比如，尽管你可以在显示器上移动图形对象，但是 GDI 总体上来说只是一个静态显示系统，对动画的支持很有限。如果你需要为游戏编写复杂的动画，应当学习一下 DirectX，它提供了必要的动画支持。

小甲鱼解读：DirectX，（Direct eXtension，简称DX）是由微软公司创建的多媒体编程接口。由C++编程语言实现，遵循COM。被广泛使用于Microsoft Windows、Microsoft XBOX、Microsoft XBOX 360和Microsoft XBOX ONE电子游戏开发，并且只能支持这些平台。


温馨提示：下边内容新鱼油看后可能会导致心烦意乱或内心冲突，若有此症状请记住小甲鱼的一句话“以下内容仅需知道，暂不要求掌握”

### GDI 函数调用

GDI 包含有几百个函数，可以分成下面几大类。


#### 获取（或建立）和释放（或销毁）设备环境的函数

我们在前面的教程中已经接触过，你在窗口上绘图时需要一个设备内容句柄。BeginPaint 和 EndPaint 函数（尽管在技术上它们是 USER 模块而不是 GDI 模块的一部分）允许你在处理 WM_PAINT 消息时做到这一点。在处理其他消息时，可以通过调用 GetDC 和 RealseDC 函数来达到相同的目的。后边的教程小甲鱼会介绍其他一些有关设备环境的函数。
* 获取设备上下文的方法
```c
//方法一:
hdc = BeginPaint(hwnd, &ps);
//使用hdc的代码
EndPaint(hwnd, &ps);

//方法二:
hdc = GetDC(hwnd);
//使用hdc的代码
ReleaseDC(hwnd);
```
* 使用hdc的方法
	1. 显示文本
	```c
	DrawText(hdc, TEXT("大家好，这是我的第一个窗口程序！"), -1, &rect, DT_SINGLELINE | DT_CENTER | DT_VCENTER);
	//或者
	TextOut();//可以通过GetTextAlign()或者SetTextAlign()修改坐标的基准点
	```

#### 获取设备环境信息的函数

在前边的 SYSMETS 程序中，我们使用 GetTextMetrics 函数来取得有关设备内容中目前所选字体的尺寸信息。

#### 绘图函数

显然，在所有前提条件都得以满足之后，这些函数是真正重要的部分。在前边的教程中，我们使用 TextOut 函数在窗口的显示区域显示一些文字（I love FishC.com!）。在紧接着的教程中，我们还将学习到使用其它 GDI 函数绘制线条和填充区域。

#### 设置和获取设备环境属性的函数

设备环境的属性确定绘图函数在绘制时的各种细节。在 SYSMETS 程序中，我们使用 SetTextAlign 函数来通知 GDI，TextOut 函数中文本字符串的起始位置应当在字符串的右边，而不是默认的左边（不懂得请看这篇：如何设置文本对齐模式，并深刻检讨是否有独立完成课后作业）。

所有的设备环境的属性都有一个默认值，这个默认值在获取设备环境时就已经被设置好了。对所有的以“Set”开头的函数，都有相应的一个以“Get”开头的函数用于获取当前设备环境的属性。

#### 使用 GDI“对象”的函数

GDI 在这里变得有点混乱。首先举一个例子：在默认情况下，使用 GDI 绘制的所有直线都是实线，并具有一个标准的宽度。你可能希望绘制更细的直线，或者是由一系列的点或短划线组成的直线。但直线的宽度和画笔的样式并不是设备环境的属性。然而，它们是“逻辑画笔”的特征。你可以通过在 CreatePen、 CreatePenIndirect 或 ExtCreatePen 函数中指定这些特征来建立一个逻辑画笔，这些函数传回一个逻辑画笔的句柄（虽然这些函数被认为是 GDI 的一部分，但是和大多数 GDI 函数调用不一样，它们不要求设备内容的句柄）。要使用这个画笔，就要将画笔句柄选进设备环境中。

我们认为，设备环境中目前选中的画笔就是设备环境的一个属性。这样，您画任何线都使用这个画笔，然后，您可以取消设备内容中的画笔选择，并清除画笔对象。清除画笔对象是必要的，因为画笔定义占用了分配的内存空间。除了画笔以外，GDI 对象还用于建立填入封闭区域的画刷、字体、位图以及 GDI 的其它一些方面使用 GDI 对象。


### GDI 的基本图形

在屏幕上或者打印机上显示的图形类型可以分为下面几类，被称为“基本图形”。


#### 线条和曲线

线条是所有矢量图形绘制系统的基础。GDI 支持直线、矩形、椭圆（包括特殊的椭圆，也就是我们所说的圆形）、椭圆弧线（椭圆圆周上的部分曲线）以及贝塞尔样条曲线（Bezier spline），我们将在接下来的教程中分别对它们进行介绍。如果需要绘制一个不同类型的曲线，你可以把它绘制成折线代替（折线就是通过一组非常短的直线来定义一条曲线）。GDI 使用当前选入设备环境的画笔绘制线条。

#### 可被填充的封闭区域

当一系列的线条或者曲线构成一个封闭的区域时，该区域就可以使用当前的 GDI 画刷对象进行填充。这个画刷可以是纯色的，或者是使用某种填充模式（如一系列水平的、垂直的或者倾斜的图案），还可以是在水平或垂直方向不停重复的位图图像。

#### 位图

位图是位的矩形数组（二维数组），每一个位对应于显示设备上的一个像素。位图是光栅图形的基础。位图通常用于在视频显示器或者打印机上显示复杂（一般都是真实世界）的图像。位图也通常用于显示必须要快速绘制的小图像（诸如图标、鼠标光标以及在应用工具条中出现的按钮等）。GDI 支持两种型态的位图－旧式的（虽然还非常有用）“设备相关”位图，属于 GDI 对象；新式的“设备无关”位图（DIB），可以储存在磁盘文件中。

#### 文字

文字的数学味道不像计算机图形的其它方面那样浓。文字和几百年的传统印刷术有关，它被许多印刷工人看作为一门艺术。因此，文字通常不仅是所有的计算机图形系统中最复杂的部分，而且（如果识字还是社会基本要求的话）也是最重要的部分。用于定义 GDI 字体对象和取得字体信息的数据结构是 Windows 中最庞大的部分之一。从 Windows 3.1 开始，GDI 开始支持 TrueType 字体，该字体是在填入轮廓线基础上建立的，这样的填入轮廓线可由其它 GDI 函数处理。


### 其他

GDI 的其他方面就不太容易分类了，具体如下。


#### 映像模式（mapping mode）和转换（transform）

尽管在默认时是以像素为单位进行绘图，但是你并非别无选择。GDI 的映像模式允许你以英寸（或者甚至以几分之一英寸）、毫米或者任何您想使用的单位来绘图（Windows NT 还支持传统的以 3 * 3 矩阵表示世界坐标变换（world transform）。 这用于倾斜和旋转图形对象。

#### 图元文件（metafile）

一个图元文件是以二进制形式储存的 GDI 命令的集合。图元文件主要用于通过剪贴板转换矢量图形绘制的表现形式。

#### 区域（region）

区域是一个任意形状的封闭图形，通常定义为较简单的绘图区域组合。在 GDI 内部，绘图区域除了储存为最初用来定义绘图区域的线条组合以外，还以一系列扫描线的形式储存。您可以将绘图区域用于绘制轮廓、填入图形和剪裁。

#### 路径（path）

路径是GDI内部储存的直线和曲线的集合。路径可以用于绘图、填入图形和剪裁，还可以转换为绘图区域。

#### 剪裁（clipping）

当绘图被限制在客户区的一个特定的空间位置时，就发生了剪裁。那个特定的空间位置可以是矩形或者非矩形，它通常被指定为一个区域或者一个路径。

#### 调色板（palettes）

仅在支持 256 种颜色时，才能使用自定义的调色板。Windows 仅保留这些色彩之中的 20 种以供系统使用。您可以改变其它 236 种色彩，以准确显示按位图形式储存的真实图像。

#### 打印（printing）

尽管在本章教学中我们只限于讨论视频显示器，但是你在本章中所学到的所有知识几乎都可以应用于打印机。

### GDI使用
#### 绘制点
`SetPixel()`设置像素点颜色, 还有`SetPixelV()`, 这个比较快(返回值简单)
> 参数中最后一个是`COLORREF`类型, 有4个字节, 低三个字节分别代表RGB三原色值, 高字节填充0
`GetPixel()`获取像素点颜色
#### 绘制线
* 基础
```c
	//画线
		for (i = rect.left; i <= rect.right; i++) {
			SetPixel(hdc, i, 110, RGB(255, 0, 0));
		}
```
* 使用封装好的API
```c
//设置画笔属性/画刷属性
//画刷用来填充封闭图形
SelectObject()//应用画笔属性
GetStockObject()//设置画笔属性
```
```c
//自己创建画笔/创建自己的画刷
CreatePen()//直接创建画笔
SetBkColor()//设置画笔对象的背景色, 主要用来填充非实线画笔的空隙
SetBkMode()//设置画笔对象背景模式, 有透明和不透明两种
GetBkMode()//获得画笔背景模式
CreatePenIndirect() & LOGPEN结构体//通过LOGPEN结构体间接创建画笔

//创建画刷
CreateSolidBrush()//创建实心画刷
CreateHatchBrush()//创建阴影画刷
SetPolyFillMode()//设置画刷填充模式
CreateBrushIndirect() & LOGBRUSH//通过LOGBRUSH某结构体创建画刷
```
```c
//删除画笔
DeleteObject()//删除画笔
```
```c
//设置绘图模式, 就是背景和画笔颜色怎么叠加, eg:颜色反转
GetROP2()
SetROP2()
```
```c
// 绘制直线
MoveToEx()//起点
LineTo()//终点
GetCurrentPositionEx()//获得当前画笔位置
```
```c
//绘制折线
PolyLine()//绘制一条折线, 不改变画笔位置
PolyLineTo() //绘制一条折线, 改变画笔位置
PolyPolyLine() // 绘制多条折线
```
```c
//绘制贝塞尔曲线
PolyBezier()
PolyBezierTo()
```
#### 绘制各种图形
```c
Rectangle()//绘制矩形
Ellipse()//绘制三角形
RoundRect()//绘制圆角矩形
Arc()//绘制弧
Chord()//绘制弧和弦构成的封闭图形
Pie()//绘制弧和半径构成的封闭图形
Polygon()//绘制封闭多边形
PolyPolygon()//绘制多个封闭多边形
```
windows中专门用来处理矩形的函数
```c
FillRect();//填充矩形
FrameRect();//填充边框
InvertRect();//反转矩形内像素

//处理矩形相关的RECT结构
SetRect();//设置RECT.x, RECT.y等, 一句话就是设置RECT结构
OffsetRect();//对指定的矩形坐标进行移动
InflateRect();//缩放矩形
SetRectEmpty();//将矩阵坐标全部设置为0
CopyRect();//将一个矩形坐标拷贝到另一个矩形坐标
IntersectRect();//两个矩形取交集
UnionRect();//两个矩形取并集
IsRectEmpty();//判断矩形是否坐标全为0
PtInRect();//检测一个点,是否在矩形中, 很常用的一个API
```
绘制区域对象
```c
//HRGN数据类型

//绘制区域

//区域有什么用????用画笔或者图形也能画出来, 作用在于, 区域可以合并, 布尔运算

//裁剪区域, 指定一个区域最为当前设备环境的裁剪区域

```
## Windows字符串处理函数
本质是从C语言发展过来的
* 基础函数==不安全==
  `wsprintf()`输出
  `lstrlen()`长度
  `lstrcpy()`复制
  `lstrcat()`拼接

* 安全函数==给出字符个数==
  头文件`strsafe.h`
  `StringCchPrintf`
  `StringCchLength`
  `StringCchCopy`
  `StringCchCat`
  上面的`Cch`替换为`Cb`就是另一种, `Cb`要给出==字节个数==麻烦

* 字符串尺寸

	![image-20210321112136826](https://gitee.com/l-1ertz/Typora/raw/master/img/image-20210321112136826.png)
	通过`GetTextMetrics()`API填充`TEXTMETRIC`结构体, 该结构体规定了上图的一些属性

## GUI映射模式
API: 
`SetMapMode()`
`GetMapMode()`
![image-20210321220638101](https://gitee.com/l-1ertz/Typora/raw/master/img/image-20210321220638101.png)
## 视口和窗口
编程时, 使用逻辑坐标, 在显示时, windows将逻辑坐标(单位与映射模式相关)转换为设备坐标(像素为单位)
* 窗口: 逻辑坐标
* 视口: 设备坐标
API:
`SetViewportOrgEx()`指定映射到**窗口**原点的设原点的坐标
`SetWindowOrgEx()`指定映射到设备原点的窗口原点的坐标
* 逻辑坐标和设备坐标互换
### 设备坐标和逻辑坐标手动转换 
API:
`DPtoLP()`将设备坐标转换为逻辑坐标，常用

`LPtoDP()`将逻辑坐标转换为设备坐标

### MM_ISOTROPIC各向同性&MM_ANISOTROPIC各向异性模式

应用非常广泛

* 简单来说：前者在用户通过`SetWindowExtEx()`设置好窗口尺寸, `SetViewportExtEx()`设置好视口尺寸, 然后windows系统会自动裁剪视口以适应窗口
* 后者相比前者少了windows系统对视口的管理, 直接根据视口大小进行比例拉伸
* 相同之处是: 这两种映射模式比前几种更加自由, 可以自定义坐标轴方向

```c
SetViewportExtEx(hdc, cxClient/2, cyClient/2 ,NULL);
//在这里修改映射的视口值为负就可以改变方向了
SetViewportExtEx(hdc, cxClient/2, -cyClient/2 ,NULL);
```
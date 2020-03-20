网络资料整理

https://blog.csdn.net/yangchun1213/article/details/7722520
1、两种方式访问WPF图像处理API：托管组件和非托管组件(使用于新的或专用图像格式的扩展性模型)。
2、位图(BMP)、联合图像专家组(JPEG)、可移植网络图形(PNG)、标记图像文件格式(TIF)、Microsoft Windows Media照片、图形交换格式(GIF)、图标(ico)。
3、编解码器用于对特定媒体格式解码或编码。
4、BitmapSource，用于对图像进行解码和编码。是WPF图像处理管线的基本构造块，表示具有特定大小和分辨率的单个不变的像素集。是BitmapFrame的父级。
5、BitmapFrame用于存储图像格式的实际位图数据。
6、解码器的选择是基于图像格式做出的。编码器的选择是自动做出的。
7、使用非托管WPF图像处理界面开发并向系统注册的自定义格式解码器会自动加入到解码器选择队列。
8、在WPF中显示图像，Image控件，ImageBrush在可视图面上绘制图像或使用ImageDrawing绘制图像。设置BitmapImage的DecodePixelWidth或DecodePixelHeight(Image.Source需要的宽高)降低内存使用。
9、BitmapImage是一个专用的BitmapSource，已经优化为用于扩展应用程序标记语言(XAML)加载。是一种将图像显示为Image控件的Source的简便方式。
10、BitmapImage实现ISupportInitialize接口，以对多个属性的初始化进行优化。只能在对象初始化过程中进行属性更改。调用BeginInit以表示初始化开始；调用EndInit以表示初始化结束。
11、旋转、转换和裁切图像,使用BitmapImage的属性或使用其他BitmapSource对象(如CroppedBitmap或FormatConvertedBitmap)来转换图像。
12、BitmapImage的Rotation属性来执行图像旋转。旋转只能以90度的增量进行。
13、FormatConvertedBitmap将图像转换为不同的像素格式，如灰度。
14、Image或CroppedBitmap的Clip属性，裁切图像。
15、拉伸图像，Strecth。
16、绘制图像，Brush。ImageBrush是一种TileBrush类型，用于将其内容定义为位图图像。
17、图像元数据，每个图像格式处理元数据的方式不同，WPF图像处理提供一种统一的方式来为每个支持的图像格式存储和检索元数据。
18、通过BitmapSource对象的Metadata属性来进行。
19、WPF支持的图像元数据架构：可交换图像文件(Exif)、tEXt(PNG文本数据)、图像文件目录(IFD)、国际新闻通信委员会(IPTC)和可扩展元数据平台(XMP)。
20、BitmapMetadata提供若干属性。元数据查询读取器提供对读取元数据的其他支持。GetQuery方法，通过提供字符串查询。SetQuery获取产寻编写器并设置需要的值。
21、编解码器扩展性，说明：编解码器必须进行数字签名，以便于系统识别。


1、图像信息保存在Frames(大部分图像(jpg，bmp，png等)只有一帧，但GIF，ico等图像有多帧)属性中。

https://www.cnblogs.com/xrwang/archive/2010/01/26/TheComparisonOfImageProcessingLibraries.html (图像处理类库比较)
1、OpenCv(BSD)、EmguCv(GPLv3,付费)、AForge.net(LGPLv3，原生.net库)。
2、OpenCv：http://sourceforge.net/projects/opencvlibrary/files/，VC下使用需要重新编译。大多数功能都以C风格函数形式提供。
3、图像处理大部分时间都用于内存读写及运算(特别是矩阵)。

整理完成了C# WinForm开发系列 - 图形图像处理文章
https://bbs.csdn.net/topics/310177057

完全免费的矢量图标网站阿里妈妈iconfont，超级好用。

《C#数字图像处理算法》典型实例，赵春江，人民邮电出版社。

https://blog.csdn.net/aobannie0463/article/details/101118518?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task
1、C#下完指针+Struct，和C没啥区别。图像处理这样的基本类型简单的程序，非常适合用C#编写。大量的指针，大量用非托管内存，可最大化性能。最小化内存占用，最小化GC对程序的影响。
2、使用硬件。对图像处理加速最有效果的是GPU。无界面的情况下调用GPU，需要引用XNA。
3、进一步就是写HLSL了。
4、https://www.cnblogs.com/xiaotie/archive/2010/03/08/1680662.html(高手)
5、https://blog.csdn.net/weixin_33946020/article/details/86057618
6、https://www.cnblogs.com/xdesigner/

在WPF中创建OpenGL Windows

Windows Forms 2.0 Programming (2nd Edition) (Microsoft .NET Development Series) (Paperback)
by Chris Sells, Michael Weinhardt 
http://www.amazon.com/Windows-Forms-Programming-Microsoft-Development/dp/0321267966/ref=pd_bxgy_b_img_b/002-4945107-4387252
Data Binding with Windows Forms 2.0: Programming Smart Client Data Applications with .NET (Microsoft .NET Development Series) (Paperback)
by Brian Noyes 
http://www.amazon.com/Data-Binding-Windows-Forms-2-0/dp/032126892X/ref=pd_bxgy_b_img_b/002-4945107-4387252


如何学习C++图像处理
1、机器视觉，小公司，商业图像处理库，德国的Halcon或美国康耐视公司的VisionPro。类似Matlab。要钱，一两万。大公司，自己写处理算法，用OpenCV。
2、刚萨雷斯的教材，远远不能涵盖OpenCV3.0时代的所有modules。官方文档。
3、TensorFlow，学习成本，但在RNN尤其灵活。深度学习的caffe，TS，和图像沾边的OpenCV是一个不可少的开源库。
4、OpenCV算法分两大类，图像算法如feature2d，机器学习算法ml。需要了解算法的原理。
5、机器学习，李航的统计学习法，周志华的机器学习。
6、专栏：【OpenCV】入门教程 https://blog.csdn.net/zhmxy555/category_9262318.html
7、工程驱动，针对实际问题提出具体的解决方案。
8、图像方面的如classification，detection等任务，用深度学习会更方便，更简单。直接使用caffe的c++接口，再加WPF完成桌面程序。
9、其他算法可以自己写，匹配定位绕不开。需要底层算法是那些嵌入式，或产品量巨大。

https://blog.csdn.net/qq_27825451/article/details/84133943 OpenCV3编程入门 OpenCV3计算机视觉python语言实现

使用WriteableBitmap提高WPF图形绘制性能
1、进行大批量图形数据绘制，利用WriteableBitmap结合GDI+和WPF图形绘制方法，能够大幅度提高图形绘制的效率。
2、BMP(Bitmap)是Windows操作系统中的标准图像文件格式。两类：设备相关位图(DDB)和设备无关位图(DIB).采用位映射存储格式，除了图像深度可选之外，不采用其他任何压缩(占用空间大)。1bit、4bit、8bit、24bit。
3、Image空间加载Bitmap，需要转为ImageSource类型或BitmapImage的类型。
4、《C#网络应用编程》第二版 马俊 人民邮电出版社。https://blog.csdn.net/xpj8888/article/list/2?t=1
5、Uri表达式的一般形式：协议+授权+路径。
6、https://blog.csdn.net/jiuzaizuotian2014/article/details/81279423
7、注意WPF中带有图像PNG的DPI-图像比例奇怪或模糊，WPF默认为i96dpi。将SnapsToDevicePixels设置true。
8、https://docs.microsoft.com/en-us/previous-versions/aa970908(v=vs.100)?redirectedfrom=MSDN

https://blog.csdn.net/WPwalter/article/details/103760445
1、、WPF控制渲染的部分，D3DImage(用来承载使用DirectX各个版本渲染内容的控件)；WriteableBitmap(内存空间来指定如何渲染一个位图的图片)；HwndHost(承载一个子窗口以便能叠加任何种类渲染的控件)。
2、使用 CompositionTarget.Rendering 逐帧渲染以评估其渲染帧率
3、使用 Benchmark 基准测试来测试内部各种不同方法的性能差异。
4、WPF应用程序中的像素捕捉。当边缘的位置落在器件像素的中间而不是器件像素之间时，通常会被视为模糊或半透明的这些伪像。使视觉树中的对象边缘可以通过像素捕捉来捕捉或固定到设备像素，从而消除了抗锯齿产生的半透明边缘。
5、注意：SnapsToDevicePixels仅影响通过布局遍历运行的元素。可以使用DrawingGroup对象的GuidelineSet属性在图形上设置参考线。若要手动设置Visual的准则，请使用VisualYSnappingGuidelines和VisualXSnappingGuidelines属性创建新准则。
6、Windows Presentation Foundation（WPF）检测到任何类似于动画的移动（例如滚动，缩放或动画平移）时，将关闭像素捕捉，直到移动完成。动画或滚动运动完成后，将缓慢重新激活像素捕捉。
7、在Windows Presentation Foundation（WPF）应用程序中应尽可能避免产生高频图像。 
8、BitmapSource，图像的最大高度和宽度为2 ^ 16像素，每通道32位* 4通道。BitmapSource的最大大小为2 ^ 32字节（64 GB），最大图像大小为4 GB。最小图像尺寸为1x1。
9、注意：默认的并行循环对于函数体很小的情况是很慢的，这种情况必须用Partitioner 创建循环体，这在MSDN有介绍，是关键之中的关键。
10、TPL任务并行库，
11、渲染任何可能的图形的时候没有GC。
12、查看界面渲染帧率。WPF Performance Suite
13、脏区大小与 CPU 占用率之间的关系。脏区渲染是 CPU 占用的最大瓶颈（因为没有脏区仅剩内存拷贝的时候 CPU 占用为 0%）
14、分析主线程的性能分布。
15、启用基准测试（Benchmark）。
16、如果 WriteableBitmap 不渲染，那么无论设置多大的脏区都不会对性能有任何影响。
17、内存拷贝不是 WriteableBitmap 的性能瓶颈。
18、WriteableBitmap渲染原理，在调用 WriteableBitmap 的 AddDirtyRect 方法的时候，实际上是调用 MILSwDoubleBufferedBitmap.AddDirtyRect，这是 WPF 专门为 WriteableBitmap 而提供的非托管代码的双缓冲位图的实现。


https://blog.csdn.net/zuozewei/article/details/82656926
1、DWM基于Direct3D，D3D下面是WDDM驱动。
2、Windows图像渲染的基本流程：App->DXruntime->Usermodedriver->dxkrnl->Kernelmodedriver->GPU.
3、window桌面程序UI自动化测试技术，
4、Win32程序，所有窗口和控件都是一个窗口的实例，都拥有一个窗口句柄，窗口对象属于内核对象，由Windows子系统来维护(为标准控件定义了窗口类，并使用GDI来绘制这些标准控件)。采用消息循环机制。
5、WPF程序，使用DirectX绘制，不直接支持MSAA，通过UIA用桥转换技术来支持MSAA，用AutomationPeer类支持自动化，每一种控件都有对一个的AutomationPeer类。
6、AutomationPeer不直接暴露给测试客户端，而是通过UIA来使用。UIA向应用程序窗口发送WM_GetObject消息，获得由AutomationPeer实现的UIA Server端Provider。AutomationPeer由控件创建(OnCreateAutomationPeer)。
7、UIAutomation是微软从Windows Vista开始推出的一套全新UI自动化测试技术， 简称UIA。
8、https://lindexi.gitee.io。
9、WPF 应用程序从两个线程开始：一个用于处理呈现，一个用于管理 UI。呈现线程有效地隐藏在后台运行，而 UI 线程则接收输入、处理事件、绘制屏幕以及运行应用程序代码。
10、Exception from HRESULT: 0x88980406 https://blog.csdn.net/muzizongheng/article/details/47008247?locationNum=14，WPF后端的Render线程渲染UI到DirectX时崩溃。
11、https://www.cnblogs.com/clowwindy/archive/2009/02/13/1390060.html。线程安全性：可以在线程之间共享冻结的 Freezable 对象。
12、https://www.jianshu.com/p/1d19514bccea
13、 Dispatcher的作用是用于管理线程工作项队列，类似于Win32中的消息队列，Dispatcher的内部函数，仍然调用了传统的创建窗口类，创建窗口，建立消息泵等操作。
14、DispatcherObject 类有两个主要职责：提供对对象所关联的当前 Dispatcher 的访问权限，以及提供方法以检查 (CheckAccess) 和验证 (VerifyAccess) 某个线程是否有权访问对象（派生于 DispatcherObject）。
15、线程上的操作又由Dispatcher分为不同的优先级。如果不希望UI上出现卡顿的情况，就必须将UI线程的图片加载（render）操作的优先级别降到UI线程上输入（input）操作的优先级之下。也就是input操作优先于图片呈现，界面就不会出现卡死状态。
16、优秀博客，https://www.cnblogs.com/loveis715/
17、PPL（C++并行模式库 ）和TPL(C#并行模式库)，https://docs.microsoft.com/en-us/previous-versions/msp-n-p/gg675934(v=pandp.10)?redirectedfrom=MSDN
18、试试这本 Parallel Programming with Microsoft Visual C++还有对于的 C#版本 Parallel Programming with Microsoft .NET再试试跨平台的库 Threading Building Blocks |再试试gpu版本的 Thrust | CUDA ZONE
19、
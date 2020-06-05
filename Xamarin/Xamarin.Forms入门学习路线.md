# Xamarin.Forms入门学习路线

标签（空格分隔）： Xamarin

---

## Xamarin 介绍 ##
Xamarin是一套跨平台解决方案，目的是使用C#语言创造原生的iOS，Android，Mac和Windows应用。

Xamarin的三个优势：

1. Xamarin App拥有原生App的性能，因为最后生成的App中是使用的原生的控件和原生的API，所以它的体验和效率与原生App相近。
2. 使用熟悉的C#语法，在Objective-C，Swift或者Java中能做的任何事情都可以用C#做到。除此之外，C#还有强大的IDE智能提示，lambdas语法，更自然的异步语法（Task、Async），NuGet快速获取组件。
3. 在不同的平台上使用同样的语言还具有共享代码的优势，各个平台大约可以共享75%的APIs和数据结构代码。如果使用Xamarin.Forms来创建UI几乎可以共享100%的代码。

### 最终的思想，共享代码 ###
说白了，Xamarin宣称的最大的优势就是在三个平台上使用同一种语言来共享代码，总体说来有三种技术实现：

1. [Shared Projects](https://developer.xamarin.com/guides/cross-platform/application_fundamentals/shared_projects/)：可以在里面添加供三个平台公用的代码，图片和多媒体文件等，代码部分可使用`#if __ANDROID__`等条件编译符来指定哪一部分会编译输出到特定平台中。
2. [Portable Class Libraries](https://developer.xamarin.com/guides/cross-platform/application_fundamentals/pcl/introduction_to_portable_class_libraries/)(PCLs)：使用更多的还是PCLs，PCLs库直接就能被各个平台所引用，一些流行的库如SQLite，Json.NET,ReactiveUI都支持PCL。
3. [Xamarin.Forms](https://xamarin.com/forms)：支持你用C#代码来创建在三个平台上共享的UI界面，总共可以使用超过40个控件，它们都会在运行时映射为原生控件。

共享代码的关系就如下图：
![](http://images2015.cnblogs.com/blog/282687/201602/282687-20160224135616958-412345898.png)
## Xamarin 安装指南 ##
工欲善其事，必先利其器。Xamarin的安装过程参考简书上的一篇文章，内容很齐全很详细：<http://www.jianshu.com/p/c67c14b3110c>

由于墙的原因，从官网下载的安装包无法直接安装，可以通过安装包中解析出配置文件，从中获取下载路径：
[Windows下载路径](https://static.xamarin.com/installer_assets/v3/Windows/Universal/InstallationManifest.xml)
[Mac下载路径](https://static.xamarin.com/installer_assets/v3/Mac/Universal/InstallationManifest.xml)

Windows下的大体流程如下：

1. Visual Studio肯定是需要的，推荐VS2013
2. 安装jdk，修改环境变量
3. 安装Android SDK，需要修改为[国内镜像](http://www.androiddevtools.cn/)
4. 安装NDK
5. 安装GTK
6. 安装Xamarin.VisualStudio
7. 安装XamarinStudio（可选）

注意6和7的版本号很重要，必须要跟Mac端相匹配，跟破解补丁的版本也需要匹配。如果以后升级，通常只需要更新6和7就可以了。

关于Android模拟器，之前折腾过不少，最后推荐一款专用于游戏玩家的Andorid模拟器，[海马玩模拟器](http://www.droid4x.cn/)，它的性能很好很流畅，不过游戏模拟器屏幕默认是横屏的，第一次要手动改成竖屏。

Mac下的大体流程：
如果只考虑用Mac开发iOS程序，不考虑在Mac下开发Android程序，那么大体流程如下:

1. 安装MonoFramework
2. 安装monotouch
3. 安装XamarinStudio

需要注意三者之间的版本一一对应。

关于商业证书，Xamarin的[价格](https://store.xamarin.com/)是很昂贵的：

![](http://images2015.cnblogs.com/blog/282687/201602/282687-20160225091939349-1001403408.png)
上面看到的价格只是针对单用户单设备平台，通常我们使用Xamarin都希望至少能用于Android和iOS两个平台，所以价格还得乘以2。

安装完毕后如果没有购买商业证书，那么可以按照上面那篇文章来破解试用，如果使用的版本号在3.11之前，那么只需要完成离线破解，IDE不需要登陆Xamarin账号，如果版本号在3.11之后，而且要编译iOS（目的是为了连接Mac端的BuildHost，如果是在Mac上开发编译iOS则不需要），那么还需要完成在线破解，具体破解流程文章里有，大体流程如下，最后提醒一下试用完了别忘了购买官方的商业授权。
### 离线破解流程 ###

1. 软件读取机器特征码；
2. 将特征码通过邮件发给破解者，等待他回复授权证书，不付费证书有效期1个月，付费20元证书有效期10年；
3. 将证书和对应版本的破解文件拷贝到指定目录。

### 在线破解流程 ###

1. 邮件申请开通在线服务
2. 修改host的IP地址
3. 导入SSL证书
4. 登陆Xamarin账号

## Xamarin.Forms 程序结构 ##
![](http://images2015.cnblogs.com/blog/282687/201603/282687-20160301171921673-1640559633.png)
程序的目录结构大致就可以参考这个图，最顶上一层表示三个特定平台的工程，第二层表示一个PCL或者SAP工程，通常也是Forms所在的工程，然后引用两个核心库Xamarin.Forms.Core和Xamarin.Forms.Xaml，然后特定平台的工程还要引用两个特定平台的库，这个特定平台的库可以让程序集使用特定平台的API。
## Xamarin.Forms 官方Demo ##
Xamarin提供了很多学习用的Demo，地址是：<https://developer.xamarin.com/samples-all/>。不过官网的网速确实太慢，在GitHub上还有更多更全的Forms的Demo：<https://github.com/xamarin/xamarin-forms-samples>。
其中我认为几个比较重要的Demo可以学习一下：

* [CustomRenderers](https://github.com/xamarin/xamarin-forms-samples/tree/master/CustomRenderers)：教你怎样重写Forms里的一些原生控件的样式；
* [Forms2Native](https://github.com/xamarin/xamarin-forms-samples/tree/master/Forms2Native)：教你怎样从Forms页面跳转到Native页面；
* [Native2Forms](https://github.com/xamarin/xamarin-forms-samples/tree/master/Native2Forms)：教你怎样从Native页面跳转回Forms页面；
* [FormsGallery](https://github.com/xamarin/xamarin-forms-samples/tree/master/FormsGallery)：里面有几乎全部的Forms控件展示；
* [Navigation](https://github.com/xamarin/xamarin-forms-samples/tree/master/Navigation)：教你Forms的App页面导航跳转是怎么回事；
* [UsingDependencyService](https://github.com/xamarin/xamarin-forms-samples/tree/master/UsingDependencyService)：教你使用依赖服务在Forms里调用Native的方法；
* [XamFormsImageResize](https://github.com/xamarin/xamarin-forms-samples/tree/master/XamFormsImageResize)：教你图片尺寸相关的东西；

## Xamarin.Forms官方文档 ##
Xamarin官方提供了一套很全的在线学习指南，地址是：<https://developer.xamarin.com/guides/xamarin-forms/getting-started/>，这份指南目录结构良好，便于快速查看，从怎样开始第一个程序到后面怎样到商城发布一应俱全。
还有一个学习途径就是官网教材，可以免费下载离线版：<https://developer.xamarin.com/guides/xamarin-forms/creating-mobile-apps-xamarin-forms/>，教材的随书Demo地址：<https://github.com/xamarin/xamarin-forms-book-preview-2>。这本教材支持Forms1.3以上，并且章节一直在保持更新，截至2016/02/25已发布到24章，Demo的核心库已更新到2.0并且加入了UWP工程。
如果说在线学习指南可以帮助你快速入门，那么这本教程可以帮助你更细化的理解Forms程序。
下面我将24章的官方教材的目录做个简单介绍，后面有时间也会对重要的几章做个更详尽的剖析：

1. How Does Xamarin.Forms Fit In?（Forms适合什么场景）
2. Anatomy of an App（剖析一个FormsApp）
3. Deeper into Text（深入文本）
4. Scrolling the Stack（滚动面板）
5. Dealing with Sizes（处理尺寸大小）
6. Button Clicks（按钮点击）
7. XAML vs. Code（创建UI的两种方式）
8. Code and XAML in Harmony（XAML和代码的协调合作）
9. Platform-Specific API Calls（平台特定的API调用）
10. XAML Markup Extensions（XAML扩展标记语言介绍）
11. The Bindable Infrastructure（绑定的基础知识）
12. Styles（样式）
13. Bitmaps（位图）
14. Absolute Layout（绝对布局）
15. The Interactive Interface（交互控件）
16. Data Binding（数据绑定）
17. Mastering the Grid（熟练掌握Grid布局）
18. MVVM（数据绑定开发模式Mvvm讲解）
19. Collection Views（集合控件讲解--List）
20. Async and File I/O（异步I/O操作文件）
21. Transforms（变换---缩放、定位等）
22. Animation（动画）
23. Triggers and Behaviors（触发器和行为）
24. Page Navigation（页面导航）

其中我感觉有几章比较重要，如果对Xaml（WPF主要用的界面标记语言）开发不太熟悉的同事需要看一下这几章：

* 7.XAML vs. Code：了解Xaml和Code两种方式来创建UI界面
* 8.Code and XAML in Harmony：了解XAML和后台代码如何协同工作
* **10.XAML Markup Extensions：了解扩展标记语言**
* 11.The Bindable Infrastructure：了解绑定的基础知识
* 16.Data Binding：更深入的了解数据绑定
* **18.MVVM：了解基于数据绑定的UI开发模式Mvvm**

要对Forms的细节有深入理解看下面几章：

* 3.Deeper into Text：深入理解文本
* **5.Dealing with Sizes：深入理解如何处理尺寸大小，重点也是拿文本举例，教你如何理解移动开发里面像素、物理尺寸（英尺、厘米）、[DPI](http://baike.baidu.com/link?url=gTHZg3zK0U2XUIMpkUUtW9rhOTuPd2Rz-jFfkgWvrBdKX1gPd6DwxoBK-SbcRtU42TFRKq4CU5OPxuYZ_cBXnnWk2dOngpUl9DqSU1o-Y67)、DIU，主要思想反正就是不要去关注表示大小的那些数值，字体应该使用字体枚举，布局应该是用比例去控制，要充分相信Xamarin平台能帮你控制好大小尺寸。**
* **13.Bitmaps：了解怎样在Forms中使用图片，也是满满的都是坑，显示在界面上的图片体积一定要尽量的小，不要将一张原始尺寸的图片加载成缩略图然后放在列表中显示，否则程序一定会内存溢出，一定要对图片进行裁剪，将适合的体积的图片用在适合的地方。从这一章中还可以学习图片在具体平台下的用法和差异等。**
* **19.Collection Views：了解集合控件，列表在App当中用得非常普遍，所以应当着重了解。**
* 20. Async and File I/O：在Xamarin中只能使用异步IO（或者说是PCL中只能使用异步IO），从趋势看未来的.Net Core可能也只支持异步IO、异步Http请求等，感觉这种更重视性能的IO思想是未来框架的趋势，所以可以借此熟悉一下，C#的异步语法应该算是众多编程语言中的佼佼者了。

下面对第五章Dealing with Sizes稍作讲解，这章重点介绍了移动平台下尺寸相关的一些知识，先看下下面两个表格：

|**型号**|iPhone 2，3|iPhone 4|iPhone 5|iPhone 6|iPhone 6 Plus|
|---|---|---|---|---|---|
|**像素尺寸**|320x480|640x960|640x1136|750x1134|1080x1920|
|**屏幕尺寸**|3.5英寸|3.5英寸|4英寸|4.7英寸|5.5英寸|
|**像素密度**|165 DPI|330 DPI|326 DPI|326 DPI|401 DPI|
|**单位点包含像素数量**|1|2|2|2|3|
|**点数尺寸**|320x480|320x480|320x568|375x667|414x736|
|**每英寸包含点数量**|165|165|163|163|154|

<br>

|**屏幕类型**|WVGA|WXGA|720P|1080P|
|---|---|---|---|---|
|**像素尺寸**|480x800|768x1280|720x1280|1080x1920|
|**缩放比例**|1|1.6|1.5|2.25|
|**DIUs尺寸**|480x800|480x800|480x853|480x853|

第一张图是iPhone下的一些尺寸元素间的关系，第二张是WinPhone的，这里没有给出Android的，其实Android整体上说来跟iPhone的那些参数很相似。
Forms中真正使用的不是像素，而是点数，点里面包含的像素数量是不一致的，像iPhone2，3基本上是一一对应，一个点包含一个像素，iPhone4，5，6就是两倍像素，iPhone6Plus就是三倍像素，所以iPhone的图片里出现`@2x`，`@3x`这些标识就是对应平台所使用的像素不同的图片。我们在Forms中使用的那些表示宽高的值就是这种点数单位，要知道设置的这些值可以获取整个页面的Width和Height值。
下面说下字号，Forms提供了几种枚举字号：Default，Micro，Small，Medium，Large，在不同的设备，不同的用户系统字号设置，不同的控件中，相同的枚举返回的字号数值可能都不一样。通过Device.GetNamedSize方法获取的FontSize值的单位是double，表示文本字符从最下面到最上面到高度，字体的宽度一般都是FontSize值的一半，字体的行距一般是FontSize值的1.2倍。
## Forms中UI布局细节 ##
在Forms中设计各种元素布局等细节依然可以参考设计网页采用的盒模型的思想。从大的块元素的分离到小如一个文字，都可以想象成一个个小盒子。由内容区，内边距，边框，外边距组成。
![](http://images2015.cnblogs.com/blog/282687/201603/282687-20160301183231830-2012572054.jpg)
Forms中还有几个比较容易混淆的类：ContentView，Frame，BoxView。
虽然可以按照盒模型的思想来布局元素，但是Forms中没有标准的margin的概念，Forms的做法是在一个内容视图外面再嵌套一个ContentView，ContentView继承自Layout，只多了一个Content属性来存放内容视图。此时，ContentView的Padding属性就可以想象成盒子的Margin。
Frame在布局中也比较常用，通常用于定义页面中一组视图的区块，它继承自ContentView，多了些边框、阴影等属性。
BoxView是一个矩形填充区，在Forms中用得最多的地方就是用它来绘制横线、竖线等分割线。虽然看起来很山寨，但它却是是Forms中的一个标准用法。
## APP的发布 ##
前面教程重点是介绍Xamarin.Forms相关的东西，对于平台特定的那些没有做介绍，比如平台和Forms之间的交互（依赖注入，前面的Demo介绍PPT有），比如最后APP的发布，发布相关的东西参考前面提到的在线教程：
### Android发布教程 ###
我们项目中的Android安装包没有发布进商城，是通过网址直接下载，所以发布教程没有验证：<https://developer.xamarin.com/guides/android/deployment,_testing,_and_metrics/publishing_an_application/>
### iOS发布教程 ###
iOS需要发布，流程主要是有很多和apple打交道的地方比较麻烦，比如说开发者证书，AppStore证书，用特定的证书打包你的IPA，提交到itunesconnect，审核等等，Xamarin的教程如下：<https://developer.xamarin.com/guides/ios/deployment,_testing,_and_metrics/app_distribution/>
## Xamarin组件商店的使用 ##
Xamarin有自己的组件商店，里面有很多免费和收费的组件，刚开始就在这上面找东西，不过网速实在不可恭维，后来发现免费插件这上面有的GitHub上几乎都有，所以使用GitHub又快又方便。
如果要在组件商店中下载需要注意最后一步需要[翻墙](https://github.com/getlantern/lantern "lantern")，因为网站用了google提供的jquery库：<https://components.xamarin.com/>
GitHub上Xamarin提供的一个常用的免费插件目录，这个插件库里有Xamarin官方的也有第三方的。我们的项目所使用的插件大多来自这个目录，里面有插件的NuGet和GitHub地址： <https://github.com/xamarin/plugins>
GitHub上Xamarin官方插件库的源代码： <https://github.com/jamesmontemagno/Xamarin.Plugins>
## GitHub上去找东西 ##
在GitHub上使用“Xamarin.Forms”为关键词进行搜索，可以快速找到相关资源。

* [**Xamarin-Forms-Labs**](https://github.com/XLabs/Xamarin-Forms-Labs)：这个库很大，包含的东西很多，IOC容器、序列化组件、缓存组件、UI控件等，我们用得最多的还是UI控件。但是用法不是像其他插件一样直接引用它的相关dll（之前尝试过很久，直接使用会导致莫名其妙的问题），而是直接拷贝代码到我们项目中直接用，但是这个库也正如它的名字一样，是实验性的，在GitHub介绍上也可以看到可用控件里几乎所有控件都是beta状态，我们在使用过程中也发现了不少Bug，所以项目里的代码有所改动，跟以前应该不太一样了。我们项目里参考并使用的控件有[Checkbox](https://github.com/XLabs/Xamarin-Forms-Labs/wiki/Checkbox-Control)、[RadioButton](https://github.com/XLabs/Xamarin-Forms-Labs/tree/master/src/Forms/XLabs.Forms/Controls/RadioButton)等。
* [**XamarinFormsGestureRecognizers**](https://github.com/RobGibbens/XamarinFormsGestureRecognizers)：这个没有使用过，从说明来看是一个手势功能相关的库。XamarinForms里的控件默认只有Tap点击事件，其他手势操作都在平台内部，这个库就是教你怎样将它们连接起来，然后在PCL中写针对控件的手势操作代码。

## IDE技巧 ##

* Android很简单，在Windows上启动海马玩模拟器，模拟器启动时间比较长，但启动好之后就可以不用关了，然后只需要用Visual Studio设置Android项目为启动项，附加到模拟器进行调试即可；真机用Usb连接使用同样的方式在IDE里调试。
* iOS比较麻烦，需要打开Mac电脑上的BuildHost（如果Mac不在身边，可使用远程软件[tightvnc](http://www.tightvnc.com/)操作，不过一台Mac同时只能供一人使用），然后Visual Studio设置iOS为启动项，可自动寻找局域网内的Mac电脑上的BuildHost，然后输入配对码即可连接成功，如果失败请重启BuildHost再试；真机调试一样，不过真机只能连接在Mac电脑上。

## 一些常用插件 ##
Forms中插件的使用也比较简单，基本上用一次就会了。首先，插件的使用方式都很统一，Forms的PCL中一般引用两个库,两个库都是PCL的，一个带Abstractions后缀，里面只定义了接口和实体，不包含逻辑代码；另一个不带Abstractions后缀，就像工厂一样，只负责创建Abstractions程序集里定义的那些接口的实现者，创建的方式不是使用前面提到的Xamarin提供的依赖注入（ [UsingDependencyService](https://github.com/xamarin/xamarin-forms-samples/tree/master/UsingDependencyService) ），而是条件编译的方式直接New对应平台的实现者。在Andorid和iOS（或者WP）里引用了带Abstractions后缀的程序集，然后引用一个真正的属于该平台的程序集（非PCL，可以调用平台特殊API），这个程序集实现了Abstractions程序集里的接口，它的实例化对象在运行时被真正使用。如果我们自己写插件就可以使用Xamarin提供的依赖注入的方式，在特定平台内部写好功能类，然后在PCL中直接导出就可以使用了。
然后下面列出一些常用插件：

* [Corcav.Behaviors](https://github.com/corradocavalli/Corcav.Behaviors)：帮助你将列表的每一项绑定命令到这个列表的BindingContext，而不是具体项的BindingContext，帮助将事件转为命令，Xamarin自身不带这个功能。
* [EZ-Compress-for-Xamarin](https://github.com/VictorGrunn/EZ-Compress-for-Xamarin) ：压缩图片流的库。
* [MvvmLight](https://mvvmlight.codeplex.com/)：Mvvm开发模式的支持库，还用到了里面的Ioc容器（SimpleIoc，我们系统里有两套Ioc容器，一个就是这个，另一个是Xamarin的依赖注入容器）；还用到了它提供的导航组件。
* [Xam.Plugins.Messaging](https://github.com/cjlotz/Xamarin.Plugins)：提供打电话、发短信、发Email等功能。
* [Media.Plugin](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/Media)：提供拍照、选照片的功能。
* [PCLStorage](https://github.com/dsplaisted/PCLStorage) ：跨平台的异步I/O库。
* [Vibrate](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/Vibrate)：提供了手机震动的功能。
* [Toasts.Forms.Plugin](https://github.com/EgorBo/Toasts.Forms.Plugin) ：顶部的那个彩色浮动提示框。

然后一些用得比较多的UI组件有：圆形图片、[Checkbox](https://github.com/XLabs/Xamarin-Forms-Labs/wiki/Checkbox-Control)、[RadioButton](https://github.com/XLabs/Xamarin-Forms-Labs/tree/master/src/Forms/XLabs.Forms/Controls/RadioButton)、图片选择器等，有自己写的，也有在[Xamarin-Forms-Labs](https://github.com/XLabs/Xamarin-Forms-Labs)的基础上改的。
Andorid使用插件时注意在工程的Properties的AndroidManifest.xml中写入对应的权限。
## Xamarin官方论坛 ##
遇到疑难的问题，上Xamarin官方论坛搜索，大部分你遇到的问题上面应该都会有，基本用不着主动提问，这个地址我认为访问频率相当高，地址如下：<http://forums.xamarin.com/>
## 没有涉及到的东西 ##
本教程没有涉及到GIS相关的内容。
没有对特定平台内部相关知识介绍，我们团队的成员对平台特定API都了解太少，特别是涉及UI方面的，要掌握这些知识的难度跟学习原生开发无异，所以对一些难题解决起来比较费力，比如之前的Android和iOS的Tab页样式差异问题（Android的Tab在屏幕上面，iOS的Tab在屏幕底部）。因为Tab属于页面，跟控件不一样，不能使用[CustomRenderers](https://developer.xamarin.com/guides/xamarin-forms/custom-renderer/)的技术重写样式，在论坛上搜索的结果如下：
<http://forums.xamarin.com/discussion/54668/bottom-tab-bar-menu-for-android>
<https://forums.xamarin.com/discussion/10004/tabs-on-the-bottom-for-android-example-code>
<http://forums.xamarin.com/discussion/56320/is-there-any-way-to-show-tabs-on-bottom-in-android-using-tabbedpagerenderer>
主要意思先是从设计的角度强调不要进行这样通用的设计，如果一定是通用样式那么给出的解决方案也是平台内部的，首先不说技术门槛，这个实现方式跟Forms的思想就是有冲突的，所以最好的方案就是在新APP里用Forms纯手写Tab页面。
## 资源汇总 ##
> 官方Demo：
<https://developer.xamarin.com/samples-all/>
<https://github.com/xamarin/xamarin-forms-samples>
官方程序安装地址：
<https://static.xamarin.com/installer_assets/v3/Windows/Universal/InstallationManifest.xml>
<https://static.xamarin.com/installer_assets/v3/Mac/Universal/InstallationManifest.xml>
[windows下的ios模拟器](https://developer.xamarin.com/guides/cross-platform/windows/ios-simulator/)
最新的版本信息查询地址：
<https://developer.xamarin.com/releases/current/>
一个博客提供的最新下载地址：
<https://www.coderbusy.com/archives/256.html?from=groupmessage&isappinstalled=0>
可根据以上版本号手动构造下载地址如下：
Win:
http://dl.google.com/android/ndk/android-ndk-r10e-windows-x86_64.exe
http://dl.google.com/android/installer_r24.4.1-windows.exe
http://download.xamarin.com/GTKforWindows/Windows/gtk-sharp-2.12.30.msi
https://dl.xamarin.com/XamarinforVisualStudio/Windows/Xamarin.VisualStudio_4.2.1.58.msi
http://download.xamarin.com/studio/Windows/XamarinStudio-5.10.3.26-0.msi
Mac:
http://download.xamarin.com/Installer/MonoForAndroid/jdk-7u71-macosx-x64.dmg
http://dl.google.com/android/android-sdk_r24.4.1-macosx.zip
http://dl.google.com/android/ndk/android-ndk-r10e-darwin-x86_64.bin
http://dl.xamarin.com/MonoFrameworkMDK/Macx86/MonoFramework-MDK-4.6.2.7.macos10.xamarin.universal.pkg
https://dl.xamarin.com/MonoDevelop/Mac/XamarinStudio-6.1.2.44.dmg
https://dl.xamarin.com/MonoTouch/Mac/xamarin.ios-10.2.1.5.pkg
http://dl.xamarin.com/MonoforAndroid/Mac/xamarin.android-7.0.2-37.pkg
http://dl.xamarin.com/XamarinforMac/Mac/xamarin.mac-2.10.0.113.pkg
官方文档：
<https://developer.xamarin.com/guides/xamarin-forms/creating-mobile-apps-xamarin-forms/>
官方教材：
<https://developer.xamarin.com/guides/xamarin-forms/creating-mobile-apps-xamarin-forms/>
<https://github.com/xamarin/xamarin-forms-book-samples/>
官方论坛：
<http://forums.xamarin.com/>
常用插件：
<https://github.com/xamarin/plugins>
<https://github.com/jamesmontemagno/Xamarin.Plugins>
<https://components.xamarin.com/>
<https://github.com/XLabs/Xamarin-Forms-Labs>
MvvmCross
<https://github.com/MvvmCross/MvvmCross>
<https://github.com/MvvmCross/MvvmCross-Plugins>










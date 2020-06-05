# Xamarin.Forms——WebView技术研究

标签（空格分隔）： Xamarin 翻译

---

在Xamarin中有一些Forms原生不太好实现的内容可以考虑使用HTML、Javascript、CSS那一套前端技术来实现，使用[WebView](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/)来承载显示本地或网络上的HTML文件。不像`OpenUri`方法，不是将用户带到设备中的本地浏览器里去，而是用`WebView`直接在App内部显示HTML内容。

该文档由下面几节内容组成：

* [内容（Content）](#content)——WebView支持各种内容元素，包括嵌入式HTML文件，Web页面和HTML字符串。
* [导航（Navigation）](#navigation)——WebView支持导航到特定的页面并返回。
* [事件（Events）](#events)——监听并相应响应用户在WebView中触发的动作。
* [性能（Performance）](#performance)——了解各平台上WebView的性能特点。
* [权限（Permissions）](#permissions)——了解在App中使用WebView的相应权限设置。
* [布局（Layout）](#layout)——WebView对布局有一些特别要求，了解如何恰当的显示WebView。

![img](https://images2015.cnblogs.com/blog/282687/201603/282687-20160311224937835-1397055104.png)

## Content

WebView支持下列几种内容：

* HTML和CSS网站——WebView全方位支持HTML和CSS布局的网站，也包括对JavaScript的支持。
* 文档（Documents）——因为WebView使用的是平台本地组件，所以WebView可以显示平台所支持的那些文档类型。这意味着，PDF文件可以直接在iOS和Android平台上显示，但是Windows Phone平台不能。
* HTML字符串——WebView可以显示内存中的HTML字符串。
* 本地文件——WebView可以显示嵌入在App中的任何内容。

**注意：** Windows和Windows Phone平台上的`WebView`并不支持Silverlight，Flash和任何的ActiveX控件，即使该平台上的Internet Explorer浏览器都支持。

### 网站（Websites）

要显示因特网上的一个网站，可以给`WebView`的[`Source`](http://developer.xamarin.com/api/type/Xamarin.Forms.WebViewSource/)属性设置一个URL字符串：

``` csharp
var browser = new WebView {
    Source = "http://xamarin.com"
};
```

**注意：** URLs地址必须是指定协议的完整格式（如：前面必须有"`http://`"或"`http://`"）

#### iOS和ATS

自iOS 9以后，默认情况下仅允许应用程序和实现了最佳安全实践的服务通信，如果想和不安全的服务进行通信必须要修改`Info.plist`的设置。

>**注意：** 如果你的应用程序需要链接一个不安全的服务，你应该使用`NSExceptionDomains`设置一些域名做为例外，而不是使用`NSAllowsArbitraryLoads`完全关闭[ATS](https://developer.xamarin.com/guides/ios/platform_features/introduction_to_ios9/ats/)。`NSAllowsArbitraryLoads`只应该用在某些极端紧急情况下。

下面这个例子展示了如何设置一些特殊域名（此处用xamarim.com举例）绕开[ATS](https://developer.xamarin.com/guides/ios/platform_features/introduction_to_ios9/ats/)的检验：

``` xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSExceptionDomains</key>
        <dict>
            <key>xamarin.com</key>
            <dict>
                <key>NSIncludesSubdomains</key>
                <true/>
                <key>NSTemporaryExceptionAllowsInsecureHTTPLoads</key>
                <true/>
                <key>NSTemporaryExceptionMinimumTLSVersion</key>
                <string>TLSv1.1</string>
            </dict>
        </dict>
    </dict>
```

良好的实践是只让一部分域名绕开[ATS](https://developer.xamarin.com/guides/ios/platform_features/introduction_to_ios9/ats/)的检验，允许你访问受信任的网站同时也能更安全的访问不受信任的域名。下面的例子展示了App当中最不安全的一种做法，完全禁用[ATS](https://developer.xamarin.com/guides/ios/platform_features/introduction_to_ios9/ats/)：

``` xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSAllowsArbitraryLoads </key>
        <true/>
    </dict>
```

查看[App Transport Security](https://developer.xamarin.com/guides/ios/platform_features/introduction_to_ios9/ats/)获取关于iOS 9这个新功能的更多信息。

### HTML字符串

如果你想展示一串被代码动态定义的HTML字符串，你需要创建一个[`HtmlWebViewSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.HtmlWebViewSource/)的实例：

``` csharp
var browser = new WebView();
var htmlSource = new HtmlWebViewSource();
htmlSource.Html = @"<html><body>
  <h1>Xamarin.Forms</h1>
  <p>Welcome to WebView.</p>
  </body></html>";
browser.Source = htmlSource;
```

![img](https://images2015.cnblogs.com/blog/282687/201603/282687-20160311225016038-517152499.png)

上面的代码中，`@`符号用于标记HTML作为一串字面量的字符串，所有通用转义符都会被忽略。

### 本地HTML文件

WebView可以直接展示嵌入在App中的HTML，CSS和Javascript，例如：

``` html
<html>
  <head>
    <title>Xamarin Forms</title>
  </head>
  <body>
    <h1>Xamrin.Forms</h1>
    <p>This is an iOS web page.</p>
    <img src="XamarinLogo.png" />
  </body>
</html>
```

CSS：

``` css
html,body {
  margin:0;
  padding:10;
}
body,p,h1 {
  font-family: Chalkduster;
}
```

注意上面的CSS中指定的特殊字体需要根据不同的平台自定义，不是所有的平台都具有相同的字体。

![img](https://images2015.cnblogs.com/blog/282687/201603/282687-20160311231030132-1051618272.png)

>**注意：** Xamarin.Forms中当前存在一个bug，在Windows中本地HTML内容不能被显示，但是它不会影响Windows Phone。查看[Bugzilla](https://bugzilla.xamarin.com/show_bug.cgi?id=32033)获取关于这个bug最新状态的更多信息。

使用`WebView`显示本地内容，你需要像打开其他文件一样打开一个HTML文件，然后读取内容文本到`HtmlWebViewSource`的`Html`属性上。关于打开文件的更多信息，可以查看[Working with Files](https://developer.xamarin.com/guides/xamarin-forms/working-with/files/)。

虽然我们读取了第一个页面，但是`WebView`并不知道HTML来自于哪里。这样在处理页面引用本地资源时就会有问题。例如，一个本地文件很可能有一个指向另一个文件的链接，也有可能使用一个分离的JavaScript文件，或者一个CSS样式文件。

为了解决这个问题，你需要告诉`WebView`到文件系统的哪里去寻找文件。此时需要设置`WebView`所引用的`HtmlWebViewSource`对象的`BaseUrl`属性。

因为文件系统在每个操作系统上是不一样的，所以你需要针对每个平台确定它的URL。Xamarin.Forms可以通过`DependencyService`解决运行时的平台依赖。

要使用`DependencyService`，需要先定义一个可被各平台所实现的接口：

``` csharp
public interface IBaseUrl { string Get(); }
```

注意，该接口需要被各个平台实现，否则App会无法运行。在公共项目中，要确保你记得使用`DependencyService`设置`BaseUrl`的值：

``` csharp
var source = new HtmlWebViewSource();
source.BaseUrl = DependencyService.Get<IBaseUrl>().Get();
```

至此，你只需要在各个平台中实现这个接口就可以了：

#### iOS

```csharp
[assembly: Dependency(typeof(BaseUrl_iOS))]
namespace WorkingWithWebview.iOS
{
    public class BaseUrl_iOS : IBaseUrl
    {
        public string Get()
        {
            return NSBundle.MainBundle.BundlePath;
        }
    }
}
```

此时网站的位置会被定位到项目的根目录或者**Resources**目录，并且文件的生成操作应该修改为*BundleResource*，如下图：

![img](https://images2015.cnblogs.com/blog/282687/201603/282687-20160311235821616-1732031922.png)

#### Android

在Android平台下，HTML，CSS和图片文件应该位于**Assets**文件夹下，并且生成操作应该修改为*AndroidAsset*，如下图：

![img](https://images2015.cnblogs.com/blog/282687/201603/282687-20160312000116397-682166478.png)

Android平台下，`BaseUrl`的值应该设置为`"file:///android_asset/"`：

``` csharp
[assembly: Dependency(typeof(BaseUrl_Android))]
namespace WorkingWithWebview.Android
{
    public class BaseUrl_Android : IBaseUrl
    {
        public string Get()
        {
            return "file:///android_asset/";
        }
    }
}
```

Android平台下，**Assets**文件夹下的文件也可以通过`Forms.Context.Assets`属性访问，代码如下：

``` csharp
var assetManager = Xamarin.Forms.Forms.Context.Assets;
using (var streamReader = new StreamReader(assetManager.Open("local.html")))
{
    var html = streamReader.ReadToEnd();
}
```

#### Windows Phone

>**注意：** 在Windows Phone 8.1下`WebView`不支持本地内容，但是在Windows Phone 8下支持。

在Windows Phone平台下，HTML，CSS和图片文件位于项目根目录，并且生成操作应该修改为*Content*，如下图：

![img](https://images2015.cnblogs.com/blog/282687/201603/282687-20160312000754944-897950844.png)

Windows Phone平台下，`BaseUrl`的值应该设置为`""`：

``` csharp
[assembly: Dependency(typeof(BaseUrl_Windows))]
namespace WorkingWithWebview.Windows
{
    public class BaseUrl_Windows : IBaseUrl
    {
        public string Get()
        {
            return "";
        }
    }
}
```

## Navigation

WebView可以通过几个方法和属性来进行导航：

* **GoForward()**——如果`CanGoForward`等于true，调用`GoForward`导航到下一个访问页面。
* **GoBack()**——如果`CanGoBack`等于true，调用`GoBack`将会导航到最后一个访问过的页面。
* **CanGoBack**——如果有页面可以返回则该值等于`true`，如果浏览的是起始`URL`则该值返回`false`。
* **CanGoForward**——如果用户返回了某个页面并且可以向前访问某个已经访问过的页面则该值为`true`。

在页面内部，`WebView`不支持多点触控。所以重点是需要针对移动端优化内容，使其呈现时不需要额外的缩放等操作。

对于应用程序来说，在`WebView`中展示链接比在设备的浏览器中展示更常见。在这种情况下，允许正常的导航非常有用，但是当用户在起始页面点击返回按钮时，程序会返回到一个正常的App页面中。

下面演示如何使用内建的方法和属性启用这种场景。

首先在一个page页面中创建一个浏览器视图：

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="WebViewDemo.InAppDemo"
Title="In App Browser">
    <ContentPage.Content>
        <StackLayout>
            <StackLayout Orientation="Horizontal" Padding="10,10">
                <Button Text="Back" HorizontalOptions="StartAndExpand" Clicked="backClicked" />
                <Button Text="Forward" HorizontalOptions="End" Clicked="forwardClicked" />
            </StackLayout>
            <WebView x:Name="Browser" WidthRequest="1000" HeightRequest="1000" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

后台代码：

``` csharp
public partial class InAppDemo : ContentPage
{
    //sets the URL for the browser in the page at creation
    public InAppDemo(string URL)
    {
        InitializeComponent();
        Browser.Source = URL;
    }

    private void backClicked(object sender, EventArgs e)
    {
        //check to see if there is anywhere to go back to
        if (Browser.CanGoBack)
        {
            Browser.GoBack();
        }
        else { //if not, leave the view
            Navigation.PopAsync();
        }
    }

    private void forwardClicked(object sender, EventArgs e)
    {
        if (Browser.CanGoForward)
        {
            Browser.GoForward();
        }
    }
}
```

最后呈现效果如下：

![img](https://images2015.cnblogs.com/blog/282687/201603/282687-20160312225451741-1318967697.png)

## Events

WebView通过触发两个事件来帮助你响应状态变更：

* **Navigating**——当WebView开始加载一个新页面时触发。
* **Navigated**——当一个页面加载完毕并且导航停止时触发。

如果你能预感到加载网页将会耗费比较长的时间，可以考虑使用这两个事件实现一个状态指示器。代码如下：

界面XAML代码：

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="WebViewDemo.LoadingDemo" Title="Loading Demo">
    <ContentPage.Content>
    <StackLayout>
      <Label x:Name="LoadingLabel"
        Text="Loading..."
        HorizontalOptions="Center"
        isVisible="false" />
      <WebView x:Name="Browser"
      HeightRequest="1000"
      WidthRequest="1000"
      Navigating="webOnNavigating"
      Navigated="webOnEndNavigating" />
    </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

后台两个事件处理：

``` csharp
void webOnNavigating (object sender, WebNavigatingEventArgs e)
{
    LoadingLabel.IsVisible = true;
}

void webOnEndNavigating (object sender, WebNavigatedEventArgs e)
{
    LoadingLabel.IsVisible = false;
}
```

当页面加载中时输出结果如下：

![img](https://images2015.cnblogs.com/blog/282687/201603/282687-20160312232058866-1544884984.png)

当页面加载完成时输出如下：

![img](https://images2015.cnblogs.com/blog/282687/201603/282687-20160312232156100-1198256173.png)

## Performance

最近的各大web浏览器都已经采用了一些被世人所熟知的流行技术，如硬件加速渲染和JavaScript编译。不幸的是，出于安全限制，大多数高级功能在iOS的`WebView`和`UIWebView`里无法使用。Xamarin.Forms里的`WebView`在iOS平台下实际上是使用的`UIWebView`。如果这个限制对你的使用造成了困扰，你需要在平台内部使用支持快速浏览等功能的`WKWebView`进行重新渲染。但是需要注意`WKWebView`仅支持iOS 8以上的系统。

WebView在Android的默认设置中跟内建浏览器一样快。

在Windows Phone 8和Windows Phone 8.1里的`WebBrowser`控件不支持最新的HTML5相关新特性，并且它常常出现性能问题。需要注意测试网站在Windows Phone的`WebView`中究竟会怎样呈现，如果仅在IE浏览器中测试是不够的。

## Permissions

为了让`WebView`正常工作，你必须知道各个平台的权限设置。注意在一些平台，`WebView`在debug模式下可以工作，但是在最后发布时的release模式下却不能。这就是权限造成的问题，比如Android上的internet访问权限，在Xamarin Studio的调试模式下默认是打开的。

* **Windows Phone 8.0**——需要`ID_CAP_WEBBROWSERCOMPONENT`权限来使用控件，需要`ID_CAP_NETWORKING`来访问因特网。
* **Windows Phone 8.1**——需要`Internet`才能访问网络内容。
* **Android**——需要`Internet`才能访问网络内容，访问本地内容不需要特殊的权限。
* **iOS**——不需要特殊的权限。

## Layout

不像大多数其他Xamarin.Forms视图，在`StackLayout`和`RelativeLayout`中嵌套`WebView`时必须指明它的`HeightRequest`和`WidthRequest`属性。如果你没有指明这些属性，那么`WebView`将不会渲染。

下面的例子展示了`WebView`的渲染布局代码：

在`StackLayout`中指明`HeightRequest`和`WidthRequest`：

``` xml
<StackLayout>
    <Label Text="test" />
    <WebView Source="http://www.xamarin.com/"
        HeightRequest="1000"
        WidthRequest="1000" />
</StackLayout>
```

在`RelativeLayout`中指明`HeightRequest`和`WidthRequest`：

``` xml
<RelativeLayout>
    <Label Text="test"
        RelativeLayout.XConstraint= "{ConstraintExpression
                                      Type=Constant, Constant=10}"
        RelativeLayout.YConstraint= "{ConstraintExpression
                                      Type=Constant, Constant=20}" />
    <WebView Source="http://www.xamarin.com/"
        RelativeLayout.XConstraint="{ConstraintExpression Type=Constant,
                                     Constant=10}"
        RelativeLayout.YConstraint="{ConstraintExpression Type=Constant,
                                     Constant=50}"
        WidthRequest="1000" HeightRequest="1000" />
</RelativeLayout>
```

在`AbsoluteLayout`中不用指明`HeightRequest`和`WidthRequest`：

``` xml
<AbsoluteLayout>
    <Label Text="test" AbsoluteLayout.LayoutBounds="0,0,100,100" />
    <WebView Source="http://www.xamarin.com/"
      AbsoluteLayout.LayoutBounds="0,150,500,500" />
</AbsoluteLayout>
```

在`Grid`中不用指明`HeightRequest`和`WidthRequest`，`Grid`是少数的几个不用指明宽和高的布局容器：

``` xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="100" />
        <RowDefinition Height="*" />
    </Grid.RowDefinitions>
    <Label Text="test" Grid.Row="0" />
    <WebView Source="http://www.xamarin.com/" Grid.Row="1" />
</Grid>
```

> 原文链接：
<https://developer.xamarin.com/guides/xamarin-forms/user-interface/webview/>
相关Demo：
[Working with WebView](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/)：WebView的简单演示
[WebView](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView/)：WebView的简单演示
[HybridWebView](https://github.com/xamarin/xamarin-forms-samples/tree/master/CustomRenderers/HybridWebView)：WebView的高级演示，包括平台自定义渲染WebView和JavaScript和C#代码交互。

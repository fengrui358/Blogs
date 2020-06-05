# WPF 技术点

标签（空格分隔）： WPF

---

## 常用 Path 路径

- **正三角形(左)：**&lt;Path Data="M40,0 L0,30 40,60 z" Stretch="Uniform"/&gt;
- **正三角形(上)：**&lt;Path Data="M0,40 L30,0 60,40 z" Stretch="Uniform"/&gt;
- **正三角形(右)：**&lt;Path Data="M0,0 L40,30 0,60 z" Stretch="Uniform"/&gt;
- **正三角形(下)：**&lt;Path Data="M0,0 L30,40 60,0 z" Stretch="Uniform"/&gt;
- **矩形：**&lt;Path Data="M0,0 L1,0 L1,1 L0,1 z" Stretch="Uniform"/&gt;
- **圆形：**&lt;Path Data="M100,50 C100,77.614237 77.614237,100 50,100 C22.385763,100 0,77.614237 0,50 C0,22.385763 22.385763,0 50,0 C77.614237,0 100,22.385763 100,50 z" Stretch="Uniform"/&gt;
- **箭头(左)：**&lt;Path Data="M40,0 L0,30 40,60" Stretch="Uniform"/&gt;
- **箭头(上)：**&lt;Path Data="M0,40 L30,0 60,40" Stretch="Uniform"/&gt;
- **箭头(右)：**&lt;Path Data="M0,0 L40,30 0,60" Stretch="Uniform"/&gt;
- **箭头(下)：**&lt;Path Data="M0,0 L30,40 60,0" Stretch="Uniform"/&gt;
- **半圆(左)：**&lt;Path Data="M34,0 L34,68 C15.222317,68 0,52.777684 0,34 0,15.222319 15.222317,3.5762787E-07 34,0 z" Stretch="Uniform"/&gt;
- **半圆(上)：**&lt;Path Data="M34,0 C52.777684,7.1525574E-07 68,15.222319 68,34.000001 L0,34.000001 C3.5762787E-07,15.222319 15.222319,7.1525574E-07 34,0 z" Stretch="Uniform"/&gt;
- **半圆(右)：**&lt;Path Data="M0,0 C18.777683,3.5762787E-07 34,15.222319 34,34 34,52.777684 18.777683,68 0,68 z" Stretch="Uniform"/&gt;
- **半圆(下)：**&lt;Path Data="M0,0 L68,0 C68,18.777681 52.777684,34 34,34 15.222319,34 3.5762787E-07,18.777681 0,0 z" Stretch="Uniform"/&gt;
- **椭圆矩形(水平)：**&lt;Path Data="M34,0 L76,0 C94.777683,9.8347664E-07 110,15.222319 110,34.000001 110,52.777684 94.777683,68.000001 76,68.000001 L34,68.000001 C15.222317,68.000001 0,52.777684 0,34.000001 0,15.222319 15.222317,9.8347664E-07 34,0 z" Stretch="Uniform"/&gt;
- **椭圆矩形(垂直)：**&lt;Path Data="M34.000001,0 C52.777684,0 68.000001,15.222321 68.000001,34 L68.000001,76 C68.000001,94.777681 52.777684,110 34.000001,110 15.222319,110 8.9406967E-07,94.777681 0,76 L0,34 C8.9406967E-07,15.222321 15.222319,0 34.000001,0 z" Stretch="Uniform"/&gt;
- **菱形:**&lt;Path Data="M150,0 L0,75 0,225 150,300 300,225 300,75 z" Stretch="Uniform"/&gt;

`Stretch`默认使用`Uniform`，在这种参数下矩形为正方形、圆形为正圆形，如果修改`Stretch`为`Fill`，则根据容器高宽进行自由拉伸。

## 动画

- 缓动函数（使用 IE 查看）：<http://tinyurl.com/animationeasing>
- WPF 动画默认为 60 帧/s，程序会在系统资源允许的情况下努力使动画达到这个帧率，也可以在代码里手动指定帧率，修该附加属性`Timeline.DesiredFrameRate`的值。

## 变化

- `LayoutTransform`：对元素使用该变换时会真正改变元素的大小和位置，从而引发布局容器的重新计算，因此，只当使用该变化一次，不要在动画中使用。
- `RenderTransform`：显示变化，对元素运用该变化时并不会真正的改变元素的大小和位置，只是显示上面产生了变化，因此不会影响布局容器的测量和排版。
- `RenderTransformOrigin`：使用变化时可以在具体变化中使用决对坐标，也可以直接指定元素的`RenderTransformOrigin`，从而使用相对坐标，比如"0.5,0.5"就是该元素的中心点。

## 字体

- WPF 有个问题是小文本显示不太清晰，原因是 WPF 没有使用 GDI 渲染文本，好的解决方法是增大文本，在通常的`96dpi`的显示器上，文本字号最小控制在**15**，如果小于这个值最好将文本的`TextOptions.TextFormattingMode`的值设置为`Display`，而不是标准的`Ideal`，这样做会使文本更清晰。
- 检查字体版权是否允许在自己的程序中嵌入，可使用工具：<https://www.microsoft.com/en-us/Typography/TrueTypeProperty21.aspx>，如果右键属性显示可安装就可以使用。
- 字体加载的两种方式，第一种字体为嵌入式资源：`FontFamily="pack://application:,,,/WpfLabs;component/FontFamilyDemo/#Aileron"`；第二种字体为文件，可在后台代码中设置：`new FontFamily(@"D:\FontFamilyDemo\FontFiles\#Aileron");`。

## 性能

- WPF 中，很多元素继承了`Freezable`，表示该元素支持冻结，冻结之后变更通知就失效了，但是内存占用会下降，很多情况下都可以将元素冻结，常见的比如`SolidColorBrush`和`Storyboard`。在`Xaml`中使用时先引用命名空间`xmlns:options="http://schemas.microsoft.com/winfx/2006/xaml/presentation/options"`，然后对元素使用`<Storyboard options:Freeze="True">`。
- 集合虚拟化：启用 UI 虚拟化的两个附加属性 1、`ScrollViewer.CanContentScroll="True"`；2、`VirtualizingStackPanel.IsVirtualizing="True"`。
- 大集合项容器再循环，项容器再循环提高了滚动性能，降低了内存消耗量：`VirtualizingStackPanel.VirtualizationMode="Recycling"`
- 当集合中使用了分组，默认不会启用虚拟化，需要手动设置`VirtualizingStackPanel.IsVirtualizingWhenGrouping`属性纠正这个问题

```csharp
<ListBox VirtualizingStackPanel.IsVirtualizingWhenGrouping="True"...>
```

## 绑定

`Xaml`中`String.Format`的使用参考：<https://www.cnblogs.com/candyzkn/p/4476832.html>
`Uri`绑定资源路径写法：`pack://application:,,,/WpfLabs;component/FontFamilyDemo/#Aileron`
`Uri`绑定本地文件路径写法：`pack://siteoforigin:,,,/Aileron.jpg`

## 错误验证

显示错误提示可使用专用的错误模板，即在真正的元素之上附加一层装饰层来展示真正的错误，注意在错误模板中使用`AdornedElementPlaceholder`来标识真正的控件元素，然后使用路径属性绑定去寻找真正的错误提示，参考《宝典》 19.4.5。

## 常用代码片段

### WPF MahApps.Metro

```csharp
    <Application.Resources>
         <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <!--MahApps.Metro风格-->
                <ResourceDictionary Source="pack://application:,,,/MahApps.Metro;component/Styles/Controls.xaml" />
                <ResourceDictionary Source="pack://application:,,,/MahApps.Metro;component/Styles/Fonts.xaml" />
                <ResourceDictionary Source="pack://application:,,,/MahApps.Metro;component/Styles/Colors.xaml" />
                <ResourceDictionary Source="pack://application:,,,/MahApps.Metro;component/Styles/Accents/Blue.xaml" />
                <ResourceDictionary Source="pack://application:,,,/MahApps.Metro;component/Styles/Accents/BaseLight.xaml" />
            </ResourceDictionary.MergedDictionaries>
         </ResourceDictionary>
    </Application.Resources>
```

```csharp
    默认颜色：AccentColorBrush
```

```csharp
    高亮按钮：<Style TargetType="Button" BasedOn="{StaticResource AccentedSquareButtonStyle}" />
```

```csharp
    带按钮的TextBox：<Style TargetType="TextBox" BasedOn="{StaticResource MetroButtonTextBox}" />
    <TextBox Grid.Column="1" mah:TextBoxHelper.ButtonContent="p" mah:TextBoxHelper.ButtonCommand="{Binding FileSelectedCommand}"
    mah:TextBoxHelper.Watermark="选择文件" IsReadOnly="True" Text="{Binding FilePath, Mode=OneWay}"/>
```

### WPF OpenFileDialog

```csharp
    var dialog = new OpenFileDialog
    {
        Multiselect = false
    };

    var result = dialog.ShowDialog(this);
    if (result == true)
    {
        if (dialog.FileNames.Any())
        {
            FilePath = dialog.FileNames.First();
        }
    }
```

### WPF Animation

```csharp
    <DoubleAnimation Storyboard.TargetProperty="(UIElement.RenderTransform).(TransformGroup.Children)[0].(ScaleTransform.ScaleX)"
                                                BeginTime="0" Duration="{StaticResource Duration.Tiny}" From="1"
                                                To="1.2"
                                                EasingFunction="{StaticResource SineEaseOut}" />
```

### ToolTip IsEnabled

```csharp
    ToolTipService.IsEnabled="{Binding RelativeSource={RelativeSource Self}, Path=ToolTip, Converter={x:Static converters:StaticConverterHelper.SuperConverter}}"
```

### 帧动画

```csharp
    <Storyboard>
        <DoubleAnimation Storyboard.TargetProperty="Opacity" Storyboard.TargetName="TopMenuArea"
        From="1.0" To="0" Duration="0:0:1" />
        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="TopMenuArea"
        Storyboard.TargetProperty="Visibility">
            <DiscreteObjectKeyFrame KeyTime="0:0:2" Value="{x:Static Visibility.Collapsed}" />
        </ObjectAnimationUsingKeyFrames>
    </Storyboard>
```

### 颜色控制动画

```csharp
    <ColorAnimation To="Green" Storyboard.TargetProperty="(Button.Background).(SolidColorBrush.Color)" FillBehavior="Stop" Duration="0:0:1"/>
```

### WPF MarkupExtension

```csharp
    public class RevealBorderBrushExtension : MarkupExtension
    {
        public override object ProvideValue(IServiceProvider serviceProvider)
        {
            // 如果没有服务，则直接返回。
            if (!(serviceProvider.GetService(typeof(IProvideValueTarget)) is IProvideValueTarget service)) return null;
            // MarkupExtension 在样式模板中，返回 this 以延迟提供值。
            if (service.TargetObject.GetType().Name.EndsWith("SharedDp")) return this;
            // 如果不是 FrameworkElement，那么返回 this 以延迟提供值。
            if (!(service.TargetObject is FrameworkElement element)) return this;
            // 如果是设计时，那么返回白色
            if (DesignerProperties.GetIsInDesignMode(element)) return Brushes.White;

            var window = Window.GetWindow(element);
            if (window == null) return this;
            // 这一句是编译不通过的，我只是拿来做示范。
            var brush = CreateBrush(window, element);
            return brush;
        }
    }
```

> 文章中提到的参考来源默认为**《WPF 编程宝典：使用 C# 2012 和.NET 4.5 第 4 版》**

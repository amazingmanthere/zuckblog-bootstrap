---
layout: post
---

# Windows Phone自定义控件

本文以自定义一个菜单按钮(上面是图标，下面是文本)为例子，来描述如何在WP平台自定义控件。

## 定义控件  

##### 新建一个类MenuBarCustomButton，并继承ContentControl类：

	namespace Test.MyControls
	{
		public class MenuBarCustomButton : ContentControl
    	{
			public MenuBarCustomButton()
			{
			}
		}
	}

##### 在类MenuBarCustomButton的定义上方声明控件的信息，并在内部定义信息变量名称：

	[TemplatePart(Name = BtnImageControl, Type = typeof(Image))]
	[TemplatePart(Name = BtnTextControl, Type = typeof(TextBlock))]
	[TemplateVisualState(Name = NormalState, GroupName = ControlStates)]
	[TemplateVisualState(Name = PressedState, GroupName = ControlStates)]
	public class MenuBarCustomButton : ContentControl
    {
        private const string BtnImageControl = "BtnImage";
        private const string BtnTextControl = "BtnText";

        private const string ControlStates = "ControlStates";
        private const string NormalState = "Normal";
        private const string PressedState = "Pressed";
        private const string DisabledState = "Disabled";

        public MenuBarCustomButton()
		{
		}
    }

TemplateVisualState声明控件的状态信息，上面声明了控件有正常状态、点击状态和不可用状态；   
TemplatePart声明该控件由哪几个控件组成，上面声明了控件有Image和TextBlock控件；  
定义了变量名称会在后面逻辑处理中用到。  

##### 定义控件的属性，如控件的图片源、文本等：  

    public ImageSource ImageSource
    {
        get { return (ImageSource)GetValue(ImageSourceProperty); }
        set { SetValue(ImageSourceProperty, value); }
    }

    public static readonly DependencyProperty ImageSourceProperty =
        DependencyProperty.Register("ImageSource", typeof(ImageSource), typeof(MenuBarCustomButton), null);

	public string Text
    {
        get { return (string)GetValue(TextProperty); }
        set { SetValue(TextProperty, value); }
    }

    public static readonly DependencyProperty TextProperty =
        DependencyProperty.Register("Text", typeof(string), typeof(MenuBarCustomButton), null);

## 定义样式

##### 在全局样式文件中定义该控件的样式：  

在头部定义处引用控件的命名空间  

	xmlns:MyControls="clr-namespace:Test.MyControls"

在内容定义处定义控件的样式 

	<Style x:Key="MenuBarCustomButtonDefaultStyle" TargetType="MyUIControls:MenuBarCustomButton">
	        <Setter Property="Template">
	            <Setter.Value>
	                <ControlTemplate TargetType="MyControls:MenuBarCustomButton">
	                    <Border x:Name="BtnBorder" Margin="{TemplateBinding Margin}">
	                        <VisualStateManager.VisualStateGroups>
	                            <VisualStateGroup x:Name="ControlStates">
	                                <VisualState x:Name="Normal"/>
	                                <VisualState x:Name="Pressed">
	                                    <Storyboard>
	                                        <DoubleAnimationUsingKeyFrames
	                                            Storyboard.TargetName="BtnGrid"
	                                            Storyboard.TargetProperty="(UIElement.Opacity)">
	                                            <LinearDoubleKeyFrame Value="0.5" KeyTime="0:0:0" />
	                                        </DoubleAnimationUsingKeyFrames>
	                                    </Storyboard>
	                                </VisualState>
	                            </VisualStateGroup>
	                        </VisualStateManager.VisualStateGroups>
	                        <Grid x:Name="BtnGrid" Background="{TemplateBinding Background}">
	                            <Image x:Name="BtnImage" 
	                                            Source="{TemplateBinding ImageSource}" 
	                                            HorizontalAlignment="Center"
	                                            VerticalAlignment="Top">
	                            </ContentControl>

	                            <TextBlock x:Name="BtnText"
	                                         HorizontalAlignment="Center"
	                                         VerticalAlignment="Bottom"
	                                         Foreground="{TemplateBinding Foreground}"
	                                         Text="{TemplateBinding Text}"/>
	                        </Grid>
	                    </Border>
	                </ControlTemplate>
	            </Setter.Value>
	        </Setter>
	    </Style>

该样式定义了控件的各个状态，其中点击状态时控件透明度变为0.5；  
该样式定义了控件的组成，上面是一个图片控件，下面是一个文本控件。

## 处理逻辑

##### 设置控件的样式:
	
	public MenuBarCustomButton()
    {
        this.Style = Application.Current.Resources["MenuBarCustomButtonDefaultStyle"] as Style;
    }

##### 处理状态切换的逻辑:  

注册`ManipulationStarted`和`ManipulationCompleted`事件，用于切换点击状态:

	public MenuBarCustomButton()
	{
		this.Style = Application.Current.Resources["MenuBarCustomButtonDefaultStyle"] as Style;

		this.ManipulationStarted+= MenuBarCustomButton_ManipulationStarted;
		this.ManipulationCompleted+= MenuBarCustomButton_ManipulationCompleted;
	}

	void MenuBarCustomButton_ManipulationCompleted(object sender, ManipulationCompletedEventArgs e)
    {	// 切换至正常状态
        VisualStateManager.GoToState(this, NormalState, true);
    }

    void MenuBarCustomButton_ManipulationStarted(object sender, ManipulationStartedEventArgs e)
    {	// 切换至点击状态
        VisualStateManager.GoToState(this, PressedState, true);
    }

## 使用控件

##### 在xaml文件中使用自定义控件：  

1. 引入控件的命名空间  

		xmlns:MyControls="clr-namespace:Test.MyControls"  

2. 定义控件

		<MyControls:MenuBarCustomButton x:Name="MyButton" ImageSource="/Resources/1.png" Text="自定义按钮"/>

##### 在cs文件中直接使用：

	MenuBarCustomButton MyButton = new MenuBarCustomButton()
	{
		ImageSource = new BitmapImage(new Uri("/Resources/1.png", UriKind.RelativeOrAbsolute));
		Text = "自定义按钮"
	}
#[学习笔记]Android开发艺术探索：Android的Drawable

## 6.1 Drawable简介

1. Drawable表示的是一种可以在Canvas上进行绘制的抽象概念，它的种类有很多，最常见的就是颜色和图片。优点：使用简单，比自定义View成本低很多，非图片类型的Drawable占用空间较小。
    全面理解Drawable的使用细节还是很有必要的，这也是本章的**出发点**。
2. Drawable有很多种，都表示图像的概念，但不全是图片。Drawable是所有Drawable对象的基类。
3. Drawable内部宽/高可以通过getIntrinsicWidth/getIntrinsicHeight这两个方法获取。但并不是所有Drawable都有宽高；图片Drawable的内部宽/高就是图片的宽/高，但是颜色形成的Drawable并没有宽/高的概念。

## 6.2 Drawable的分类

### 6.2.1 BitmapDrawable

```xml
<?xml version="1.0" encoding="utf-8"?>
<bitmap  / nine-pathch
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:antialias="true"
    android:dither="true"
    android:filter="true"
    android:gravity="center"
    android:mipMap="false"
    android:src="@mipmap/haimei1"
    android:tileMode="clamp"></bitmap>
```

**android:src** 资源id
 **android:antialias** 是否开启图片抗锯齿功能，开启后图片会变得平滑，也会轻微降低图片清晰度，建议开启。
 **android:dither** 是否开启抖动效果，当图片的像素配置与手机屏幕像素配置不一致时，开启这个选项可以让高质量图片在低质量屏幕上还能保持较好的显示效果，建议开启。
 **android:filter** 是否开启过滤效果，当图片被拉伸或压缩时，开启过滤效果可以保持较好的显示效果，建议开启。
 **android:gravity** 当前图片小于容器的尺寸时，设置此选项可以对图片进行定位。
 **android:titleMode** 平铺模式，有这几个选项[disabled | clamp | repeat | mirror]， 开启平铺模式后gravity属性会失效，disable表示关闭平铺模式（默认值），repeat表示水平和竖直方向上的平铺效果；
 mirror表示水平和竖直上的镜面投影效果；而clamp表示图片四周的像素扩散到周围区域。

### 6.2.2 ShapeDrawable



```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">
    <corners
        android:bottomLeftRadius="10dp"
        android:bottomRightRadius="10dp"
        android:radius="5dp"
        android:topLeftRadius="10dp"
        android:topRightRadius="10dp" />

    <gradient
        android:angle="0"
        android:centerColor="#cccccc"
        android:centerX="100"
        android:centerY="20"
        android:endColor="#abcdef"
        android:gradientRadius="100dp"
        android:startColor="#000000"
        android:type="linear"
        android:useLevel="false" />

    <solid android:color="#cccccc" />

    <stroke
        android:width="1dp"
        android:color="#cccccc"
        android:dashGap="2dp"
        android:dashWidth="50dp" />
</shape>
```

android:shape 表示图片的形状，选项：rectangle（矩形）、oval（椭圆）、line（横线）、ring（圆环）。默认值是矩形，另外line和ring这两个选项必须通过<stroke>标签来指定宽度和颜色，否则看不到效果。

<corners>  表示shape的四个角的角度（圆角程度）。只适用于矩形shape。其中android:radius是同时为4个角设置相同的角度，优先级较低，会被topLeftRadius这种具体指定角度的属性所覆盖。

<gradient>与<solid>标签相互排斥的，其中shlid表示纯色填充，而gradient表示渐变效果；gradient有如下几个属性:

- android:angle 渐变的角度，默认为0，其值必须是45的倍数，0表示从左往右，90表示从下到上。
- android:gradientRadius 渐变半径，仅当android:type="radial"时有效。
- android:type 渐变的类型，有linear(线性渐变）、radial(镜像渐变）、swepp(扫描线渐变）三种，默认是线性渐变。

<solid> 表示纯色填充，通过android:color即可指定shape中填充的颜色。

<stroke> Shape的描边，有如下属性：

- android:width 描边的宽度
- android:color 描边的颜色
- android:dashWidth 组成虚线的线段的宽度
- android:dashGap 注册虚线之间的间距

<size> shape的大小，有两个属性：android:width和android:height，分别表示shape的宽高。通过<size>标签指定宽高后，ShapeDrawable就有固定宽/高了。

### 6.2.3 LayerDrawable

它表示一种层次化的Drawable集合，通过将不同的Drawable放置在不同层后达到一种叠加效果。

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@+id/res_haimei1"
        android:bottom="10dp"
        android:drawable="@mipmap/haimei1"
        android:left="10dp"
        android:right="10dp"
        android:top="10dp" />

    <item
        android:id="@+id/res_icon"
        android:width="30dp"
        android:height="30dp"
        android:drawable="@mipmap/ic_launcher"
        android:gravity="center" />
</layer-list>
```

### 6.2.4 StateListDrawable

它表示Drawable集合，每个Drawable对应View的一种状态，这样系统就会根据View的状态来选择合适的Drawable。主要用于设置可点击View的背景。

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android" 
    android:constantSize="false" 
    android:dither="true" 
    android:variablePadding="false">
    <item android:drawable="@mipmap/ic_launcher" android:state_pressed="true" />
    <item android:drawable="@mipmap/haimei1" android:state_pressed="false" />
</selector>
```

### 6.2.5 LevelListDrawable

同样表示Drawable集合，集合中的每个Drawable都会有一个等级的概念，根据等级不同来切换对于的Drawable。当它作为View的背景时，可以通过Drawable的setLevel方法来设置不同的等级从而切换具体的Drawable。level的值从0-10000。

```xml
<?xml version="1.0" encoding="utf-8"?>
<level-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@mipmap/haimei1" android:maxLevel="0" />
    <item android:drawable="@mipmap/haimei2" android:maxLevel="1" />
</level-list>
```

### 6.2.6 TransitionDrawable

用来实现两个Drawable之间淡入淡出的效果。

```xml
<?xml version="1.0" encoding="utf-8"?>
<transition xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@mipmap/haimei2" />
    <item android:drawable="@mipmap/haimei3" />
</transition>
```

```java
TransitionDrawable drawable = (TransitionDrawable) imageView.getBackground();
drawable.startTransition(1000);
```

### 6.2.7 InsetDrawable

它可以将其他Drawable内嵌到自己当中，并可以在四周留下一定的间距。当一个View希望自己的背景比自己的实际区域小的时候，可以采用InsetDrawable来实现。

```xml
<?xml version="1.0" encoding="utf-8"?>
<inset xmlns:android="http://schemas.android.com/apk/res/android"
    android:drawable="@mipmap/haimei1"
    android:insetBottom="10dp"
    android:insetLeft="10dp"
    android:insetRight="10dp"
    android:insetTop="10dp">
    <shape android:shape="rectangle">
        <solid android:color="#abcdef" />
    </shape>
</inset>
```

### 6.2.8 ScaleDrawable

它可以根据自己的等级（level）将指定的Drawable缩放到一定比例。ScaleDrawable的xml所定义的缩放比例越大，那么内部Drawable看起来越小。如果level设置的越大，那么Drawable看起来就越大。

```xml
<?xml version="1.0" encoding="utf-8"?>
<scale xmlns:android="http://schemas.android.com/apk/res/android"
    android:drawable="@mipmap/haimei2"
    android:scaleGravity="center"
    android:scaleHeight="70%"
    android:scaleWidth="70%">
</scale>

ScaleDrawable scaleDrawable = (ScaleDrawable) imageView.getBackground();
caleDrawable.setLevel(1);
```

### 6.2.9 ClipDrawable

它也通过自己的当前等级（level）来裁剪另一个Drawable，裁剪方向通过android:clipOrientation和android：gravity这两个属性来共同控制。等级0表示完全裁剪，8000表示裁剪了2000，即设置方向上裁剪了20% 。

```xml
<?xml version="1.0" encoding="utf-8"?>
<clip xmlns:android="http://schemas.android.com/apk/res/android"
    android:clipOrientation="horizontal"
    android:drawable="@mipmap/haimei2"
    android:gravity="center">
</clip>

ClipDrawable clipDrawable = (ClipDrawable) ivLevei.getBackground();
clipDrawable.setLevel(7000);
```

## 6.3 自定义Drawable

1. draw、setAlpha、setColorFilter和getOpacity这几个方法都是必须要实现的，其中draw是最重要的。当自定义Drawable有固有大小的时候最好重写getIntrinsicWidth和getIntrinsicHeight这两个方法。
2. 内部大小不等于Drawable的实际区域大小，Drawable实际区域的大小可以通过getBounds方法来得到，一般来说它和View的尺寸相同。
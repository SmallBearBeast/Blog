## 快速设置背景样式的XmlDrawableUtil

- [前言](#前言)
- [用法](#用法)
- [链接](#链接)

### 前言
第一篇写点轻松简单的东西。
我们开发过程中设置背景方式就是通过定义drawable类型xml文件，最常用的就是shape和selector文件，放在drawable文件夹里。通过background属性引用。
这种方式不是不行，标准的开发规范，但是会有几个小问题。

1.drawable文件夹会有大量的xml文件，这块如果数量大的话可能会有占有一点包体大小，不介意当然可以忽略不计。

2.复用问题，如果不同View复用同一个xml背景文件，有一天某个View的背景需要改动那就会影响到另外一个View。
如果不复用，又会出现重复的xml文件。

3.使用selector设置背景实际上不仅仅只是一个文件，因为normal，press，check属性都需要单独的xml文件，使用起来更加繁琐且需要定义更多的xml文件。简直是套娃。

因此不如考虑通过代码设置背景，xml实际上也是转换成Drawable，再通过setBackground设置，直接创建对应的Drawable还可以省略掉xml解析过程，还可以稍微提高点效率。

所以这边提供了个工具类XmlDrawableUtil，主要是对GradientDrawable和StateListDrawable进行封装，提供快捷设置背景的方法。

XmlDrawableUtil主要是对圆角和圆形样式以及按压选中状态样式提供快捷的设置方法，尽量保证一行代码搞定背景。
目前不支持设置LayerDrawable背景，此样式用的较少，如有需要自行扩展。

### 用法
XmlDrawableUtil设置的颜色都是引用类型，必须定义在xml文件里，强制规范。

XmlDrawableUtil设置的长度单位是dp，非px。

比如：

XmlDrawableUtil.slRect(R.color.colorFF5722, R.color.colorFF9800, 5.0f).setView(mTv_1);

给mTv_1设置一个圆角为5dp矩形，正常颜色是FF5722，按压颜色是FF9800的背景。

包涵rect是设置背景为圆角矩形。

包涵circle是设置背景为圆形。

stroke表示设置边框，gradient表示设置渐变，alpha表示设置透明度，它们与rect和circle组合就形成各种各样的背景。

比如strokeRect方法表示设置带有边框的圆角矩形。

比如gradientCircle方法表示设置带有渐变的圆形。

比如alphaRect方法表示设置带有透明度的圆角矩形。

rect参数需要说明一下：

不带radius参数的说明不设置圆角。

带有1个radius参数说明设置四个圆角都为一样。

带有4个radius参数说明设置四个不同大小的圆角。

带有8个radius参数说明设置四个不同大小的圆角，每个圆角由两个半径确定。

其他个数的radius参数则抛出异常

XmlDrawableUtil.rect(R.color.color00BCD4,5.0f).setView(mTv_1);

![](https://raw.githubusercontent.com/SmallBearBeast/Blog/master/XmlDrawableUtil/Pic/Pic_1.jpg)

XmlDrawableUtil.circle(R.color.color00BCD4).setView(mTv_2);

![](https://raw.githubusercontent.com/SmallBearBeast/Blog/master/XmlDrawableUtil/Pic/Pic_2.jpg)

XmlDrawableUtil.alphaRect(0.1f, R.color.color00BCD4, 5).setView(mTv_3);

![](https://raw.githubusercontent.com/SmallBearBeast/Blog/master/XmlDrawableUtil/Pic/Pic_3.jpg)

XmlDrawableUtil.strokeCircle(R.color.color00BCD4, R.color.color000000, 2f).setView(mTv_4);

![](https://raw.githubusercontent.com/SmallBearBeast/Blog/master/XmlDrawableUtil/Pic/Pic_4.jpg)

XmlDrawableUtil.gradientRect(new int[]{R.color.color00BCD4, R.color.color000000}, GradientDrawable.Orientation.TOP_BOTTOM).setView(mTv_5);

![](https://raw.githubusercontent.com/SmallBearBeast/Blog/master/XmlDrawableUtil/Pic/Pic_5.jpg)


sl开头的方法是设置StateListDrawable，支持三种状态参数，对应normal，press，check。

比如：XmlDrawableUtil.slRect(R.color.colorFF5722, R.color.colorFF9800, 5.0f).setView(mTv_1);

给mTv_1设置一个圆角为5dp矩形，正常颜色是FF5722，按压颜色是FF9800的背景。

check属性只对于实现Checkable接口的View起作用，比如ToggleButton。

XmlDrawableUtil.slRect(R.color.color8BC34A, R.color.color00BCD4, 5.0f).setView(mTv_1);

![](https://raw.githubusercontent.com/SmallBearBeast/Blog/master/XmlDrawableUtil/Pic/Pic_6.webp)

XmlDrawableUtil.slAlphaCircle(1f, 0.5f, R.color.color8BC34A).setView(mTv_2);

![](https://raw.githubusercontent.com/SmallBearBeast/Blog/master/XmlDrawableUtil/Pic/Pic_7.webp)

XmlDrawableUtil.slGradientRect(new int[]{R.color.color8BC34A, R.color.color00BCD4}, new int[]{R.color.color8BC34A, R.color.colorFFEB3B}, GradientDrawable.Orientation.TOP_BOTTOM, 5).setView(mTv_3);

![](https://raw.githubusercontent.com/SmallBearBeast/Blog/master/XmlDrawableUtil/Pic/Pic_8.webp)

XmlDrawableUtil.slGradientCircle(new int[]{R.color.color8BC34A, R.color.color00BCD4}, new int[]{R.color.color8BC34A, R.color.colorFFEB3B}, GradientDrawable.Orientation.TOP_BOTTOM).setView(mTv_4);

![](https://raw.githubusercontent.com/SmallBearBeast/Blog/master/XmlDrawableUtil/Pic/Pic_9.webp)

XmlDrawableUtil.slRect(R.color.color8BC34A, R.color.color00BCD4, R.color.colorFF9800, 5f, 5f, 0f, 0f).setView(mTb_1);

给mTb_1设置正常颜色为#8BC34A，点击颜色00BCD4，选中颜色为FF9800，左上和右上半径为5dp圆角。

![](https://raw.githubusercontent.com/SmallBearBeast/Blog/master/XmlDrawableUtil/Pic/Pic_10.webp)

Drawable drawable_1 = XmlDrawableUtil.gradientCircle(new int[] {R.color.color8BC34A, R.color.color00BCD4}, GradientDrawable.Orientation.TOP_BOTTOM).getDrawable();

Drawable drawable_2 = XmlDrawableUtil.gradientCircle(new int[] {R.color.color8BC34A, R.color.colorFFEB3B}, GradientDrawable.Orientation.TOP_BOTTOM).getDrawable();

Drawable drawable_3 = XmlDrawableUtil.gradientCircle(new int[] {R.color.color8BC34A, R.color.colorFF9800}, GradientDrawable.Orientation.TOP_BOTTOM).getDrawable();

XmlDrawableUtil.selector(drawable_1, drawable_2, drawable_3).setView(mTb_2);

给mTb_2设置三种状态的不同渐变色背景，这里可以通过XmlDrawableUtil获取Drawable。
通过selector设置三种drawable。

![](https://raw.githubusercontent.com/SmallBearBeast/Blog/master/XmlDrawableUtil/Pic/Pic_11.webp)
### 链接
代码我直接放在gist里，就一个文件，复制粘贴即可使用，根据自身项目需要自行修改。

代码比较简单，就是一些封装，就不多加赘述了，封装方式可能会比较友好，感兴趣的可以看一看。

如有问题和意见，欢迎大家指点和讨论。拜了个拜。

[XmlDrawableUtil.java](https://gist.github.com/SmallBearBeast/4e836dcab7080f3a336e35d4db532575)
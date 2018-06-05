# Frame Animation （帧动画）#

## 1：原理 ##

Frame Animation 实际上就是Drawable Animation，它将动画分解为帧的形式，预先定义好每一帧，按顺序播放每一组预先定义好的图片即可。这种动画的实质是Drawable，所以这种动画的XML定义方式文件一般放在 /res/drawable/ 目录下。

## 2：作用对象 ##

Frame Animation是作用于Android的视图控件（View）上面的。

## 3：API ##

![](/Resources/FrameAnimation_img1.png)

## 4:实现 ##

### 4.1 XML方式实现 ###

#### 4.1.1 Drawable Animation 的XML形式说明 ####

Drawable Animation 在XML中的表现形式如下：

	<!-- Animation frames are wheel0.png through wheel5.png
	     files inside the res/drawable/ folder -->
	 <animation-list android:id="@+id/selected" android:oneshot="false">
	    <item android:drawable="@drawable/wheel0" android:duration="50" />
	    <item android:drawable="@drawable/wheel1" android:duration="50" />
	    <item android:drawable="@drawable/wheel2" android:duration="50" />
	    <item android:drawable="@drawable/wheel3" android:duration="50" />
	    <item android:drawable="@drawable/wheel4" android:duration="50" />
	    <item android:drawable="@drawable/wheel5" android:duration="50" />
	 </animation-list>

`<animation-list>` 必须是根节点，包含一个或者多个`<item>`元素，属性有：

`android:oneshot` `true`代表只执行一次，`false`循环执行。

`<item>` 类似一帧的动画资源。

`<item>` `animation-list`的子项，包含属性如下：

`android:drawable` 一个frame的Drawable资源。

`android:duration` 一个frame显示多长时间。

#### 4.1.2 实现步骤 ####

- 1.将动画资源（图片）放到drawable文件夹里

可以找到自己需要的gif动画，用gif动画分解软件（Gifsplitter）分解成一张张图片即可。

- 2.在/res/drawable 中新建一个以 animation-list 为根节点的 Drawable resources file，然后设置动画资源。

- 3.在Java代码中设置并启动动画


### 4.2：Java代码方式实现

## 5：注意 ##

特别注意，AnimationDrawable的start()方法不能在Activity的onCreate方法中调运，因为AnimationDrawable还未完全附着到window上，所以最好的调运时机是onWindowFocusChanged()方法中。

## 6：特点 ##

- 优点：使用方便简单
- 缺点：容易引起 OOM，因为会使用大量 的图片资源 ，所以要尽量避免使用尺寸较大的图片

## 7：应用场景 ##

较为复杂的个性化动画效果。

## 8： ##
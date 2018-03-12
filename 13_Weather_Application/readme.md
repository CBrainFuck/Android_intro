# Weather application

天气应用是每个人都在使用的应用，我们的安卓应用学习就从动手制作一个天气应用开始！让我们一起来设计一个十分酷炫的天气应用吧，我们将首先完成它的制作然后再讨论一些细节，整个过程都将由大家亲手完成！
将我们今天的任务稍微细分一下，主要包括以下这些:
- [前端：在android studio中设计这个应用的界面](#前端：在android—studio中设计这个应用的界面)
- [后端：将应用连上网络并依据天气预报信息对内容进行更新](#后端：将应用连上网络并依据天气预报信息对内容进行更新)


# 前端：在android studio中设计这个应用的界面

设计界面主要包含以下步骤:
1. 重绘(Redraw)并截出(slice)应用所需要的图片
2. 创建一个新的安卓项目并完成界面上的核心设计
3. 进一步润色设计同时改善(refactor)代码

这就是我们最终想要达到的界面效果:

![Weather application - Design](display/need_to_achieve_weather_app.gif)

怎么样，是不是很像一款成熟的产品？仔细看一下这个实际，我们需要绘制的图片有：天气条件图标（有风，晴等等），上面的一个小铃铛以及看起来像钟乳石的小锯齿。 
首先我们需要使用adobe illustator来绘制这些图片, 通过它我们可以得到高质量的矢量图。接下来我们只需要截出我们需要的图片就可以了。

![Weather app design redraw](display/re_draw.gif)

对于最初的界面设计来说，我们只需要以下的5张图片

![Image slices](display/slices.gif)

现在是时候来创建一个包含空activity组件的安卓项目了：

![Creating a new project with an empty Activity](display/create_a_new_project.gif)

我们新创建的项目就像这样。 

![Newly created project](display/new_project.gif)

为了保证是否所有一切都在正常工作，我们创建一个新项目后应该立即执行一下以来确认，在设计和开发的过程中，一旦对应用进行了较大的修改也应该运行立即应用来测试，这是一个很好也很重要的习惯。
我使用的虚拟设备规格如下: 

- 设备名:Galaxy Nexus API 23
- 高度:1280
- 宽度:720
- API等级:23


我们的设计工作将会主要在'activity_main.xml'文件中完整，该文件位于res/layout'文件夹。就当前的进度而言，这个文件中的内容应该如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="mg.studio.weather.MainActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</android.support.constraint.ConstraintLayout>

```

在这个应用中我们将会混合使用[LinearLayout（线性布局）](https://developer.android.com/guide/topics/ui/layout/linear.html)和[RelativeLayout（相对布局）](https://developer.android.com/guide/topics/ui/layout/relative.html)而不用自动生成的[ConstraintLayout（约束布局）](https://developer.android.com/training/constraint-layout/index.html)。我们进行如下的修改：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context="mg.studio.weather.MainActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</LinearLayout>

```

现在仍然有自动生成的'TextView' 在我们的'LinearLayout'中。移除它以来确保对自动生成的布局清楚干净。注意 'android:orientation="vertical"'制定了'LinearLayout'的方向。


```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context="mg.studio.weather.MainActivity">

</LinearLayout>

```

现在我们来修改xml代码以展现出我们设想的界面设计效果。 

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#2495d1"
    android:orientation="vertical"
    android:weightSum="8"
    tools:context="mg.studio.weather.MainActivity">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_marginTop="10dp"
        android:layout_weight="1"
        android:gravity="center"
        android:text="Sunday"
        android:textAllCaps="true"
        android:textColor="#10000000"
        android:textSize="36sp"
        android:textStyle="bold" />

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_marginBottom="10dp"
        android:layout_weight="2"
        android:orientation="vertical">

        <ImageView
            android:id="@+id/imageView"
            android:layout_width="48dp"
            android:layout_height="48dp"
            android:layout_gravity="center"
            app:srcCompat="@mipmap/ic_launcher" />

        <TextView
            android:id="@+id/tv_news"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="center"
            android:layout_margin="8dp"
            android:text="You have 1 appointment"
            android:textSize="10sp" />

        <Button
            android:id="@+id/button"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="center"
            android:background="@drawable/ic_launcher_background"
            android:gravity="center"
            android:paddingLeft="40dp"
            android:paddingRight="40dp"
            android:text="Go to Calendar"
            android:textColor="#50ffffff" />
    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="3"
        android:orientation="vertical"
        android:weightSum="3">

        <RelativeLayout
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="2">

            <LinearLayout
                android:id="@+id/linearLayout"
                android:layout_width="wrap_content"
                android:layout_height="match_parent"
                android:layout_alignParentLeft="true"
                android:layout_margin="16dp"
                android:gravity="center_vertical"
                android:orientation="vertical">

                <ImageView
                    android:id="@+id/img_weather_condition"
                    android:layout_width="48dp"
                    android:layout_height="48dp"
                    android:layout_gravity="center"
                    app:srcCompat="@mipmap/ic_launcher" />

                <TextView
                    android:id="@+id/tv_location"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_gravity="center"
                    android:text="Location"
                    android:textColor="@android:color/white"
                    android:textStyle="bold" />

                <TextView
                    android:id="@+id/tv_date"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="03/03/2018"
                    android:textColor="@android:color/white" />

            </LinearLayout>

            <LinearLayout
                android:id="@+id/tv_temperature"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_alignParentEnd="true"
                android:layout_alignParentRight="true"
                android:layout_centerVertical="true"
                android:layout_marginEnd="30dp"
                android:layout_marginRight="30dp"
                android:orientation="horizontal">

                <TextView
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="18"
                    android:textColor="@android:color/white"
                    android:textSize="100sp"
                    android:textStyle="bold" />

                <TextView
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_gravity="right"
                    android:text="°C"
                    android:textColor="@android:color/white"
                    android:textSize="22sp" />
            </LinearLayout>


        </RelativeLayout>

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1">

            <ImageView
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:scaleType="fitXY"
                app:srcCompat="@mipmap/ic_launcher" />
        </LinearLayout>
    </LinearLayout>


    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="2"
        android:background="@android:color/white"
        android:orientation="horizontal"
        android:weightSum="4"
        tools:layout_editor_absoluteX="8dp"
        tools:layout_editor_absoluteY="380dp">

        <LinearLayout
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:gravity="center"
            android:orientation="vertical">

            <ImageView
                android:layout_width="48dp"
                android:layout_height="48dp"
                app:srcCompat="@mipmap/ic_launcher" />

            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:gravity="center"
                android:text="mon"
                android:textAllCaps="true"
                android:textColor="#909090" />
        </LinearLayout>

        <LinearLayout
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:gravity="center"
            android:orientation="vertical">

            <ImageView
                android:layout_width="48dp"
                android:layout_height="48dp"
                app:srcCompat="@mipmap/ic_launcher" />

            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:gravity="center"
                android:text="tue"
                android:textAllCaps="true"
                android:textColor="#909090" />
        </LinearLayout>

        <LinearLayout
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:gravity="center"
            android:orientation="vertical">

            <ImageView
                android:layout_width="48dp"
                android:layout_height="48dp"
                app:srcCompat="@mipmap/ic_launcher" />

            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:gravity="center"
                android:text="thu"
                android:textAllCaps="true"
                android:textColor="#909090" />
        </LinearLayout>

        <LinearLayout
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:gravity="center"
            android:orientation="vertical">

            <ImageView
                android:layout_width="48dp"
                android:layout_height="48dp"
                app:srcCompat="@mipmap/ic_launcher" />

            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:gravity="center"
                android:text="fri"
                android:textAllCaps="true"
                android:textColor="#909090" />
        </LinearLayout>
    </LinearLayout>
</LinearLayout>

```

现在我们的布局看起来就像这样：

![Current layout](display/layout_part_one.gif)

为了确保在实际运行时与我们设计的界面效果一样，我在虚拟机中运行了一下，以下是输出结果：

![Primary design](display/design_version_01.gif)


我们要把之前创建的图片拷贝到drawable文件夹。如果大家现在都紧跟着我的进度，现在就要有一些事情需要注意。默认情况下, Android Studio将你的项目文件展示在安卓视图(Android view)中。而这个视图并不会展示出文件在硬盘上的真实结构，但是它是以模块的概念组织起来的并且通过文件类型来简化项目中关键资源文件之间的关系,它隐藏起来了真实文件结构和平常使用的目录. ([Read more](https://developer.android.com/studio/projects/index.html))

![The drawable folder](display/drawable.gif)


为了确保这些图片都放到了我们想要的放到的文件夹,将他们粘贴到drawable文件夹中，如下图所示。

![Paste the images in the drawable folder](display/paste_drawables.gif)

现在我们可以使用这些图片了,但在这之前,让我们先删除应用顶部的工具栏（toolbar）.
因为我们想要应用使用整个屏幕，所以我们要移除行动条（action bar）. 

![Primary design](display/manifest_no_action_bar.gif)

来看一看这个对图片的小改动带来的影响，前后的效果如下图左右所示：

![Before and after removing the action bar](display/toolbar_none.gif)

接下来我们继续处理添加到项目里的这些图片. 我们要把目前指向'app:srcCompat="@mipmap/ic_launcher'的路径全部替换,这个路径将会在我们的应用中展示绿色的安卓标志,我们用要使用的图片的正确路径来替换. 
我们的图片全部都在drawable文件夹, 通过layout xml文件我们可以非常简单的处理他们就像这样：'app:srcCompat="@drawable/image_name_without_the_extension'

一个非常友好的事情是，android studio将会通过自动补全函数来帮我们快速得到图片正确的全称.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#2495d1"
    android:orientation="vertical"
    android:weightSum="8"
    tools:context="mg.studio.weather.MainActivity">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_marginTop="10dp"
        android:layout_weight="1"
        android:gravity="center"
        android:text="Sunday"
        android:textAllCaps="true"
        android:textColor="#10000000"
        android:textSize="36sp"
        android:textStyle="bold" />

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_marginBottom="10dp"
        android:layout_weight="2"
        android:orientation="vertical">

        <ImageView
            android:id="@+id/imageView"
            android:layout_width="48dp"
            android:layout_height="48dp"
            android:layout_gravity="center"
            app:srcCompat="@drawable/notification" />

        <TextView
            android:id="@+id/tv_news"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="center"
            android:layout_margin="8dp"
            android:text="You have 1 appointment"
            android:textSize="10sp" />

        <Button
            android:id="@+id/button"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="center"
            android:background="@drawable/ic_launcher_background"
            android:gravity="center"
            android:paddingLeft="40dp"
            android:paddingRight="40dp"
            android:text="Go to Calendar"
            android:textColor="#50ffffff" />
    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="3"
        android:orientation="vertical"
        android:weightSum="3">

        <RelativeLayout
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="2">

            <LinearLayout
                android:id="@+id/linearLayout"
                android:layout_width="wrap_content"
                android:layout_height="match_parent"
                android:layout_alignParentLeft="true"
                android:layout_margin="16dp"
                android:gravity="center_vertical"
                android:orientation="vertical">

                <ImageView
                    android:id="@+id/img_weather_condition"
                    android:layout_width="48dp"
                    android:layout_height="48dp"
                    android:layout_gravity="center"
                    app:srcCompat="@drawable/rainy_up" />

                <TextView
                    android:id="@+id/tv_location"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_gravity="center"
                    android:text="Location"
                    android:textColor="@android:color/white"
                    android:textStyle="bold" />

                <TextView
                    android:id="@+id/tv_date"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="03/03/2018"
                    android:textColor="@android:color/white" />

            </LinearLayout>

            <LinearLayout
                android:id="@+id/tv_temperature"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_alignParentEnd="true"
                android:layout_alignParentRight="true"
                android:layout_centerVertical="true"
                android:layout_marginEnd="30dp"
                android:layout_marginRight="30dp"
                android:orientation="horizontal">

                <TextView
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="18"
                    android:textColor="@android:color/white"
                    android:textSize="100sp"
                    android:textStyle="bold" />

                <TextView
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_gravity="right"
                    android:text="°C"
                    android:textColor="@android:color/white"
                    android:textSize="22sp" />
            </LinearLayout>


        </RelativeLayout>

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1">

            <ImageView
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:scaleType="fitXY"
                app:srcCompat="@drawable/design" />
        </LinearLayout>
    </LinearLayout>


    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="2"
        android:background="@android:color/white"
        android:orientation="horizontal"
        android:weightSum="4"
        tools:layout_editor_absoluteX="8dp"
        tools:layout_editor_absoluteY="380dp">

        <LinearLayout
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:gravity="center"
            android:orientation="vertical">

            <ImageView
                android:layout_width="48dp"
                android:layout_height="48dp"
                app:srcCompat="@drawable/rainy_small" />

            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:gravity="center"
                android:text="mon"
                android:textAllCaps="true"
                android:textColor="#909090" />
        </LinearLayout>

        <LinearLayout
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:gravity="center"
            android:orientation="vertical">

            <ImageView
                android:layout_width="48dp"
                android:layout_height="48dp"
                app:srcCompat="@drawable/partly_sunny_small" />

            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:gravity="center"
                android:text="tue"
                android:textAllCaps="true"
                android:textColor="#909090" />
        </LinearLayout>

        <LinearLayout
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:gravity="center"
            android:orientation="vertical">

            <ImageView
                android:layout_width="48dp"
                android:layout_height="48dp"
                app:srcCompat="@drawable/windy_small" />

            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:gravity="center"
                android:text="thu"
                android:textAllCaps="true"
                android:textColor="#909090" />
        </LinearLayout>

        <LinearLayout
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:gravity="center"
            android:orientation="vertical">

            <ImageView
                android:layout_width="48dp"
                android:layout_height="48dp"
                app:srcCompat="@drawable/sunny_small" />

            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:gravity="center"
                android:text="fri"
                android:textAllCaps="true"
                android:textColor="#909090" />
        </LinearLayout>
    </LinearLayout>
</LinearLayout>


```

![After adding the images to the design](display/design_version_02.gif)

到目前为止我们的工作完成的很好，接下来将要更新按钮。按钮也可以使用一个图片来展示就像之前我们做的那样，不过我们可以通过IDE本身提供的形状（shapes）来达到我们想要的设计。
在安卓中我们可以把一个按钮的状态分为两类：普通状态，被点击状态. 我们可以为每个状态指定一种设计.
我们将要在drawable文件夹中创建三个新文件. 一个选择器以及两个其他的文件来为按钮定义形状.

- button_selector.xml
- button_shape.xml
- button_shape_pressed.xml

下面展示了我的button_selector文件：

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <!--When the button is pressed-->
    <item android:drawable="@drawable/button_shape_pressed" android:state_pressed="true" />
    <!--Default-->
    <item android:drawable="@drawable/button_shape" />
</selector>

```

下面定义了按钮的形状:
button_shape:

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">
    <solid android:color="#31a7dc" />
    <corners android:radius="90dp" />
</shape>
```

当按钮被点击时:

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">
    <solid android:color="#2191ce" />
    <corners android:radius="90dp" />
</shape>
```

接下来只需要使用选择器(button_selector.xml) 来作为我们按钮的背景即可.

之前: 

```xml
    android:background="@drawable/ic_launcher_background"

```

之后:

```xml
    android:background="@drawable/button_selector"

```

来看看在输出上的不同:

![Button before and after using the selector](display/button.gif)

到此界面设计基本就要完成了，但仍有一些事情需要设置. 我们在设计时总是下意识的认为用户会在竖屏模式下使用我们的应用.如果我们在应用运行时将虚拟机旋转一下，来看看结果:

![In Landscape mode, the design does not look good](display/landscape.gif)

我们可以通过将这个activity组件的屏幕布局方向设置为无论手机如何旋转都处于竖屏模式来暂时解决这个问题。为此需要更新我们的android manifest文件.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="mg.studio.weather">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.NoActionBar">
        <activity android:name=".MainActivity"
            android:screenOrientation="portrait">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```
所以现在即便手机旋转到了横屏模式，此应用的此活动组件页面也不会旋转，我们所作的界面设计也不会因此而变得乱七八糟。

# 后端：将应用连上网络并依据天气预报信息对内容进行更新

为了使应用可以连接上互联网，有一些事情是我们必须提前知道的：

1. 如何来监听一个按钮?  这是一个非常基础但也十分简单的问题。我已经发布了[how to display a toast when a button is clicked](https://github.com/dragona/Android_intro/tree/master/03_Button_Toast)

2. 权限许可: 为了保护设备所有者的隐私, 安卓设置了一些权限许可。正因为如此，某些时候我们所创建的应用需要获得用户的一些许可，比如能够连接网络的权限许可。如果你有时间的话关于安卓权限问题可以参考, [read more](https://developer.android.com/guide/topics/permissions/overview.html) .对于这个天气应用来说, 我们只需要简单明了的把应用需要连接网络的权限请求放在AndroidManifest文件即可。

```xml
    <uses-permission android:name="android.permission.INTERNET"/>

```

3. AsyncTask（异步任务）: 这是一个添加在API level 3中的类，它可以帮助我们正确并简单的使用界面线程（UI thread）.在这个天气应用中, 我们需要从互联网上获取一些数据, 这是一个正常情况下几秒之内就完成的行为 (在一个好的网络环境中). 我们需要使用 [asynchronous task](https://developer.android.com/reference/android/os/AsyncTask.html) 来在界面背后处理下载来的数据并且避免界面停顿的情况.

4. ConnectivityManager（连接管理）: 当我们的设备没有联网时我们就不需要去网上获取数据. 这一行为应该由我们的天气应用自己来监控.[ConnectivityManager](https://developer.android.com/reference/android/net/ConnectivityManager.html) 这个类早在API 1时代就被添加进来了，我们可以利用它来监测网络连接情况.


现在我们知道了以上的四点, 我们完成这个天气应用的计划也变成了这样: 对按钮添加一个监听，当我们按它时, 就可以依据所下载的最新天气预报来产生即将展示在应用界面上的天气信息。


我们之前在xml文件中完成的对按钮的设计,不言而喻, 我们也可以在同样的文件中对它设置监听.

![Add listener to a button from the xml](display/btn_click.gif)

现在在xml文件中对于该按钮的描述变成了这样:

```xml
 <Button
            android:id="@+id/button"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="center"
            android:background="@drawable/button_selector"
            android:gravity="center"
            android:paddingLeft="40dp"
            android:paddingRight="40dp"
            android:onClick="btnClick"
            android:text="Go to Calendar"
            android:textColor="#50ffffff" />

```

我们的MainActivity.java文件也终于被更新了： 

```java
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    public void btnClick(View view) {
        //to do when the button is clicked

    }
}

```

按照我们最初的设想, 当按钮被点击时,这个应用将会从互联网上下载数据，并且将最新的温度值展示出来.首先我们要使用按钮来改变在xml中设置的温度初始值.
这里是我们需要做的事情:

- 为这个当按钮被点击时，展示温度并且需要动态更新的TextView指定一个名字.所起的名字稍后将会用来辨认这个TextView.
- 添加代码以在按钮被点击时更新TextView的内容

![Add an ID to the TextView that displays the temperature](display/textview_id.gif)

```xml

    <TextView
        android:id="@+id/temperature_of_the_day"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="18"
        android:textColor="@android:color/white"
        android:textSize="100sp"
        android:textStyle="bold" />

```

```java
public void btnClick(View view) {
        ((TextView)findViewById(R.id.temperature_of_the_day)).setText("27");
    }
```


![Outcome when the button is pressed](display/change_txt_on_btn_clicked.gif)

现在按钮可以按预想工作了, 我们需要在manifest文件中添加权限申请以来访问互联网, 以及使用异步任务（asynctask）来从服务器上下载最新的天气预报信息.

正如之前已经提到的那样,下面的代码需要添加到manifest文件中以来进行访问互联网权限许可的请求.

```xml
<uses-permission android:name="android.permission.INTERNET"/>
```

我们需要在MainActivity.java文件中使用下面这个类以来从服务器获得最新的天气预报信息.

```java

    private class DownloadUpdate extends AsyncTask<String, Void, String> {


        @Override
        protected String doInBackground(String... strings) {
            String stringUrl = "http://mpianatra.com/Courses/info.txt";
            HttpURLConnection urlConnection = null;
            BufferedReader reader;

            try {
                URL url = new URL(stringUrl);

                // Create the request to get the information from the server, and open the connection
                urlConnection = (HttpURLConnection) url.openConnection();

                urlConnection.setRequestMethod("GET");
                urlConnection.connect();

                // Read the input stream into a String
                InputStream inputStream = urlConnection.getInputStream();
                StringBuffer buffer = new StringBuffer();
                if (inputStream == null) {
                    // Nothing to do.
                    return null;
                }
                reader = new BufferedReader(new InputStreamReader(inputStream));

                String line;
                while ((line = reader.readLine()) != null) {
                    // Mainly needed for debugging
                    buffer.append(line + "\n");
                }

                if (buffer.length() == 0) {
                    // Stream was empty.  No point in parsing.
                    return null;
                }
                //The temperature
                return buffer.toString();

            } catch (MalformedURLException e) {
                e.printStackTrace();
            } catch (ProtocolException e) {
                e.printStackTrace();
            } catch (IOException e) {
                e.printStackTrace();
            }

            return null;
        }

        @Override
        protected void onPostExecute(String temperature) {
            //Update the temperature displayed 
            ((TextView) findViewById(R.id.temperature_of_the_day)).setText(temperature);
        }
    }
```

应该注意到，在上述的测试连接中从服务器取回的信息只是一个字符串（string）27。 如若服务器返回了不同格式的信息则可能需要我们进行更进一步的字符解析.

到目前为止,只要有互联网连接我们就可以从服务器下载最新的天气温度信息.那么当没有网络连接时我们的应用会怎么样呢?


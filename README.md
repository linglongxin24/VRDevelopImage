#【Android VR开发】一.给用户呈现一个360°全景图片

>VR即Virtual Reality虚拟现实。虚拟现实技术是一种可以创建和体验虚拟世界的计算机仿真系统它利用计算机生成一种模拟环境是一种多源信息融合的交互式的三维动态视景和实体行为的系统仿真使用户沉浸到该环境中。
那么，如何在Android中去开发VR功能的APP呢？我们利用谷歌提供的开源SDK去实现一个360°全景图片的功能

![](https://github.com/linglongxin24/VRDevelopImage/blob/master/screenshot/Screenshot_2016-12-23-14-10-28-903_VRDevelop.png?raw=true)
![](https://github.com/linglongxin24/VRDevelopImage/blob/master/screenshot/Screenshot_2016-12-23-14-10-41-700_VRDevelop.png?raw=true)

#一.在build.gradle中引入谷歌VR的SDK依赖

```gralde
 compile 'com.google.vr:sdk-panowidget:1.10.0'
```

#二.注意支持的最小SDK

```gradle
  minSdkVersion 19
  targetSdkVersion 25
```

#三.界面布局文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context="cn.bluemobi.dylan.vrdevelop.MainActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="AndroidVR开发360度全景图片" />

    <com.google.vr.sdk.widgets.pano.VrPanoramaView
        android:id="@+id/vr_pan_view"
        android:layout_width="match_parent"
        android:layout_marginTop="20dp"
        android:layout_height="250dp"></com.google.vr.sdk.widgets.pano.VrPanoramaView>
</LinearLayout>

```

#四.加载360°全景图片

```java

    /**
     * 加载360度全景图片
     */
    private void load360Image() {
        vr_pan_view = (VrPanoramaView) findViewById(R.id.vr_pan_view);
        /**获取assets文件夹下的图片**/
        InputStream open = null;
        try {
            open = getAssets().open("andes.jpg");
        } catch (IOException e) {
            e.printStackTrace();
        }
        Bitmap bitmap = BitmapFactory.decodeStream(open);
        /**设置加载VR图片的相关设置**/
        VrPanoramaView.Options options = new VrPanoramaView.Options();
        options.inputType = VrPanoramaView.Options.TYPE_STEREO_OVER_UNDER;
        /**设置加载VR图片监听**/
        vr_pan_view.setEventListener(new VrPanoramaEventListener() {
            /**
             * 显示模式改变回调
             * 1.默认
             * 2.全屏模式
             * 3.VR观看模式，即横屏分屏模式
             * @param newDisplayMode 模式
             */
            @Override
            public void onDisplayModeChanged(int newDisplayMode) {
                super.onDisplayModeChanged(newDisplayMode);
                Log.d(TAG, "onDisplayModeChanged()->newDisplayMode=" + newDisplayMode);
            }

            /**
             * 加载VR图片失败回调
             * @param errorMessage
             */
            @Override
            public void onLoadError(String errorMessage) {
                super.onLoadError(errorMessage);
                Log.d(TAG, "onLoadError()->errorMessage=" + errorMessage);
            }

            /**
             * 加载VR图片成功回调
             */
            @Override
            public void onLoadSuccess() {
                super.onLoadSuccess();
                Log.d(TAG, "onLoadSuccess->图片加载成功");
            }

            /**
             * 点击VR图片回调
             */
            @Override
            public void onClick() {
                super.onClick();
                Log.d(TAG, "onClick()");
            }
        });
        /**加载VR图片**/
        vr_pan_view.loadImageFromBitmap(bitmap, options);
    }

```

#五.[GitHub](https://github.com/linglongxin24/VRDevelopImage)
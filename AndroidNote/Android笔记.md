#### 查看手机当前页面Activity名称
- 调式模式连接电脑,控制台此命令输入即可
  ```
  adb shell dumpsys activity | findstr "mFocusedActivity"
  ```
  
#### Android Studio远程调式安卓设备
  将Android Studio安装ADB Wife插件，使用数据线连接。如果没有数据线，并且设备root了。可以使用WirelessADB这个应用打开设备的ADB调式模式

  - 让设备监听TCP/IP,端口号5555
    ```
    adb tcpip 5555
    ```

  - 连接设备ip
    ```
    adb connect 192.168.1.122
    ```
  - 查看主机连接的安卓设备
    ```
    adb devices
    ```
  - 访问手机,然后可以通过命令查看sd卡内容，并操作
    ```
    adb shell
    ```
#### 兼容Android8.0版本更新无法弹出安装页面的bug
  - 因为8.0添加了新的安全措施，不允许应用内安装未经过Google play验证的应用,所以添加下面这个权限即可解决问题
    ```
    <uses-permission android:name="android.permission.REQUEST_INSTALL_PACKAGES"/>
    ```
#### PopWindow弹出位置7.0、7.1版本兼容
  ```java
  /**
     *
     * @param pw     popupWindow
     * @param anchor v
     * @param xoff   x轴偏移
     * @param yoff   y轴偏移
     */
    public static void showAsDropDown(final PopupWindow pw, final View anchor, final int xoff, final int yoff) {
        if (Build.VERSION.SDK_INT >= 24) {
            Rect visibleFrame = new Rect();
            anchor.getGlobalVisibleRect(visibleFrame);
            int height = anchor.getResources().getDisplayMetrics().heightPixels - visibleFrame.bottom;
            pw.setHeight(height);
            pw.showAsDropDown(anchor, xoff, yoff);
        } else {
            pw.showAsDropDown(anchor, xoff, yoff);
        }
    }
  ```

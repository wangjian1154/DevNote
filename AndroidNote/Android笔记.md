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

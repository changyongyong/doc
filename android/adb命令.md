# adb命令

## 基础命令
查看连接设备
```
adb devices
```

安装app
```
adb install ./x.apk
```

当前活动界面的信息
```
adb shell dumpsys activity top 
```

查看当前界面的app的包名等信息
```
adb shell dumpsys window windows | findstr mFocusedApp    // window下
adb shell dumpsys window windows | grep mFocusedApp     // mac
```

获取包的信息
```
adb shell dumpsys package [packageName]
```

查看启动的app的包名
```
adb shell dumpsys activity top | find "ACTIVITY"
```

查看所有启动的应用的包名
```
adb shell dumpsys activity activities | findstr "Run"
```

获取内存数据
```
adb shell dumpsys meminfo [packageName/pid]
```

获取数据库文件
```
adb shell dumpsys dbinfo [packageName]
```

列出当前所安装的程序包
```
adb shell pm list packages
```

获取制定程序所在路径
```
adb shell pm path [packageName]
```

发送文件到手机
```
adb push /电脑文件 /手机目录
```

从手机拉文件到电脑
```
adb pull /手机文件 /电脑文件夹
```

## 进阶命令

启动app
```
adb shell am start -n com.ss.android.ugc.aweme/com.ss.android.ugc.aweme.main.MainActivity
```

点击，原点在屏幕左上角，540：x轴 1890：y轴
```
adb shell input mouse tap 540 1890
```

连续点击
```
adb shell "seq 20 | while read i;do input mouse tap 540 850 & input mouse tap 540 850 & sleep 0.01;done"
```

tap 点击的意思，x,y – 要点击的位置的横纵轴坐标
```
adb shell input touchscreen tap 110 66
```

swipe 滑动，x1 y1 x2 y2 – 滑动起始和终止位置的横纵轴坐标
```
adb shell input touchscreen swipe 930 880 930 380 //向上滑
adb shell input touchscreen swipe 930 880 330 880 //向左滑
adb shell input touchscreen swipe 330 880 930 880 //向右滑
adb shell input touchscreen swipe 930 380 930 880 //向下滑
```

文本输入hello
```
adb shell input text hello
```

安卓运行jar服务
需要编译成dex格式，当然后缀可以改成jar，推送文本文件到安卓系统的tmp文件夹下，该文件夹有运行权限
```
adb push test.jar /data/local/tmp
adb push test.dex /data/local/tmp
```

运行
```
adb shell CLASSPATH=/data/local/tmp/test.jar app_process /data/local/tmp /org.example.App org.example.App 0 8000000 false - false
```

运行test.jar的HelloWorld类，需要有Main方法
```
app_process -Djava.class.path=/data/local/tmp/test.jar /data/local/tmp HelloWorld
app_process -Djava.class.path=/data/local/tmp/test.dex /data/local/tmp org.example.HelloWorld
```



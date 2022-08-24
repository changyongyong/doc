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

获取包的信息
```
adb shell dumpsys package [packageName]
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


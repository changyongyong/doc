# Frida的用法
frida官网：https://frida.re/

## 1、服务端环境

下载对应平台的服务端包   
https://github.com/frida/frida/releases

### Android平台
1、开发环境需要有adb工具，方便对手机操作   
2、下载安卓包，要对应手机cpu等硬件版本[frida-server-15.2.2-android-arm64.xz]，减压获取一个可以运行的包   
3、注意：该平台下，手机需要有root权限

#### 查看系统的版本
```
cat /proc/cpuinfo
```
#### 查看cpu架构
```
getprop ro.product.cpu.abi
```
#### 执行以下命令将服务端推到手机的/data/local/tmp目录
```
adb push frida-server /data/local/tmp/frida-server
```

#### 执行以下命令修改frida-server文件权限
```
adb shell chmod 777 /data/local/tmp/frida-server
```

## 2、客户端环境

### 2.1、安装frida客户端（python运行）
```
pip install frida-tools
```

### 2.2、node客户端
使用js脚本开发，需要安装该插件
```
npm i "@types/frida-gum" -S
```

### 2.3、查看系统运行进程
```
frida-ps -U
```

### 2.4、运行脚本
下面是frida客户端命令行的参数帮助  
```
Usage: frida [options] target
Options:
  --version             show program's version number and exit
  -h, --help            show this help message and exit
  -D ID, --device=ID    connect to device with the given ID
  -U, --usb             connect to USB device
  -R, --remote          connect to remote frida-server
  -H HOST, --host=HOST  connect to remote frida-server on HOST
  -f FILE, --file=FILE  spawn FILE
  -n NAME, --attach-name=NAME
                        attach to NAME
  -p PID, --attach-pid=PID
                        attach to PID
  --debug               enable the Node.js compatible script debugger
  --enable-jit          enable JIT
  -l SCRIPT, --load=SCRIPT
                        load SCRIPT
  -c CODESHARE_URI, --codeshare=CODESHARE_URI
                        load CODESHARE_URI
  -e CODE, --eval=CODE  evaluate CODE
  -q                    quiet mode (no prompt) and quit after -l and -e
  --no-pause            automatically start main thread after startup
  -o LOGFILE, --output=LOGFILE
                        output to log file
```

#### 3.1、spawn模式运行frida脚本
该模式下，app是frida启动的，启动的app会重启，所以在app启动过程的可以注入js脚本   
【参数解释】：    
-f 指定一个进程，重启它并注入脚本   
--no-pause 自动运行程序  
```
frida -U --no-pause -f [包名] -l [js脚本]
```

#### 3.2、attach模式运行frida脚本
程序已经运行中，所以在打开app过程无法注入js脚本
```
frida -U [进程编号] -l [js脚本]
```

## 3、Hook Java方法

### 3.1 载入类
Java.use方法用于加载一个Java类，相当于Java中的Class.forName()。比如要加载一个String类：
```
var StringClass = Java.use("java.lang.String");
```

### 3.2 加载内部类：
```
var MyClass_InnerClass = Java.use("com.xxx.MyClass$InnerClass");
```
其中InnerClass是MyClass的内部类

### 3.3 修改函数
3.3.1 一般函数
修改函数名为func，参数为String类型的函数的实现，方法无重载的时候可以不加overload，方法有返回值，hook之后也需要返回值，否则被hook的方法无法继续执行
```
ClassName.func.overload('java.lang.String').implementation=function(param){
    //do something
    //return ...
}
```
3.3.2 无参数的函数
```
ClassName.func.overload().implementation=function(){
    //do something
}
```

### 3.3 调用函数
和Java一样，创建类实例就是调用构造函数，而在这里用$new表示一个构造函数。
```
var ClassName=Java.use("com.xxx.ClassName");
var instance = ClassName.$new();
```

实例化以后调用其他函数
```
var ClassName=Java.use("com.xxx.ClassName");
var instance = ClassName.$new();
instance.func();
```

### 3.4 字段操作
字段赋值和读取要在字段名后加.value，假设有这样的一个类：
```
package com.xxx.app;
public class Person{
    private String name;
    private int age;
}
```
写个脚本操作Person类的name字段和age字段：
```
var person_class = Java.use("com.xxx.app.Person");
//实例化Person类
var person_class_instance = person_class.$new();
//给name字段赋值
person_class_instance.name.value = "zhangsan";
//给age字段赋值
person_class_instance.age.value = 18;
//输出name字段和age字段的值
console.log("name = ",person_class_instance.name.value, "," ,"age = " ,person_class_instance.age.value);

输出：
name =  zhangsan , age =  18
```

### 3.5 类型转换
用Java.cast方法来对一个对象进行类型转换，如将variable转换成java.lang.String：
```
var StringClass=Java.use("java.lang.String");
var NewTypeClass=Java.cast(variable,StringClass);
```

### 3.6 Java.available字段
这个字段标记Java虚拟机（例如： Dalvik 或者 ART）是否已加载, 操作Java任何东西之前，要确认这个值是否为true

### 3.7 Java.perform方法
Java.perform(fn)在Javascript代码成功被附加到目标进程时调用，我们核心的代码要在里面写。格式：
```
Java.perform(function(){
//do something...
});
```

### 3.8 实例讲解
#### 3.8.1 修改返回值
业务场景：假设有以下的程序，给isExcellent方法传入两个值，通过计算，返回一个布尔值，表示是否优秀。默认情况下，它是只会显示是否优秀：false的，因为我们默认传入的数很小；

安卓代码：
```
public class MainActivity extends AppCompatActivity {
    private  String TAG="Crackme";
    private TextView textView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        textView =findViewById(R.id.tv);
        textView.setText("是否优秀："+isExcellent(46,54));
    }

    private  boolean isExcellent(int chinese, int math){
        if( chinese + math >=180){
            return true;
        }
        else{
            return false;
        }
    }

}
```
脚本代码exp1.js
```
if(Java.available){
    Java.perform(function(){
        var MainActivity = Java.use("com.xxx.name.MainActivity");
        MainActivity.isExcellent.implementation=function(){
            return true;        
        }
    });

}
```
启动服务端（注意：root权限启动）
```
adb shell 'su -c /data/local/tmp/frida-server'
```

运行目标App，注入代码
```
frida -U -l exp1.js com.xxx.name
```
按返回键返回桌面，再重新打开App,发现达到预期    
在命令行输入exit，回车，停止注入代码

### 3.9 配合Python脚本注入
编写Python代码来配合Javascript代码注入示例exp3.py
```
# -*- coding: UTF-8 -*-

import frida, sys

jscode = """
if(Java.available){
    Java.perform(function(){
        var MainActivity = Java.use("com.xxx.name.MainActivity");
        MainActivity.isExcellent.overload("int","int").implementation=function(chinese,math){
            console.log("[javascript] isExcellent be called.");
            send("isExcellent be called.");
            return this.isExcellent(95,96);      
        }
    });

}
"""

def on_message(message, data):
    if message['type'] == 'send':
        print("[*] {0}".format(message['payload']))
    else:
        print(message)
pass

# 查找USB设备并附加到目标进程
session = frida.get_usb_device().attach('com.xxx.name')

# 在目标进程里创建脚本
script = session.create_script(jscode)

# 注册消息回调
script.on('message', on_message)
print('[*] Start attach')

# 加载创建好的javascript脚本
script.load()

# 读取系统输入
sys.stdin.read()
```

运行python代码即可注入
```
python exp3.py
```


## 直接脱壳方式

https://github.com/hluwa/frida-dexdump


```
pip install frida-dexdump
```

```
frida-dexdump -U -f com.app.pkgname
```

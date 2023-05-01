# Objection用法


## Objection的安装
```
pip3 install objection 
```

## 用前准备
1、连接usb手机   
2、运行 frida-server   
3、通过 Frida-ps 找到对应的设置包名   

## 使用objection注入“设置”应用
```
objection -g 包名 explore
```

## Memory 指令
```
memory list modules  // 查看内存中加载的库
memory list exports libssl.so  // 查看库的导出函数
memory list exports libart.so --json /root/libart.json //将结果保存到json文件中
memory search --string --offsets-only //搜索内存
memory search "64 65 78 0a 30 35 00"
```

## android heap
```
android heap search instances com.android.settings.DisplaySettings //堆内存中搜索指定类的实例, 可以获取该类的实例id
android heap execute 0x2526 getPreferenceScreenResId //直接调用指定实例下的方法
android heap evaluate 0x2526 //自定义frida脚本, 执行实例的方法
root
```

## root
```
//尝试关闭app的root检测
android root disable

//尝试模拟root环境
android root simulate
```

## activities
```
可以列出app具有的所有avtivity: android hooking list activities
启动指定avtivity: android intent launch_activity [class_activity]
查看类的全部广法：android hooking list class_methods com.android.settings.DisplaySettings
```

## ui
```
//截图
android ui screenshot [image.png]

//设置FLAG_SECURE权限
android ui FLAG_SECURE false
```

## 内存漫游
```
//列出内存中所有的类
android hooking list classes

//在内存中所有已加载的类中搜索包含特定关键词的类
android hooking search classes [search_name] 

//在内存中所有已加载的方法中搜索包含特定关键词的方法
android hooking search methods [search_name] 

//直接生成hook代码
android hooking generate simple [class_name]
```

## hook 方式
```
/*
  hook指定方法, 如果有重载会hook所有重载,如果有疑问可以看
  --dump-args : 打印参数
  --dump-backtrace : 打印调用栈
  --dump-return : 打印返回值
  */
android hooking watch class_method com.xxx.xxx.methodName --dump-args --dump-backtrace --dump-return
android hooking watch class_method java.lang.StringBuilder.toString --dump-return //获取全部tostring的返回来值
android hooking watch class_method android.app.Dialog.show --dump-args --dump-backtrace --dump-return //弹窗
//hook指定类, 会打印该类下的所以调用 
android hooking watch class com.xxx.xxx 
//设置返回值(只支持bool类型) 
android hooking set return_value com.xxx.xxx.methodName false
```

## Spawn方式Hook
```
objection -g packageName explore --startup-command '[obejection_command]'
```

## activity和service操作

```
//枚举activity
android hooking list activities

//启动activity
android intent launch_activity [activity_class]

//枚举services
android hooking list services

//启动services
android intent launch_service [services_class]
```

## 任务管理器
```
//查看任务列表
jobs list

//关闭任务
jobs kill [task_id]
```

## 关闭app的ssl校验
```
android sslpinning disable
```

## 监控系统剪贴板
获取Android剪贴板服务上的句柄并每5秒轮询一次用于数据。 如果发现新数据，与之前的调查不同，则该数据将被转储到屏幕上。
```
help android  clipboard
```

## 执行命令行
```
help android shell_exec [command]
```

## Spawn方式Hook
从Objection的使用操作中我们可以发现，Obejction采用Attach附加模式进行Hook，这可能会让我们错过较早的Hook时机，可以通过如下的代码启动Objection，引号中的objection命令会在启动时就注入App。   
```
objection -g packageName explore --startup-command 'android hooking watch xxx' 
```

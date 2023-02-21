# fastboot命令

```
adb reboot bootloader（进fastboot模式）
fastboot devices（查看设备）
fastboot flashing unlock（解锁）
fastboot reboot
```

## android系统分区

```
boot              引导区,存放内核和ramdisk的分区(BIOS)
recovery       recovery分区(PE)
system         系统分区(C盘)
userdata       数据分区(D盘)
cache           缓存分区
```

## 列出fastboot设备

```
fastboot devices
```

## 重启相关
```
fastboot reboot            #重启⼿机
fastboot reboot-bootloader    #重启到bootloader模式,其实就是再次进入fastboot
```

## 擦除相关（erase）

```
fastboot erase boot    #擦除boot分区(擦了引导就没了,会卡在第一屏,)
fastboot erase recovery   #擦除recovery分区
fastboot erase system    #擦除system分区(擦了系统就没了,会卡在第二屏)
fastboot erase userdata    #擦除userdata分区(可擦,清空数据用)
fastboot erase cache    #擦除cache分区(可擦,清空数据用)
```

## 写⼊分区（flash）

```
fastboot flash boot boot.img    #写⼊boot分区
fastboot flash recovery recovery.img    写⼊recovery分
fastboot flash system system.img    #写⼊system分区
```

## 获取⼿机的全部信息
```
fastboot getvar all
```

## 其它:

```
fastboot -w reboot #清除手机中所有数据然后重启，等同于系统中的“恢复出厂设置”，或Recovery模式的“清空所有数据”操作

fastboot boot <内核镜像文件名或路径> #临时启动镜像，不会烧录和替换内核文件到存储中,类似于在PC端用U盘启动PE系统

fastboot oem device-info #输出当前BL锁状态(非MTK)

fastboot oem lks #输出当前BL锁状态(MTK)

fastboot oem reboot-recovery #重启进入Recovery模式

fastboot oem poweroff #拔掉数据线后关机

fastboot oem lock #重新上BL锁并清空所有数据(需未开启root)

fastboot oem unlock #解除BL锁并清空所有数据(小米手机必须绑定账号,主动申请解锁,等待7天,使用工具才行)

fastboot oem edl #进入高通9008模式,无需工程线或主板短接,可无视BL锁线刷
```




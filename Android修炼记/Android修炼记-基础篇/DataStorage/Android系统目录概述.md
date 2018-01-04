# Android系统目录概述

## /data：保存应用程序数据

- /data/app：**用户安装的应用apk目录**

- /data/data：保存应用使用的私有数据

 - **"/data/data/package_name** ：应用存储内部数据根目录
 - **"/data/data/package_name/files"** ： 应用存储内部数据文件目录，Context.getFilesDir() ，Context.openFileOutput() 获取的目录，应用安装目录下
 - **"/data/data/package_name/cache"** ： 应用存储内部缓存数据目录，Context.getCacheDir()  获取的目录，应用安装目录下，系统会自动在内存不足或目录大小达到特定数值时自动清理。
 - **"/data/data/package_name/databases"** ： 应用存储内部数据库目录
 - **"/data/data/package_name/shared_prefs"** ： 应用存储内部键值对数据目录，Context.getSharedPreferences() 建立的preferences文件（xml）存放目录。
 
<br>

- /data/system：系统的配置信息，注册表文件

- /data/anr：anr异常的记录信息，方便开发人员定位ARN异常,通过应用包名定位错误信息

## /dev：devices的缩写，硬件设备驱动

存放设备所对应的文件（注：Android所有的设备都是以文件的形式体现）

## /mnt：mount的缩写

挂载外接设备：sdcard，u盘

以下几个目录都是指向同一个目录的不同链接。

- /storage/emulated/legacy
- /storage/sdcard0
- /sdcard
- /mnt/sdcard
- /mnt
 - **"/mnt/Android/data/package_name** ：应用存储外部数据目录,，应用卸载时自动删除。
 - **"/mnt/Android/data/package_name/cache** ： Context.getExternalCacheDir()获取的缓存目录。设置->应用->具体应用详情-> 清除缓存  操作对象就是这个目录。
 - **"/mnt/Android/data/package_name/files** ：Context.getExternalFilesDir()获取的目录。设置->应用->具体应用详情-> 清除数据  操作对象就是这个目录。



## /proc：硬件配置，进程状态信息

 - cpuinfo、meminfo
 - 虚拟的文件系统，即使文件大小是0，但是还是有内容的
 
## /sbin：system bin系统级可执行程序

- 系统重要的二进制执行文件
- adbd：服务器的adb进程 <->adb客户端 （socket）
- adb connect 192.168.1.101 链接一个局域网Android设备

##/sys：Android的模块。组件、设备信息

##/system/：

- /system/app：存放系统自带的应用，默认不能删除
- /system/bin：Android中可执行的linux指令文件（ELF）
- /system/etc：系统配置文件如host：主机名和ip地址的映射
- /system/fonts：Android中自带的字体
- /system/framework：存放谷歌提供的java api
- /system/lib：核心功能的类库，C/C++文件
- /system/media/audio：存放Android的音效文件
- /system/tts：语音发声引擎，默认不支持中文
- /system/usr：用户设备的配置信息，键盘编码和按键编码的映射
- /system/xbin：是专为开发人员准备的二进制指令


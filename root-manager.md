# 解锁bootloader之后，如何获取root权限？

## 一些操作前的准备和注意事项

**一般情况下**，成功解锁bootloader之后，系统会**格式化用户分区**，所以在解锁bootloader之前，需要提前备份好你**重要**的数据.

你需要准备以下这些物品：  
_通常这些物品在你第一步解锁bootloader的时候就应该准备好了_

- 一台电脑，上面有adb和fastboot工具和相对应的驱动程序
- 一根可以将你的安卓设备和电脑连接的数据线
- 一部分可用的磁盘空间(通常不多于15GB)
- 你的安卓设备的对应系统版本号的全量包
- 一名可以读完并正确按照以下步骤操作的人类(通常是你)
- 可用的root管理器(如magisk,kernelsu,apatch,~~或者supersu(不推荐)~~)

---

## 1.获取当前系统在使用的插槽

Android10之后，大部分安卓设备使用了a/b或者avb分区，而非传统的a-only，这项功能使得系统可以无缝衔接进行更新，如果要获取root权限则需要知道当前系统所在的插槽为a还是b.  
在fastboot模式下，使用该指令来获取当前系统所在插槽：  
_注意：此时不要将你的安卓设备启动至系统_

```bash
fastboot getvar current-slot
```

如果成功的话，输出可能是这样的：

```
current-slot:a
Finished.Total time:0.001s

```

这说明了当前系统所使用的槽位为`a`.

或者是：

```
current-slot:b
Finished.Total time:0.001s

```

这说明了当前系统所使用的槽位为`b`.

## 2.安装root管理器

1. 重启系统

你可以采用以下任意一种方式来重启：

- 在fastboot模式下，使用fastboot指令

```bash
fastboot reboot
```

- 使用音量上下键在fastboot模式界面操作，找到'restart'选项，并按下电源键确认.  
  ![fastboot界面图片]()

- 长按电源键重启.

2. 安装root管理器
   > 一般情况下，解锁bootloader后系统会格式化用户分区，因此重启系统后需要再次进行出厂设置.

重启并进入桌面后，将已经准备好的root管理器安装包(为apk格式)安装在你的设备上.

安装完成后进入root管理器，此时会有这样的界面：

![magisk界面图片]()
![kernelsu界面图片]()

## 3.获取并修补镜像文件

### 如果你使用的是kernelsu(kernelsu/sukisu ultra/kernelsu next)的LKM模式或者使用magisk(magisk/magisk delta)：

如果你的设备出厂系统是在Android 13之后的，那么需要获取并修补的分区为`init_boot`分区。
反之，则需要获取并修补`boot`分区。

### 如果你使用的是kernelsu(kernelsu/sukisu ultra/kernelsu next)的GKI模式：

那么需要获取并修补的分区始终为`boot`分区。

那么，该如何获得你所需要的分区镜像文件呢？

### 使用super payload dumper对全量包内的payload.bin进行解包来获得文件

1. 下载super payload dumper,并在本地进行解压。
   > super payload dumper是大侠阿木对5ec1cff开发的payload dumper进行二次开发的payload.bin解包工具，相比原版来说操作更加简单。  
   > 注意！这里强烈建议将解压后的文件放在一个新的空白文件夹内！

解压之后，会获得两个文件：
![super payload dumper解压文件图]()

`payload dumper.exe`
和`unpack.exe`

2. 重命名`unpack.exe`为你要解包的分区名。

例如：如果你要获取`init_boot`分区，那么就将`unpack.exe`重命名为`init_boot.exe`。

3. 解包。

将之前准备好的全量包拖到重命名后的`unpack.exe`上，随后等待解包完成(根据电脑配置的不同，解包时间有所不同)。

4. 获得分区镜像文件。

将提取出的文件(可能在output文件夹里面或者是super payload dumper工具所在的文件夹)复制到一个你能找到并可以使用的地方。

### 使用刷机匣工具来获得文件

条件：支持高通9008刷机或者支持联发科深度刷机
(待完善)

### 使用DSU Sideloader来获取文件

1. 安装treble check软件，打开并查看设备是否支持DSU和PT。

2. 根据你的设备的Android系统版本，来确认你所需要的GSI包。

3. 


# pixel 3xl 安卓与内核代码下载、编译与烧入全流程（含安卓源码部分编译）

# 目录

- [环境搭建](#环境搭建)
- [安卓源码下载](#安卓源码下载)
- [安卓源码编译](#安卓源码编译)
- [安卓源码刷机](#安卓源码刷机)
- [内核源码构建与烧入](#内核源码构建与烧入)
- [部分编译安卓源码（framework）](#部分编译安卓源码)


# 环境搭建

建议：ubuntu20.04最好，ubuntu18.04应该也可

建议：最大磁盘大小350GB以上，内存16GB以上，4个CPU内核以上

# 安卓源码下载

## 一、准备下载环境

### 1、安装Python 3.9

- sudo apt update
- sudo apt install software-properties-common
- sudo add-apt-repository ppa:deadsnakes/ppa
- sudo apt install python3.9
- sudo apt-get install python

### 2、安装git

- sudo apt-get upgrade
- sudo apt-get install git
- git config --global user.email "xxx@gmail.com"
- git config --global user.name "xxx"
（xxx 填自己的信息）

### 3、安装curl

- sudo apt-get install curl

### 4、配置环境变量 安装repo

- mkdir ~/bin
- PATH=~/bin:$PATH
- curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
- chmod a+x ~/bin/repo

打开主目录bin文件夹下的repo

将`REPO_URL = 'https://gerrit.googlesource.com/git-repo'`

改为`REPO_URL = 'https://mirrors.tuna.tsinghua.edu.cn/git/git-repo'`

## 二、下载源代码

### 1、创建目录

- mkdir android12.0.0
- cd android12.0.0

### 2、初始化仓库

- repo init -u https://mirrors.tuna.tsinghua.edu.cn/git/AOSP/platform/manifest

切换分支

- repo init -u https://mirrors.tuna.tsinghua.edu.cn/git/AOSP/platform/manifest -b android-12.0.0_r1
  （要和自己手机里的版本进行对应，位置在 Android 版本中的版本号。[进入该网页查看对应路径](https://source.android.com/setup/start/build-numbers#source-code-tags-and-builds)，然后修改`android-12.0.0_r1`）

### 3、同步

- repo sync

# 安卓源码编译

## 一、准备编译环境

### 1、安装jdk

- sudo apt-get update
- sudo apt-get install openjdk-8-jdk

### 2、使用 ubuntu 14+，需要安装以下依赖包

- sudo apt-get install git-core gnupg flex bison gperf build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev ccache libgl1-mesa-dev libxml2-utils xsltproc unzip

### 3、安装libncurses5

- sudo apt install libncurses5

### 4、构建编译环境依赖

- sudo apt-get install libx11-dev:i386 libreadline6-dev:i386 libgl1-mesa-dev g++-multilib
- sudo apt-get install -y git flex bison gperf build-essential libncurses5-dev:i386
- sudo apt-get install tofrodos python-markdown libxml2-utils xsltproc zlib1g-dev:i386
- sudo apt-get install dpkg-dev libsdl1.2-dev libesd0-dev
- sudo apt-get install git-core gnupg flex bison gperf build-essential
- sudo apt-get install zip curl zlib1g-dev gcc-multilib g++-multilib
- sudo apt-get install libc6-dev-i386
- sudo apt-get install lib32ncurses5-dev x11proto-core-dev libx11-dev
- sudo apt-get install libgl1-mesa-dev libxml2-utils xsltproc unzip m4
- sudo apt-get install lib32z-dev ccache libncurses5

## 二、下载编译驱动

[进入该网页找对所下载安卓源代码版本所对应的驱动，之前的版本和此处的驱动要与手机对应](https://developers.google.cn/android/drivers#crosshatchsp1a.210812.015)

点击链接中对应版本的`Link`下载机器对应的驱动编译脚本文件并解压，得到`extract-google_devices-crosshatch.sh`和`extract-qcom-crosshatch.sh`（不同手机对应不同文件），放到源码的根目录执行（会让输入 I ACCEPT,回车别按的太快 之后ctrl+c 跳过），执行后会得到vender目录

- ./extract-qcom-crosshatch.sh（自行修改为手机对应驱动编译脚本文件）
- ./extract-google_devices-crosshatch.sh（自行修改为手机对应驱动编译脚本文件）

## 三、开始编译

- cd#（源代码文件名）
- source build/envsetup.sh
- make clobber
- lunch
- 输入#后的数字，如 25（[进入该网页找到手机对应的build配置，然后修改`25`](https://source.android.com/setup/build/running#selecting-device-build)）
- （make update-api ，该步可以不执行，如果在第一次编译之前就修改过安卓代码，需要进行该步）
- make

# 安卓源码刷机

## 一、准备刷机环境

### 1、fastboot 安装

- sudo apt-get install android-tools-fastboot
```
💥 报错

E: Invalid operation install android-tools-fastboot

💥 解决
- sudo apt-get update 更新系统
- sudo apt-get upgrade 更新软件包
- sudo apt-get install android-tools-adb android-tools-fastboot 安装android-tools-adb和android-tools-fastboot
```

### 2、安装kvm

- sudo apt-get install qemu-kvm

## 二、开始刷机

### 1、进入reboot-bootloader模式

- adb reboot bootloader
- fastboot reboot-bootloader

### 2、配置ANDROID_PRODUCT_OUT环境

- export ANDROID_PRODUCT_OUT=/home/xxx/android12.0.0/out/target/product/crosshatch
（/home/xxx/android12.0.0/是编译过后的源代码目录 注意要用自己的路径替代）

### 3、刷机

- fastboot flashall -w

```
💥 报错

FAILED (remote: Partition should be flashed in fastbootd)

💥 解决

下载最新的SDK工具包：https://developer.android.com/studio/releases/platform-tools.html?hl=zh-cn，解压工具包

在 /usr/bin 目录下删除 adb 和 fastboot

- sudo rm adb
- sudo rm fastboot

把工具包中的 adb 和 fastboot 复制到 /usr/bin

- sudo cp adb /usr/bin
- sudo cp fastboot /usr/bin

赋予权限

- su 进入root
- chmod 777 adb
- chmod 777 fastboot

重新从 1. 进入reboot-bootloader模式 开始执行命令
```
# 内核源码构建与烧入

## 一、下载相应分支的源代码

[在该网页中找到自己手机对应的内核版本](https://source.android.com/docs/setup/build/building-kernels?hl=zh-cn)

- mkdir android-kernel && cd android-kernel
- repo init -u https://android.googlesource.com/kernel/manifest -b android-msm-crosshatch-4.9-android12
  （将 `android-msm-crosshatch-4.9-android12` 替换为自己手机的内核版本）
```
💥 报错

manifests:
fatal: unable to access 'https://android.googlesource.com/kernel/manifest/': Failed to connect to [android.googlesource.com](http://android.googlesource.com/) port 443: Connection timed out
manifests: sleeping 4.0 seconds before retrying

manifests:
fatal: unable to access 'https://android.googlesource.com/kernel/manifest/': Failed to connect to [android.googlesource.com](http://android.googlesource.com/) port 443: Connection timed out
fatal: cannot obtain manifest https://android.googlesource.com/kernel/manifest

💥 解决

属于连接超时，在 Ubuntu 中挂梯子，然后在终端中输入：

git config --global https.proxy http://127.0.0.1:{VPN端口号}

```
- repo sync

## 二、构建内核（编译）

- build/build.sh

```
💥 报错

private/msm-google/scripts/extract-cert.c:21:10: fatal error: 'openssl/bio.h' file not found

#include <openssl/bio.h>
               ^~~~~~~~~~

💥 解决

- sudo apt-get install libssl-dev 安装libssl-dev
```

## 三、运行内核（烧入）

### 方案1：永久刷入手机

在编译好的安卓源码目录下有一个out文件夹，将目录out/target/product/crosshatch目录下的*.img和kernel删除

- rm *.img kernel

然后备份安卓源码的目录device/google/crosshatch-kernel下的Image.lz4，用内核源码中的Image.lz4进行替换

回到安卓目录后进行：

- source build/envsetup.sh
- make

### 方案2（推荐）：快速刷入手机，但手机重启后恢复原样
进入 out/android-msm-pixel-4.9/private/msm-google/arch/arm64/boot 目录
- adb reboot bootloader
- fastboot boot Image.lz4-dtb


# 部分编译安卓源码


## 1. framework-minus-apex

### 编译：

- cd#（安卓源代码文件名）
- source build/envsetup.sh
- lunch
- 输入#后的数字，如 25（[进入该网页找到手机对应的build配置，然后修改`25`](https://source.android.com/setup/build/running#selecting-device-build)）
- make framework-minus-apex
- make

编译完后观察framework.jar是否发生了更新
（framework.jar的路径：out/target/product/crosshatch/system/framework）

### 刷机：

1. adb root
2. adb remount （将'/system' 部分置于可写入的模式）
3. adb reboot
4. adb root
5. adb remount
6. adb shell
7. cd system/framework

### 删除下面三个文件（没有就不删）:

8. rm -r oat
9. rm -r arm
10. rm -r arm64
11. exit（退出adbshell模式）
12. adb push /home/xxx/android12.0.0/out/target/product/crosshatch/system/framework/framework.jar /system/framework
（将刚刚编译新生成的framework.jar导入手机中，framework.jar的路径需要修改成自己的路径，将 /home/xxx/android12.0.0/ 换成自己的路径）

### **下面两步重启zygote：**

13. adb shell stop
14. adb shell start

### **重启手机**

15. adb reboot

## 2. libhwui

### 编译：

- cd#（安卓源代码文件名）
- source build/envsetup.sh
- lunch
- 输入#后的数字，如 25（[进入该网页找到手机对应的build配置，然后修改`25`](https://source.android.com/setup/build/running#selecting-device-build)）
- make libhwui

译完后会更新
/out/target/product/flame/system/lib
/out/target/product/flame/system/lib64
中的libhwui.so 库文件

### 刷机（刷入 .so 库文件）：

1. adb root
2. adb remount （将'/system' 部分置于可写入的模式）
3. adb push /home/xxx/android12.0.0/out/target/product/crosshatch/system/lib/libhwui.so /system/lib
adb push libhwui.so /system/lib
（将 /home/xxx/android12.0.0/ 换成自己的路径）
4. adb push libhwui.so /system/lib
5. adb push libhwui.so /system/lib64
6. adb push /home/cyrusandy/android12.0.0/out/target/product/crosshatch/system/lib64/libhwui.so /system/lib64
（将路径修改成自己的，分别将对应.so文件刷入对应lib文件夹）

### **下面两步重启zygote：**

7. adb shell stop
8. adb shell start

### **重启手机**

9. adb reboot

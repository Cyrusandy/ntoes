# pixel 3xl全流程

# 环境搭建

建议：ubuntu18.04，ubuntu20.04也可

建议：最大磁盘大小300GB以上，内存16GB以上，4个CPU内核以上

# 安卓源码下载

## 一、**准备下载环境**

### 1、**安装Python 3.9**

- sudo apt update
- sudo apt install software-properties-common
- sudo add-apt-repository ppa:deadsnakes/ppa
- sudo apt install python3.9
- sudo apt-get install python

### 2、**安装git**

- sudo apt-get upgrade
- sudo apt-get install git
- git config --global user.email "ccyrusandy@gmail.com"
- git config --global user.name "ccyrusandy"

### **3、安装curl**

- sudo apt-get install curl

### 4、**配置环境变量 安装repo**

- mkdir ~/bin
- PATH=~/bin:$PATH
- curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
- chmod a+x ~/bin/repo

打开主目录bin文件夹下的repo

将`REPO_URL = 'https://gerrit.googlesource.com/git-repo'`

改为`REPO_URL = 'https://mirrors.tuna.tsinghua.edu.cn/git/git-repo'`

## 二、**下载源代码**

### **1、创建目录**

- mkdir android12.0.0
- cd android12.0.0

### **2、初始化仓库**

- repo init -u https://mirrors.tuna.tsinghua.edu.cn/git/AOSP/platform/manifest

切换分支

- repo init -u [https://mirrors.tuna.tsinghua.edu.cn/git/AOSP/platform/manifest -b android-12.0.0_r1](https://mirrors.tuna.tsinghua.edu.cn/git/AOSP/platform/manifest%20-b%20android-12.0.0_r11)（要和自己手机里的版本进行对应）

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f1673d11-9b43-4bf6-a1f9-62223d49a948/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/314a34df-8672-41b3-a9d8-68e1ed9de2ac/Untitled.png)

可以登录以下网页查看对应路径

https://source.android.com/setup/start/build-numbers#source-code-tags-and-builds

### **3、同步**

- repo sync

# 安卓源码编译

## 一、**准备编译环境**

### **1、安装jdk**

- sudo apt-get update
- sudo apt-get install openjdk-8-jdk

### 2、**使用 ubuntu 14+，需要安装以下依赖包**

- sudo apt-get install git-core gnupg flex bison gperf build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev ccache libgl1-mesa-dev libxml2-utils xsltproc unzip

### **3、安装libncurses5**

- sudo apt install libncurses5

### **4、构建编译环境依赖**

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

## 二、**下载编译驱动**

[https://developers.google.cn/android/drivers#flamesp1a.210812.016.a1](https://developers.google.cn/android/drivers#crosshatchsp1a.210812.015)

去以上网站找对所下载安卓源代码版本所对应的驱动，之前的版本和此处的驱动要与手机对应

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f1673d11-9b43-4bf6-a1f9-62223d49a948/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/314a34df-8672-41b3-a9d8-68e1ed9de2ac/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e519dcfa-f78a-44b0-ad56-6bd778ed27ca/Untitled.png)

点击里一个Link下载机器对应的驱动编译脚本文件并解压，得到`extract-google_devices-crosshatch.sh`和`extract-qcom-crosshatch.sh`（不同手机对应不同文件），放到源码的根目录执行（会让输入 I ACCEPT,回车别按的太快 之后ctrl+c 跳过），执行后会得到vender目录

- ./extract-qcom-crosshatch.sh（自行修改为手机对应驱动编译脚本文件）
- ./extract-google_devices-crosshatch.sh（自行修改为手机对应驱动编译脚本文件）

## 三、**开始编译**

- cd#（源代码文件名）
- source build/envsetup.sh
- make clobber
- lunch
- 输入#后的数字 25（通过网址https://source.android.com/setup/build/running#selecting-device-build找到手机对应的build配置）

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/47c38a6a-a9a7-43b7-8c0c-f0eea6fa16d2/Untitled.png)

- （make update-api）
- make

# 刷机

## 一、**准备刷机环境**

### **1、fastboot 安装**

- sudo apt-get install android-tools-fastboot

### **2、安装kvm**

- sudo apt-get install qemu-kvm

## 二、**开始刷机**

### **1、进入reboot-bootloader模式**

- adb reboot bootloader
- fastboot reboot-bootloader

### 2、**配置ANDROID_PRODUCT_OUT环境**

- export ANDROID_PRODUCT_OUT=/home/cyrusandy/android12.0.0/out/target/product/crosshatch

（蓝色部分是编译过后的源代码目录 注意要用自己的路径替代）

### **3、刷机**

- fastboot flashall -w

# 内核源码构建

## 一、**下载相应分支的源代码**

[构建内核  |  Android 开源项目  |  Android Open Source Project](https://source.android.com/docs/setup/build/building-kernels?hl=zh-cn)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4cbdb5fe-10f1-43d7-ac5d-1955a1e6f426/Untitled.png)

- mkdir android-kernel && cd android-kernel
- repo init -u https://android.googlesource.com/kernel/manifest -b android-msm-crosshatch-4.9-android12
- repo sync

## 二、**构建内核（编译）**

- build/build.sh

<aside>
💥 报错

/home/cyrusandy/android-kernel/private/msm-google/scripts/extract-cert.c:21:10: fatal error: 'openssl/bio.h' file not found
#include <openssl/bio.h>
               ^~~~~~~~~~

[fatal error: openssl/bio.h: No such file or directory 解决方案](https://blog.csdn.net/qq_41533289/article/details/82985788)

</aside>

## 三、**运行内核（烧入）**

### 方案1：永久刷入手机

在编译好的安卓源码目录下有一个out文件夹，将目录out/target/product/crosshatch目录下的*.img和kernel删除

- rm *.img kernel

然后备份安卓源码的目录device/google/crosshatch-kernel下的Image.lz4，用内核源码中的Image.lz4进行替换

回到安卓目录后进行：

- source build/envsetup.sh
- make

### 方案2：快速刷入手机，但手机重启后恢复原样

- adb reboot bootloader
- fastboot boot Image.lz4-dtb

## 四、脚本（本人版本）

可以在根目录终端中输入命令`vi build.sh`之后编写脚本：

```cpp
cd android-kernel/
build/build.sh

cd out/android-msm-pixel-4.9/private/msm-google/arch/arm64/boot
adb reboot bootloader
fastboot boot Image.lz4-dtb
```

# 部分编译安卓源码（framework）

### 编译：

- cd#（安卓源代码文件名）
- source build/envsetup.sh
- lunch
- 输入#后的数字，如25（通过网址https://source.android.com/setup/build/running#selecting-device-build找到手机对应的build配置）

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/47c38a6a-a9a7-43b7-8c0c-f0eea6fa16d2/Untitled.png)

- make framework-minus-apex

编译完后观察framework.jar是否发生了更新
（framework.jar的路径：out/target/product/crosshatch/system/framework）

### 刷机：

1. adb root
2. adb remount （将'/system' 部分置于可写入的模式）

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/76861625-6e7d-4465-8187-98ed15b37da4/Untitled.png)

（执行上述命令的路径随意）

1. adb reboot
2. adb root
3. adb remount
4. adb shell
5. cd system/framework
6. ls（输出手机在文件路径system/framework下的文件目录）

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/59eb7a9a-1b11-4da7-8174-4899bd99b7fc/Untitled.png)

### 删除下面三个文件:

1. rm -r oat
2. rm -r arm
3. rm -r arm64
4. ls（输出删除结果）

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c33fe72b-52da-4543-82b5-306427a28ee8/Untitled.png)

1. exit（退出adbshell模式）
2. adb push /home/cyrusandy/android12.0.0/out/target/product/crosshatch/system/framework/framework.jar /system/framework
（将刚刚编译新生成的framework.jar导入手机中，framework.jar的路径需要修改成自己的路径，将蓝色部分换成自己的路径期中0.0是源代码的根目录）

### **下面两步重启zygote：**

1. adb shell stop
2. adb shell start

### **重启手机**

1. adb reboot

### 部分编译与刷机步骤结束

# 编译 android 5.1 

## 代码下载

- 中科大：<https://lug.ustc.edu.cn/wiki/mirrors/help/aosp>
- 清华大学：<https://mirrors.tuna.tsinghua.edu.cn/help/AOSP/>
- 东软：<http://mirrors.neusoft.edu.cn/more.we#android>

东软的帮助里边只有SDK代理的配置，而没有AOSP的配置。 

清华大学的同步源使用的是https协议，限制速度了，同步比较慢。 

对比起来，中科大的源，速度较快，能达到5M+/S左右。下载源代码大概花了1个多小时，比Google源快太多了。

使用清华大学的镜像服务器： https://aosp.tuna.tsinghua.edu.cn/

安装 repo 工具实现下载：      https://github.com/aosp-mirror/tools_repo.git

```bash
 repo init -u https://aosp.tuna.tsinghua.edu.cn/platform/manifest -b android-5.1.0_r5
```



## 环境搭建

### 安装 JDK

android 5.1 需要 JDK 7， 但是 Ubuntu 16.04 已经没有 JDK7 的安装源，需要手动添加安装：

Ubuntu16.04的安装源已经默认没有openjdk7了，所以要自己手动添加仓库，如下：

1. oracle openjdk ppa source
  sudo add-apt-repository ppa:openjdk-r/ppa
  sudo apt-get update
  sudo apt-get install openjdk-7-jdk  // OpenJdk 7安装：


2. oracle java jdk ppa source
  sudo add-apt-repository ppa:webupd8team/java
  sudo apt-get update

JDK6 ：
sudo apt-get install oracle-java6-installer

JDK 7：
sudo apt-get install oracle-java7-installer

JDK 8:
sudo apt-get install oracle-java8-installer


如果安装成功之后还是不能用可能不有多个版本，选的不对

sudo update-alternatives --config java
sudo update-alternatives --config javac
选出正确的版本 

## 最小编译系统

要编译 host 断的程序，系统自带编译工具 host gcc，只需要下载基本的构建工具，以及 host 程序依赖库：


```bash
repo sync -c "platform/build" 
repo sync -c "bionic"
repo sync -c "platform/build/soong"
repo sync -c "platform/build/blueprint"
repo sync -c "platform/prebuilts/go/linux-x86"
repo sync -c "platform/prebuilts/build-tools"
repo sync -c "platform/external/clang"
repo sync -c "platform/external/llvm"
```

基础类库：

```bash
repo sync -c "external/stlport"
repo sync -c "external/libcxx"
```

系统依赖：

```bash
sudo apt-get install libx32gcc-4.8-dev g++-multilib build-essential
```

下载完成基本的构建工具就能够编译 host 端 bin 了，比如 `system/core/mkbootimg` 

```bash
 repo sync -c "system/core" 
 source build/envsetup.sh
 mmma system/core/mkbootimg/
```

## 编译 device 程序

需要下载交叉编译工具：

```bash
repo sync -c  "prebuilts/gcc/linux-x86/arm/arm-linux-androideabi-4.8"
```

`glibc`

```bash
 repo sync -c "prebuilts/gcc/linux-x86/host/x86_64-linux-glibc2.11-4.8"
```

现在可以编译不需要依赖的一些基础类库：

```bash
cd/external/stlport
$ mm
```

使用 mm 命令可以不用加载全局 Android.mk ，提高编译速度

编译其他需要的库就按照需要下载对应的代码，进行编译。
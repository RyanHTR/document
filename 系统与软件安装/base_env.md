本文主要介绍基础环境的配置，具体有Ubuntu系统安装、nvidia驱动、CUDA以cudnn安装

# 1.ubuntu安装
## 1.1 安装步骤
考虑到装系统较为基础，在此不做太多展开。
由于新显卡要求CUDA版本至少为8.0，故建议安装16.04以上的版本。另外建议安装时语言选择为英语，这样更方便组内交流遇到的问题。
## 1.2 可能遇到的坑
### 进入安装界面黑屏
解决方法为：
在进入安装界面前的grub界面将光标移至'Install ubuntu'，按'e'，
进入命令行后去掉'--'，添加'nomodeset'，进入安装界面即可。
### 进入系统界面黑屏
解决方法为：
开机后按住shift进入grub界面，然后按'e'进入启动指令编辑界面。找到'quite splash'一项，在后面加上'nomodeset'，然后按F10启动系统即可。


# 2.驱动安装
本文建议使用软件源进行驱动的安装。步骤如下：
### 添加ppa源并更新apt源
在terminal中输入
```
sudo add-apt-repository ppa:graphics-drivers/ppa
```
然后输入
```
sudo apt update
```
### 在Software & Updates中安装显卡驱动
打开"System Settings"，在System一栏里打开"Software & Updates"。再打开"Additional Drivers"选项卡。
在NVIDIA coporation下选择驱动后点击右下角"Apply Changes"。

# 3.cuda安装
## 3.1下载安装包
在https://developer.nvidia.com/cuda-downloads 中下载安装包，建议使用runfile进行安装。后文讲基于该方式进行说明。
## 3.2执行run文件
在run文件所在文件夹下使用terminal输入
```
sudo sh cuda_8.0.61_375.26_linux.run
```
注意因为之前一步已经安装了驱动，故不需要再安装run文件自带的驱动。其他使用默认选项即可。
### 3.3添加路径
使用terminal输入
```
sudo gedit /etc/profile
```
在打开的文件末尾添加：
```
export PATH=/usr/local/cuda-8.0/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64$LD_LIBRARY_PATH
```
然后重启电脑。
### 3.4检查是否安装成功
使用terminal输入
```
nvcc –V
```
若输出CUDA的版本信息则说明CUDA已经成功安装。

# 4.cudnn安装
## 4.1下载安装包
在https://developer.nvidia.com/cudnn 中下载安装包，中间可能需要注册账号并填写调查表。填写完成后下载所需版本的cudnn压缩包。下面以v5.1为例进行讲解。
## 4.2解压并复制相应文件
将压缩包解压后打开terminal，输入：
```
cd cuda/include
sudo cp *.h /usr/local/cuda/include/
```
然后输入：
```
cd ../lib64
sudo cp libcudnn* /usr/local/cuda/lib64/
```
最后输入：
```
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```

需要注意cudnn并不向前兼容，所以在更新cudnn时需要将以前安装的.h和.so文件删除后再执行以上安装操作。

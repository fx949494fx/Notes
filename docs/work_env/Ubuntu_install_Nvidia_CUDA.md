### 准备工作
1. 如果是核显+独显，先禁用核显。
2. 如果可以，先进入管理员模式。 `su`。 否则以下命令前都需加`sudo`。
3. 如果之前安装了NVIDIA驱动，先删除。 `apt-get --purge remove nvidia-*`
4. 补充禁用黑名单,`vim /etc/modprobe.d/blacklist.conf`
```
# 写入以下内容：
blacklist vga16fb
blacklist nouveau
blacklist rivafb
blacklist nvidiafb
blacklist rivatv
```
5. 检查是否已禁用，`lsmod | grep nouveau`。应没有输出。
6. 如果仍有输出，继续`vim /etc/modprobe.d/blacklist.conf`，补充：

```
# 写入以下内容：
blacklist lbm-nouveau
options nouveau modeset=0
alias nouveau off
alias lbm-nouveau off
```
退出编辑器后，更新文件。
```Linux
update-initramfs -u
reboot
lsmod | grep nouveau
```
### 安装NVIDIA驱动
1. 检查合适的驱动程序，推荐选择‘recommended’版本。`ubuntu-drivers devices`
2. 安装驱动程序，`apt install nvidia-driver-XXX`。其中XXX为-后的版本号,如果仅作为服务器使用，可以安装"-server"版本。
3. 检查是否安装成功，`nvidia-smi`

### 安装CUDA
1. 在NVIDIA官网的[CUDA页面](https://developer.nvidia.com/cuda-downloads)下载安装包。根据自己的系统选择相应的安装包，使用'runfile'下载到本地安装更简单。
2. 根据页面提示的代码，复制到本地运行。
3. 如果还没安装NVIDIA驱动，在安装选择界面可以点选，同时安装NVIDIA驱动和CUDA。
4. 安装结束以后，编辑'.bashrc'文件`vim ~/.bashrc`。输入：
```
# XXX为版本号，在下载的文件名中有体现。如'cudua_12.4.1_550.54.15_linux.run'的版本号为'12.4'。
export PATH=/usr/local/cuda-XXX/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-XXX/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```
5. 退出编辑器后，`source ~/.bashrc`更新文件。
6. `nvcc -V`检查是否安装成功。

### 安装cuDNN
1. 在NVIDIA官网的[cuDNN页面](https://developer.nvidia.com/cudnn-downloads)下载安装包。根据自己的系统选择相应的安装包。
2. 根据页面提示的代码，复制到本地运行。
3. 安装结束后检查是否安装成功`cat /usr/include/cudnn_version.h | grep CUDNN_MAJOR -A 2`。注意：'/usr/include/cudnn_version.h'的位置可能有所不同，根据安装信息提示的地址修改，或者使用`find / -name "cudnn_version.h`查询安装地址。

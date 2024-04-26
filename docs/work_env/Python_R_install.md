### 在Ubuntu下安装Python
1. 使用Anaconda作为Python安装的整体方案，在其[官网下载页面](https://www.anaconda.com/download/success)选择相应的安装包下载.
2. 复制安装包链接，在本地使用`wget`下载，如`wget https://repo.anaconda.com/archive/Anaconda3-2024.02-1-Linux-x86_64.sh`下载。
3. `sudo bash Anaconda3-2024.02-1-Linux-x86_64.sh`运行安装程序，一路回车和'yes'。
4. 在选择安装目录的部分，可以键入`/usr/local/anaconda3`选择指定安装路径。
5. 安装完成后`source ~/.bashrc`更新。
6. 使用`python`检查是否安装成功。进入Python命令行后可使用`exit()`退出。

### 在Ubuntu下安装R
1. 根据[R官网](https://mirrors.tuna.tsinghua.edu.cn/CRAN/)介绍的方法安装R。
2. 安装结束后输入`R`检查是否安装成功。

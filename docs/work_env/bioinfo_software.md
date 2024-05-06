# 在Ubuntu上安装Guppy
1. 联系ONT经销商获取相应版本的Guppy。
2. 新建文件夹`mkdir /usr/local/guppy`
3. `sudo tar -xzf ont-guppy_6.5.7_linux64.tar.gz -C /usr/local/guppy`解压到工作文件夹。
4. `vim ~/.bashrc`,在文件末尾加上`export PATH=/usr/local/cuda-XXX/bin${PATH:+:${PATH}}`。
5. `suorce ~/.bashrc`
6. `guppy_basecaller -h`检查是否安装安装成功。

# 在Ubuntu上安装Plink
1. 在[Plink官网](https://www.cog-genomics.org/plink/1.9/)选择对应版本，复制下载链接。
2. 首先下载压缩包`wget https://s3.amazonaws.com/plink1-assets/plink_linux_x86_64_20231211.zip`。
3. 解压`unzip plink_linux_x86_64_20231211.zip -d /usr/local`。
4. `vim ~/.bashrc`,在文件末尾加上`export PATH=/usr/local/plink`。
5. `suorce ~/.bashrc`
6. `plink`检查是否安装安装成功。

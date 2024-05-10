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
3. 新建文件夹`sudo mkdir /usr/local/plink`。当然也可以建立在登录账号的目录下，使用起来更方便。
4. 解压`sudo unzip plink_linux_x86_64_20231211.zip -d /usr/local/plink/`。
5. `vim ~/.bashrc`,在文件末尾加上`export PATH=$PATH:/usr/local/plink/`。
6. `suorce ~/.bashrc`
7. `plink --version`检查是否安装安装成功。

# 在Ubuntu上安装Seqkit
1. 访问SeqKit的[官方网站下载页面](https://bioinf.shenwei.me/seqkit/download/)。
2. 选择适合操作系统的压缩可执行文件进行下载。
3. 下载后，使用解压命令（如tar -zxvf seqkit_linux_amd64.tar.gz）解压文件，并确保解压后的seqkit程序具有执行权限。
4. 复制到目标文件夹`sudo cp seqkit /usr/local/bin/`
5. `seqkit`检查是否安装安装成功

已经安装了Anaconda或Miniconda，可以使用conda包管理器来安装SeqKit：
`conda install -c bioconda seqkit`
   

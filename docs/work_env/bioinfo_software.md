# 在Ubuntu下安装Guppy
1. 联系ONT经销商获取相应版本的Guppy。
2. `sudo tar -xzf ont-guppy_6.5.7_linux64.tar.gz -C /usr/local/guppy`解压到工作文件夹。
3. `vim ~/.bashrc`,在文件末尾加上`export PATH=/usr/local/cuda-XXX/bin${PATH:+:${PATH}}`。
4. `suorce ~/.bashrc`
5. `guppy_basecaller -h`检查是否安装安装成功。

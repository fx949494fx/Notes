# ONT的fast5转fastq
1. 使用官方[guppy](work_env/bioinfo_software.md)进行数据转换。
2. 使用`guppy_basecaller -h`查看'guppy_basecaller'的帮助文档。
3. 可以使用配置文件'config file'的方法进行转换：
   `guppy_basecaller -i <input path> -s <save path> -c <config file> [options]`
  - `<input path>` 为输入文件夹路径
  - `<save path>` 为输出文件夹路径
  - `<config file>` 为配置文件，如：'dna_r9.4.1_450bps_sup.cfg'。
    cfg文件命名规则：  
    `<strand_type>_<pore_type>_<enzyme_type>_[modbases_specifier]_<model_type>_[instrument_type].cfg`  
    - `<strand_type>` ：'DNA'或'RNA'
    - `<pore_type>` : 使用的测序芯片类型
    - `<enzyme_type>` : 测序试剂使用的马达酶,可以以酶的版本或酶的过孔速度来指定,如'e8.1'或'450bps'
    - `[modbase_specifier]` : 可选项。用于识别修饰碱基，如'5mc_cg'或'5hmc_5mc_cg'
    - `<model_type>`: fast、hac、sup或sketch模式(Sketch basecalling. This is primarily for use with adaptive sampling on the MinION Mk1C device to minimise latency.)
    - `[instrument_type]`: 可选项。如果没有指定就默认为GridION或PC。'mk1c'或'prom'分别自带为MinION Mk1C或PromethION 
  - `[option]`为其他配置参数：
    - `x auto` 自动选择和使用最适合basecalling的计算设备。这通常包括自动检测系统中可用的GPU，并选择最佳的GPU进行数据处理。如果没有检测到合适的GPU，它可能会回退到使用CPU。
    - `chunks_per_runner` 控制每个 basecalling 进程一次处理的数据块数量。减少这个数字可以减少每个进程的内存使用，但可能会影响处理速度。
    - `records_per_file`决定每个输出文件中包含的读取数。减少此值可以减少处理过程中单个文件的内存占用。
    - `num_callers`  控制同时运行的 basecalling 线程数。减少这个数值可以显著降低总体内存使用。
4. 数据转换耗时长，在运行命令前建议使用`screen`打开新的终端窗口。
5. 运行数据转换命令，如`guppy_basecaller -i fast5_pass/ -s /mnt/Data/public/ACE2/20240410/fastq_pass_sup -x auto -c dna_r9.4.1_450bps_sup.cfg --chunks_per_runner 100`

### Reference
[Oxford Nanopore Technologies碱基识别软件Guppy的关键参数“-c”](https://blog.csdn.net/m0_67672416/article/details/130008693)

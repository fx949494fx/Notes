数据质控流程：
```bash
# 更新表型数据并转二进制
plink --file TOP --pheno phenotype.txt --make-bed --out data

# 去除chr0 ，XY，MT
plink --bfile data --not-chr 0 25 26 --make-bed --out filter

# 检查样本的call rate，移除call rate低于98%的样本
plink --bfile filter --mind 0.02 --make-bed --out data_qc

# 检查SNPs的call rate，移除call rate低于98%的SNPs
plink --bfile data_qc --geno 0.02 --make-bed --out data_qc_snp

# 检查并纠正性别不一致问题
plink --bfile data_qc_snp --check-sex --make-bed --out data_qc_sex

# 检查Hardy-Weinberg平衡，移除P-value小于0.001的SNPs
plink --bfile data_qc_sex --hwe 0.001 --make-bed --out data_hwe

# 检查并移除连锁不平衡的SNPs
plink --bfile data_hwe --indep-pairwise 50 5 0.2 --out data_ld

# 使用上一步生成的.prune.in文件来过滤SNPs
plink --bfile data_hwe --extract data_ld.prune.in --make-bed --out data_pruned

# 进行关联分析
plink --bfile data_pruned --pheno phenotype.txt --assoc --adjust --out gwas_results
```

### 参考资料
- [Plink官网](https://www.cog-genomics.org/plink/1.9/)
- [全基因组关联分析学习资料（GWAS tutorial）20210313更新版](https://www.cnblogs.com/chenwenyan/p/11803311.html)
- [GWASLab](https://gwaslab.org/blog/)

# 1. 我今天完成了什么

寻找高变基因并查看统计  

---

# 2. 我仍然不懂什么

为什么adata.var["highly_variable"]和adata.var[adata.var["highly_variable"]]输出的结果不一样？因为前者是查看一整列，后者是布尔索引/过滤，内部的 adata.var["highly_variable"] 产生了一个由 True 和 False 组成的掩码（Mask）。 外部的 adata.var[...] 告诉 Python：“只把那些对应位置是 True 的行拿给我看”。  

---

# 3. 今天最重要的概念

- 计算高变基因：sc.pp.highly_variable_genes(adata, n_top_genes=1, subset=False)，找出不同细胞之间变化最大的基因，排除管家基因输入是adata.X，输出是adata.var  

- 显示并统计高变基因：adata.var["highly_variable"]输出高变基因列，结果有false有true。adata.var[adata.var["highly_variable"]]是提取高变基因列中结果为true的，adata.var["highly_variable"].sum()是统计多少个是true的高变基因。  

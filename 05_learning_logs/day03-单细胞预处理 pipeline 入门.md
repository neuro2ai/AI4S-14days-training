# 1. 我今天完成了什么

1. AnnData的转化  
2. 归一化  
3. log1p  

---

# 2. 我仍然不懂什么（至少1条）

为什么adata不是矩阵本身，adata.X才是表达矩阵，为什么是X不是其他字母,adata中除了表达矩阵，其他信息怎么调用显示  

adata
├── X      （表达矩阵，在机器学习里：X = 输入特征矩阵，y = 标签）  
├── obs    （细胞信息）  
├── var    （基因信息）  
├── obsm   （降维结果，比如 PCA/UMAP）  
├── layers （不同版本的数据）  

---

# 3. 今天最重要的概念

- import numpy as np：导入数值计算库，用来处理矩阵。import pandas as pd：导入表格数据处理库，用来构建带标签的数据表。import scanpy as sc：导入单细胞分析库，后面 sc.AnnData、sc.pp.normalize_total 都来自它。  

- sc.AnnData(expr_df)：把 DataFrame 包装成一个单细胞分析常用的数据对象，可以包含“表达矩阵、细胞注释、基因注释、后续降维结果，不同处理层”.AnnData object with n_obs × n_vars = 5 × 4中n_obs是细胞数，n_vars是基因数  

- 计算原始总表达量（单个细胞的所有基因表达量）adata.X.sum(axis = 1)，看总counts有没有过大  

- 归一化sc.pp.normalize_total(adata, target_sum = 1e4)..pp是preprocession预处理，normalize_total是把每个细胞的总表达量缩放到同一水平，target_sum = 1e4是总量标准值设为10000。目的：归一化后，每个细胞都被拉到同一个“总量基准”上，再比较基因表达，让不同细胞之间更可比。得到的adata是对之前的adata进行原地修改  

- 做log1p：sc.pp.log1p(adata),对表达矩阵做 log(1 + x) 变换。+1能防止log0的出现。目的：原始表达数据通常分布不均匀，存在极端值，log之后数值范围更平衡，更适合后续PCA、邻居图、聚类等分析，统计更稳定。  

---

## 问题自检：

- adata 和 adata.X 的区别是什么？adata.X是表达矩阵，adata中还有其他信息  

- 为什么归一化之后，每个细胞总表达量会变得接近？因为设定了相同的sum目标值  

- 为什么 log1p 不是直接取 log？防止出现log0无穷大  

- 为什么单细胞不用“一个 DataFrame 跑到底”，而要用 AnnData？  单细胞分析不仅需要表达矩阵，还需要统一管理细胞注释、基因注释以及中间结果（如 PCA、UMAP 等），AnnData 提供了一个标准化容器来组织这些信息，而 DataFrame 无法很好管理多层数据结构。  

- 如果你做完 PCA，结果应该放在哪？adata.obsm  

- 为什么 PCA 结果不能放在 adata.X？  adata.X 约定保存的是细胞 × 基因表达矩阵，而 PCA 结果已经不是基因空间，而是主成分空间，所以不能直接覆盖到 adata.X。 

- adata.obsm["X_pca"] 的 shape 为什么是 (5, 2)，而不是 (5, 4)？  adata.obsm["X_pca"] 的 shape 是 (5, 2)，因为现在还是 5 个细胞，但每个细胞不再用 4 个基因表示，而是用 2 个主成分表示。  PCA降维后只有两个维度，只保留了两个成分  

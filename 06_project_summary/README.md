# AI4S 14天最小成果包

## 目标
在14天内，从接近零基础出发，完成一个可展示给潜在导师看的最小成果包，证明自己已经能够理解并跑通基础单细胞分析与最小模型 pipeline。

## 我完成了什么
1. 搭建了独立 Python/conda 环境，能处理 Jupyter 与 kernel 问题
2. 理解了单细胞表达矩阵、AnnData、obs/var/obsm 等核心数据结构
3. 跑通了基础单细胞预处理流程：normalize、log1p、HVG、PCA、neighbors、UMAP
4. 学习了 PyTorch 基础：tensor、shape、Linear、forward、Dataset、DataLoader
5. 构建了一个最小单细胞模型 pipeline：adata.X -> tensor -> Dataset -> DataLoader -> encoder -> embedding
6. 理解了 loss、backward、optimizer.step 的作用，完成了最小训练闭环演示

## 当前仍然有限的地方
1. 当前模型仍是教学级最小模型，不是成熟的单细胞 foundation model
2. 当前训练目标是人为构造的示例，不直接对应真实生物任务
3. 对更复杂模型（如图结构模型、transformer、foundation model）的理解还在建立中

## 这份成果包想说明什么
我虽然起点不高，但已经能够把单细胞数据结构、预处理流程、基础神经网络和最小训练机制连接起来，并开始具备理解 AI4S 模型 pipeline 的能力。

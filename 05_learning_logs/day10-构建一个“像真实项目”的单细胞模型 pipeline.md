# 1. 我今天完成了什么

从dataframe到anndata  
定义dataset和dataloader  
定义forward  

---

# 2. 我仍然不懂什么

1.为什么print("input:", batch.shape)和print("output:", y.shape)放在循环内和循环外结果不同？  

2.放在循环外则只print最后一个batch？Python 循环结束后，变量仍然保留最后一个值  

3.为什么要X_tensor.shape[1]到8再到2，不能直接到2吗？  

4 → 8 → ReLU → 2。其中8是隐藏层，作用是提取更复杂的特征组合、提高表达能力。  

直接 4→2：像用一条直线拟合数据  

4→8→2：像先展开特征，再压缩  

---

# 3. 今天最重要的3个概念

1. 这个pipelin本质上是做没有训练的 embedding 映射，模型是model = CellEncoder()，参数是随机初始化，用于演示从表达矩阵到 embedding 的计算流程，并未经过训练，因此输出不具备生物学解释意义。  

2. 最小AI4Spipeline：  

AnnData  
→ tensor  
→ Dataset  
→ DataLoader  
→ model  
→ embedding  

3.for batch in loader: y = model(batch)：这是一个循环，每一轮batch被更新，y被更新  

---

# 4.问题自检：

1.这个 pipeline 的输入是什么？  

一个矩阵，行是细胞，列是基因，size是（细胞数，基因数）  


2.输出 (batch_size, 2) 的含义是什么？  

一批中包含2个细胞，批次为2  

纠正：  

• batch_size = 这一批有多少个细胞  
• 2 = 每个细胞被映射成一个2维向量（embedding）  


3.这个模型在“学什么”？（不是代码层面，是概念）  

什么都没学，这只是一个随机初始化的编码器，用于演示从表达矩阵到embedding的计算流程，未经训练，所以没有生物学意义  

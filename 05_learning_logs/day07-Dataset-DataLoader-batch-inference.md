# 1. 我今天完成了什么

1. 定义Dataset和调用DataLoader，以及两者的验证  
2. 完成inference  

---

# 2. 我仍然不懂什么（至少1条）

1.为什么要用def getitem(self, idx): return self.X[idx]随机生成索引（给一个索引，返回一个样本）？  

- 防偏见： 避免模型因为实验操作的先后顺序（比如先做低浓度，后做高浓度）而产生误判。  
- 抗作弊： 掐断模型通过“排队顺序”猜答案的可能性，逼它直视数据的生物特征。  
- 更聪明： 数学上叫 SGD（随机梯度下降），随机抓取的样本能让模型在寻找最优解时更有活力，不容易困在死胡同里。  

2.torch.utils.data都包含什么？  

torch.utils.data是一个工具集，包含数据集基类Dataset（提供len和getitem接口）、数据加载器Dataloader（batching分批打包、Shuffling打乱顺序防止模型死记硬背、Multi-processing开启多个CPU线程并行读数据）、采样Sampler（指定从Dataset中提取样本的规则）、数据划分Subset&random_split（快速把一个大表拆分成训练集和验证集）  

3.inference是什么，forword前向传播、loss损失函数、backward反向传播、update更新参数等有什么联系和区别？  

一次训练流程是  

输入 x  
↓  
forward（前向传播）：输入数据-输出预测y = model(x)  
↓  
loss（计算误差）：预测 vs 真值 → 算误差loss = criterion(y, target)  
↓  
backward（算梯度）：计算每个参数的梯度loss.backward()  
↓  
update（更新参数）：用梯度更新参数optimizer.step()  

inference是只做y = model(x)，不做后续三步，因此inference和训练是不同的  

---

# 3. 今天最重要的概念

1. from torch.utils.data import Dataset  

Dataset规定了所有数据集都必须具备“告诉我你有多少数据（len）”和“给我指定的一件数据（getitem）”这两个功能。Dataset负责数据源管理（数据在哪？怎么读？怎么把一条生信序列变成一个 Tensor？），Dataloader负责数据装载管理（一次喂给模型多少个样本（Batch Size）？要不要打乱顺序？要不要开 4 个线程提速？）。如果有dataset解耦，就不需要数据一次性占据大量内存，只有在模型需要那第n号样本时，才会去硬盘里读那一条数据。  

2. def len(self): return self.X.shape[0]：shape[0]括号中写0是因为X 通常是 (细胞数, 基因数)，shape[0]是告诉pytorch数据集中一共有多少个细胞用于训练  

3. def getitem(self, idx): return self.X[idx]： 给一个索引 idx，返回一个样本，即每次取一个细胞的表达向量  

4. 验证：len(dataset)返回细胞数（矩阵的行），dataset[0]返回第一个细胞的tensor行向量，dataset[0].shape返回基因数  

5. dataset = CellDataset(X)是进行类的实例化，包含的步骤有触发init、把矩阵X存进self.X让dataset这个变量记住这批数据，之后我可以用 len(dataset) 随时问它有多少个样本，用 dataset[idx] 随手抽出一行数据  

6. DataLoader():

```python
train_loader = DataLoader(
    dataset=dataset,      # 1. 你的数据集实体
    batch_size=32,        # 2. 批次大小
    shuffle=True,         # 3. 是否打乱顺序（每个训练轮次（Epoch）开始前，是否把数据重新洗牌）
    num_workers=2,        # 4. 开启几个线程读数据（Mac 建议设为 0 或 2）
    drop_last=False       # 5. 不够一个批次的数据要不要扔掉
)
```

 7. for batch in loader: y = model(batch)：把batch喂进模型，完成inference（inference = 用已经定义好的模型，对新数据做前向计算，得到输出）
 # 4.问题自检：

 1. 为什么不能每次只喂一个样本，而要用 batch？

因为batch一次处理多个样本，能加快处理速度  

补充：使用 batch 不仅是为了并行计算提升效率，还能让梯度估计更稳定（不会被单个样本噪声主导），并充分利用硬件（CPU/GPU/MPS）的向量化计算能力。  

32 个样本。虽然其中可能有 2 个是噪声，但剩下的 30 个正常样本会通过求平均值（Average）把噪声抵消掉。Batch 实际上是对整个数据集分布的一个统计抽样。样本越多，抽样算出的梯度就越接近真实的“真理方向”。这种“集体决策”让模型在 Update（更新参数） 时更加稳健。  

在 PyTorch 中： 当你使用 nn.Linear 处理一个 [32, 4] 的 Batch 时，硬件并不会真的循环跑 32 次加法。它会利用向量化单元，在同一个时钟周期内，同时完成这 32 个样本的矩阵运算。  


 2. Dataset 和 DataLoader 分别解决什么问题？

dataset解决数据集储存，dataloader数据集加载  

标准答案：  

Dataset 负责“定义如何取一条数据”（索引→样本），  
DataLoader 负责“如何把多条数据组织成 batch，并控制顺序、并行读取等”。  


 3. 如果输入 batch 是 (2, 4)，输出为什么是 (2, 2)？

linear是（4，2），把四维降成二维  

更准确：  

nn.Linear(4,2) 表示一个从 4 维特征空间到 2 维特征空间的线性映射，本质是一个矩阵乘法y=xWT+b，而不是简单“压缩”。  


 4. 如果没有 getitem，DataLoader 为什么不能工作？

没办法从dataset中取数据，自然也就无法后续对数据进行分批打包等处理  


 5. shuffle=True 是在哪一步起作用？Dataset 还是DataLoader？

DataLoader，起打乱顺序的作用  


 6. 训练和 inference 的本质区别是什么？（一句话）

本质是有没有用损失函数对模型进行权重更新，  

训练 = forward + loss + backward + 参数更新  
inference = 只有 forward，不改变模型  

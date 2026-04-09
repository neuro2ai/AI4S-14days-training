# 1. 我今天完成了什么

1. pytorch的安装及导入  
2. torch的转化  
3. 定义最小模型（连接层）  
4. 跑前向传播  

---

# 2. 我仍然不懂什么

1.tensor是什么？为什么用tensor不用numpy？  

可以把 Tensor 看作是高维版的 NumPy 数组（ndarray）Tensor = ndarray + 自动求导（autograd）+ 设备管理（CPU/GPU/MPS）+ 深度学习算子生态  

优点1.算得快：NumPy 只能在 CPU 上运行。而 PyTorch 的 Tensor 可以直接“搬”到Mac GPU (MPS) 上。2自动求导：在训练模型时，我们需要计算“误差”对“参数”的梯度。Tensor 能自动帮你把导数求出来  

2.为什么要定义最小模型？  

nn.Linear(4, 2) 代表一个全连接层（Linear Layer）。输入 (4)： 有 4 个特征（比如 4 个高变基因 geneA, B, C, D 的表达量）。输出 (2)： 把这 4 个信息压缩成 2 个核心信号。  

输出 = 输入* 权重 + 偏置。当定义了 model，PyTorch 会在后台自动创建两组 Tensor：权重 (Weights)： 一个 2*4 的矩阵。偏置 (Bias)： 一个长度为 2 的向量。我们需要定义这个模型结构，才能让它通过反向传播（Backpropagation）去学习。  

5. 偏置矩阵初始值是多少？如果你没有手动设置，PyTorch 会默认使用一种叫做 Kaiming Uniform（也叫 He Initialization 的变体）的数学算法，从一个特定的分布中随机抽取数字填满权重矩阵 W和偏置 b。  

---

# 3. 今天最重要的概念

- 安装pytorch：在终端激活环境conda activate ai4s14, 安装pip install torch torchvision torchaudio, 在Jupyter导入import torch  

- 把python列表变成pytorch tensor：torch.tensor([])。模型只能输入tensor，不能输入list或者dataframe。  

- 模型输入(batch_size, n_features)  

- 定义一个最小模型：导入神经网络模块(torch.nn 层级：专门处理神经网络（Neural Networks）。它把复杂的数学运算封装成了我们易于理解的“层”（Layers），比如 Linear（线性层）、Conv2d（卷积层）)import torch.nn as nn。model = nn.Linear(4,2)输入4维，输出2维  

- 把输入x传进模型：y = model(x)，这叫前向传播  

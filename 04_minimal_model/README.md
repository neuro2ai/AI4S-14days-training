# 最小模型 Pipeline（Day 10–12）

## 本目录做什么
将单细胞数据与最小 PyTorch 模型连接，构建从数据到训练的完整闭环。

## 涉及内容
- adata.X → tensor
- Dataset / DataLoader
- CellEncoder（最小编码器）
- forward 输出（embedding）
- target 构造（示例任务）
- loss 计算
- backward（梯度计算）
- optimizer.step（参数更新）

## 输入是什么
- 预处理后的表达矩阵（AnnData.X）

## 输出是什么
- embedding（模型输出表示）
- loss 数值
- 参数更新前后变化（训练闭环）

## 对应 notebook
- day10_to_day12_minimal_model_pipeline.ipynb

## 这一部分证明了什么
我能够把“单细胞数据结构 → 数据加载 → 模型前向 → 损失函数 → 反向传播 → 参数更新”连接成一个完整 pipeline，并理解每一步在训练中的作用。

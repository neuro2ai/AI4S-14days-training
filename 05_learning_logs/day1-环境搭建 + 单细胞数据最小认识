今日学习记录
1. 我今天完成了什么
• 环境的配置
• 单细胞矩阵的初步尝试
2. 今天最重要的 3 个概念
• 判断路径：which name，判断版本；name --version，进入路径：cd . 报告路径pwd
• 创建文件夹mkdir -p name.mkdir 是创建目录。-p 的意思是：如果上级目录不存在，就一起创建；如果已经存在，也不报错。
• conda create -n ai4s14 python=3.10 -y（-n ai4s14 是指定环境名。python=3.10 是指定 Python 版本。-y 是默认自动同意安装，不让你中途反复确认。现在的 3.13 太新，容易和后面要装的科学计算包不兼容。）
•  conda activate ai4s14激活环境，激活后终端前面从 (base) 变成 (ai4s14)
• pip install jupyter numpy pandas matplotlib ipykernel安装需要的包：pip install（jupyter：启动 notebooknumpy：矩阵和数值pandas：表格和数据框matplotlib：画图ipykernel：把当前 conda 环境注册成 notebook 可选内核）
• pip list | grep -E "jupyter|numpy|pandas|matplotlib|ipykernel"从已安装包列表里筛选出这几个关键包，| 是把前一个命令的输出结果，直接塞给后一个命令当作输入数据。grep 是 Global Regular Expression Print 的缩写，是文本搜索工具。-E (扩展正则表达式 / Extended RegEx)是 grep 的一个参数（选项），grep -E： 允许你使用更强大的“搜索规则”，它们之间是‘或者’的关系”，只要这行里出现了 jupyter，或者出现了 numpy，或者出现了 pandas（以此类推），就请把它显示出来。
• python -m ipykernel install --user --name ai4s14 --display-name "Python (ai4s14)"把当前环境注册成一个 Jupyter 内核。（python -m ipykernel install：用当前 Python 调 ipykernel 模块来注册内核--user：装到当前用户层面，不需要系统管理员权限--name ai4s14：内核内部名字--display-name "Python (ai4s14)"：你在 Jupyter 页面里看到的名字
• import sys导入 Python 的系统模块 sys，能告诉你当前 notebook 实际在调用哪个 Python 解释器。、print(sys.executable）打印当前 notebook 的 Python 路径，检查内核。
• expr_df = pd.DataFrame(data, index=cells, columns=genes)index是行，调用pandas创建一个表格。expr_df.shape看矩阵形状（行数，列数）。cell_sums = expr_df.sum(axis=1)其中axis=1是计算行总数，axis=0是计算列总数
• import matplotlib.pyplot as plt导入python画图包。cell_sum.plot(kind = "bar")柱状图。plt.ylabel("Total expression per cell")纵坐标名
4. 我现在能不能给别人讲清楚这部分
请用 3 句话回答：
• 我能讲清楚的部分是：看到代码知道在干什么，知道单细胞的细胞*矩阵是什么样子
• 我讲不清楚的部分是：sys.executable我只知道这是在看路径，但不知道为什么不在终端用pwd看路径。答案：pwd 看的是当前终端的工作目录（working directory）路径，也就是你当前操作文件的所在位置。sys.executable 表示当前运行这段代码的 Python 解释器路径，也就是 notebook 背后真正调用的环境。notebook 的工作目录只决定文件位置，而包的导入取决于当前 Python 解释器；如果 kernel 不对应正确环境，即使路径正确，也会出现 import 失败。

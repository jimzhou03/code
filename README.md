# 中文仇恨言论检测项目
本项目基于GLM-4-9B大模型实现细粒度片段级中文仇恨言论识别，能够从社交媒体文本中提取结构化仇恨四元组（评论对象、论点、目标群体、是否仇恨）。

## 项目背景
随着社交媒体的普及，仇恨言论的传播已成为严重社会问题。本项目针对细粒度片段级中文仇恨言论识别任务，基于[GLM-4-9B-0414](https://www.modelscope.cn/models/ZhipuAI/GLM-4-9B-0414)大模型进行微调，实现对文本中仇恨四元组的精准识别。

## 任务描述
**输入**：社交媒体文本  
**输出**：仇恨四元组（顺序：`评论对象 | 论点 | 目标群体 | 是否仇恨`）  
- 多个四元组用 `[SEP]` 分隔
- 每个四元组以 `[END]` 结尾
- **格式示例**：
  ```text
  评论对象A | 论点A | 目标群体A | hate [END] [SEP] 评论对象B | 论点B | 目标群体B | non-hate [END]

环境要求:
硬件配置
GPU：2× NVIDIA GPU（每卡24GB显存）
CPU：24核
内存：64GB


## 软件依赖:
PyTorch == 2.2.2
pip install ms-swift transformers

数据集:
训练集：train.json
测试集：test1.json, test2.json

数据集格式：
{
    "id": "样本唯一ID",
    "content": "原始文本内容",
    "output": "评论对象 | 论点 | 目标群体 | 是否仇恨 [END]"
}

使用流程
1. 数据预处理
将JSON格式转换为JSONL格式：
python3 change.py

2. 模型微调
运行微调脚本：sh sft.sh

3. 模型训练
根据微调后的检查点继续训练：
sh train.sh

4. 生成预测结果
python3 submit.py

# 项目结构

- data/                   # 原始数据集
  - train.json
  - test1.json
  - test2.json
- scripts/                # 执行脚本
  - sft.sh              # 微调脚本
  - train.sh            # 训练脚本
- utils/                  # 工具脚本
  - change.py           # 数据格式转换
  - submit.py           # 结果提交转换
- output/                 # 输出目录
  - checkpoint-xxx/     # 模型检查点
  - sft_data.jsonl      # 微调训练数据
  - test_data.jsonl     # 测试集数据
- README.md
## 比赛信息
竞赛名称：细粒度片段级中文仇恨言论识别

竞赛地址：天池竞赛平台https://tianchi.aliyun.com/competition/entrance/532298/information

任务目标：构建结构化仇恨言论四元组，增强模型在细粒度场景下的检测能力和决策可解释性





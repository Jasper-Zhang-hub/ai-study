# 中文命名实体识别：BERT、CRF 与大模型方法

中文序列标注学习项目，包含 BERT + Linear、BERT + CRF、Qwen API 提示及 Qwen 指令微调（LoRA）实现。支持 CLUENER 和人民日报 NER 数据处理；大模型流程面向 CLUENER。

## 文件结构

- `src/`：数据下载、探索、BERT 训练、评估与结果汇总。
- `src_llm/`：大模型 API 识别、指令微调及评估。
- `results/`：历史实验汇总，不含评估原文和逐样本预测。
- `USAGE_GUIDE.md`：环境和运行方法。
- `requirements.txt`：原项目依赖范围，尚未锁定经过验证的完整环境。

## 快速开始

Python 建议使用原项目缓存对应的 3.12；本发布整理未重跑训练，也未验证全新环境安装。

```bash
pip install -r requirements.txt
python src/download_data.py
python src/explore_data.py
python src/train.py --dataset cluener --use_crf
python src/evaluate.py --dataset cluener --use_crf --split validation
```

训练前需要自行准备本地预训练模型，路径见使用说明。数据、模型权重和 API 密钥不随仓库发布。

## 已有实验记录

以下是随项目保存的历史日志，并非此次重新复现的结果。

| 日志 | 评估范围 | F1 |
|---|---|---|
| `eval_linear_validation.json` | CLUENER validation | 0.755604 |
| `eval_crf_validation.json` | CLUENER validation | 0.752588 |
| `eval_peoples_daily_crf_validation.json` | 人民日报 validation | 0.937364 |
| `eval_llm.json` | 抽样 100 条，zero-shot / few-shot | 0.518072 / 0.484375 |
| `eval_sft.json` | 抽样 50 条 | 0.398148 |

不同数据集、抽样规模与指标实现不能直接作为公平排名。CRF 当前实现没有显式硬编码 BIO 合法转移约束，不能保证零非法序列；日志中的非法项计数也不应直接理解为去重后的非法句子数量。CLUENER test 无公开标签，仓库不保留原来在该集合上得到的零分评估。

历史结果单独放在 `results/`，新运行结果仍写入 `outputs/`。当前 `compare_results.py` 使用旧的日志命名，与训练/评估脚本部分新文件名不一致，重新实验后需统一文件名再汇总。

## 数据来源与发布范围

数据下载地址来自原项目 `src/download_data.py`：CLUENER 使用 CLUE 存储地址，人民日报数据使用 OYE93/Chinese-NLP-Corpus。此包不包含这些第三方数据，也不包含预训练模型、训练权重或 tokenizer。

原 README 是 CLUENER 数据集介绍，其邮箱不作为本项目维护者联系方式。原课程代码的作者归属与再发布授权尚未确认，本包没有擅自添加 MIT 或其他许可证。正式以开源项目发布前，请确认代码授权并补充适用的 LICENSE 和来源署名。

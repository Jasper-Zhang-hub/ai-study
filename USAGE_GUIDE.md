# 运行说明

所有命令从仓库根目录执行。

## 环境与模型

```bash
pip install -r requirements.txt
```

将 BERT 模型放在 `pretrain_models/bert-base-chinese/`，将 Qwen 模型放在 `pretrain_models/Qwen2-0.5B-Instruct/`。需要完整模型文件及相应 tokenizer 配置，不能只放权重文件。BERT 使用本地加载。

也可通过 BERT 脚本的 `--bert_path` 和 SFT 脚本的 `--model_path` 指定自己的模型目录。整理版统一了默认路径；未修改算法。

## BERT 流程

```bash
python src/download_data.py
python src/explore_data.py
python src/train.py --dataset cluener
python src/evaluate.py --dataset cluener --split validation
python src/train.py --dataset cluener --use_crf
python src/evaluate.py --dataset cluener --use_crf --split validation
python src/train.py --dataset peoples_daily --use_crf --epochs 1 --batch_size 8 --max_length 64 --grad_accum 2
python src/evaluate.py --dataset peoples_daily --use_crf --split validation
```

BERT 默认训练参数以脚本为准：epochs=2、batch_size=16、max_length=64、grad_accum=4。

## 大模型 API

PowerShell 中配置环境变量：

```powershell
$env:DASHSCOPE_API_KEY = "your_api_key_here"
python src_llm/llm_ner.py --n_samples 100
```

`.env.example` 仅提供变量名称示例；代码不会自动加载 `.env`。API 流程会将选取的文本发送给配置的服务商，并使用你的账户额度。

## 指令微调

```bash
python src_llm/train_sft.py
python src_llm/evaluate_sft.py
```

训练与评估应使用同一基础模型。其他参数用各脚本 `--help` 查看。此包没有训练产物，需要先训练再评估。

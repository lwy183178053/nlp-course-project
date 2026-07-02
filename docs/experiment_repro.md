# 中文文本分类实验复现说明

## 项目来源

本项目基于开源仓库 `649453932/Chinese-Text-Classification-Pytorch` 修改，原项目地址：

<https://github.com/649453932/Chinese-Text-Classification-Pytorch>

原项目采用 MIT License，本仓库保留 `LICENSE` 文件和上游仓库地址。

## 本次改动

- `run.py` 增加 `--dataset`、`--seed`、`--epochs`、`--batch-size`、`--require-improvement`、`--max-train`、`--max-dev`、`--max-test` 参数。
- `utils.py` 和 `utils_fasttext.py` 支持限制训练集、验证集、测试集样本数量。
- 数据迭代器修复了小样本数量小于 batch size 时的除零问题。
- `train_eval.py` 在缺少 `tensorboardX` 时降级为空日志写入器，并让小样本测试集缺失部分类别时仍能输出完整评价报告。

默认不传新增参数时，项目仍使用原来的数据集、训练轮次、batch size 和早停配置。

## 环境

本次验证使用本机 Anaconda 环境：

- Conda 环境：`Mybase`
- Python：`D:\ProgramData\anaconda3\envs\Mybase\python.exe`
- PyTorch：`2.8.0+cu128`
- scikit-learn：`1.7.2`
- tqdm：`4.67.1`

FastText 的 n-gram embedding 参数量较大，GPU 运行时出现 CUDA 显存分配错误，因此复现实验使用 CPU。PowerShell 中可通过以下方式临时禁用 CUDA：

```powershell
$env:CUDA_VISIBLE_DEVICES = ''
$py = 'D:\ProgramData\anaconda3\envs\Mybase\python.exe'
```

## 验证命令

静态检查：

```powershell
& $py -m py_compile run.py utils.py utils_fasttext.py train_eval.py
```

正式轻量实验：

```powershell
& $py run.py --model FastText --embedding random --epochs 1 --batch-size 32 --max-train 5000 --max-dev 1000 --seed 1
```

小样本边界验证：

```powershell
& $py run.py --model FastText --embedding random --epochs 1 --batch-size 32 --max-train 16 --max-dev 32 --max-test 128 --seed 1
```

## 实验结果

正式轻量实验使用 5000 条训练样本、1000 条验证样本和完整 10000 条测试样本。选择完整测试集的原因是 `test.txt` 前 1000 条按类别块排列，只截取前 128 或 1000 条会导致测试集只包含单一类别。

FastText 随机 embedding、1 个 epoch、batch size 为 32 时：

- 验证集在第 100 个 batch 达到 `Val Acc: 46.60%`
- 测试集达到 `Test Acc: 48.14%`
- 测试集 macro F1 为 `0.4789`

该结果低于原项目 README 中完整训练的 FastText 准确率，原因是本实验只训练 1 个 epoch 且仅使用 5000 条训练样本，目的在于课程作业中的快速复现和改动验证。

小样本边界验证中，`--max-train 16` 小于 `--batch-size 32`，训练、测试和评价报告均能正常完成，说明数据迭代器除零问题已修复。

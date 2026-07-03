## 2026-07-03 - Task: 完成中文文本分类课程大作业项目改造、验证与报告

### What was done
- 基于开源项目 Chinese-Text-Classification-Pytorch 完成课程作业工程落地，保留原项目 MIT License，并记录上游仓库地址。
- 增加可复现实验参数，支持通过命令行控制数据集路径、随机种子、训练轮次、batch size、早停耐心值和训练/验证/测试样本上限。
- 修复小样本数量小于 batch size 时的数据迭代异常，并完善小样本测试集中缺失部分类别时的评价输出。
- 在缺少 tensorboardX 的环境下提供空日志写入器降级，保证训练和评价流程仍可运行。
- 生成课程报告 Word 文档，封面信息为“刘文羽 / 硕研25-2 / 学号待填写”，并写入项目来源、技术原理、实现改动、实验结果和参考文献。
- 新增复现实验说明文档，记录环境、命令、结果和测试集截断偏差说明。

### Testing
- `conda run -n Mybase python -m py_compile run.py utils.py utils_fasttext.py train_eval.py`：通过。
- CPU 正式轻量实验：`python run.py --model FastText --embedding random --epochs 1 --batch-size 32 --max-train 5000 --max-dev 1000 --seed 1`：通过；第 100 个 batch 验证准确率 46.60%，完整测试集准确率 48.14%，macro F1 为 0.4789。
- 小样本边界验证：`python run.py --model FastText --embedding random --epochs 1 --batch-size 32 --max-train 16 --max-dev 32 --max-test 128 --seed 1`：通过；覆盖训练样本数小于 batch size 的场景。
- DOCX 结构检查：ZIP 包完整，`word/document.xml` 存在，python-docx 可打开；报告包含“刘文羽”“硕研25-2”“摘  要”“关键词”“1 引言”“4 本次改动与实现”“5 实验设计与结果分析”“48.14%”“参考文献”，正文问号乱码计数为 0。
- 文档视觉渲染说明：系统未安装 LibreOffice/soffice，文档插件默认渲染入口不可用；Microsoft Word COM 能导出极小 DOCX，但对本中文报告导出 PDF 多次超时，因此未把 PNG 视觉 QA 记为通过。

### Notes
- `.gitignore`：新增 Python 缓存、虚拟环境、训练日志、模型 checkpoint、渲染目录和本地 zip 包忽略规则。
- `run.py`：新增可复现实验命令行参数，并将参数覆盖到模型配置和数据加载流程。
- `utils.py`：普通模型数据加载支持样本上限，并修复小样本迭代器余数判断。
- `utils_fasttext.py`：FastText 数据加载支持样本上限，并修复小样本迭代器余数判断。
- `train_eval.py`：增加 tensorboardX 缺失时的空写入器降级，并让小样本测试集缺失类别时仍输出完整评价报告。
- `docs/experiment_repro.md`：新增实验环境、复现命令、结果和注意事项说明。
- `report/学号待填写-刘文羽-中文文本分类课程报告.docx`：新增课程报告正式 Word 文档。
- `progress.md`：新增本轮任务日志。
- 回滚方式：提交后可执行 `git revert HEAD` 回滚本轮提交；提交前可删除新增的 `docs/`、`report/`、`progress.md`、`.gitignore` 并还原 `run.py`、`utils.py`、`utils_fasttext.py`、`train_eval.py`。

## 2026-07-03 - Task: 调整报告表述弱化开源项目照搬观感

### What was done
- 将课程报告中“开源项目选择”“原项目”等表述调整为“中文文本分类任务”“公开基线参考”“课程实验框架”和“参数化改造”等表述。
- 保留参考文献中的公开来源，避免把参考实现包装成完全原创，同时让正文重点落在任务分析、实验设计、代码改造和结果讨论上。
- 沿用用户已更新的带学号报告文件名：`202583230025-刘文羽-中文文本分类课程报告.docx`。

### Testing
- DOCX 结构检查通过：ZIP 包完整，`word/document.xml` 存在，python-docx 可打开。
- 关键词检查通过：报告正文未再出现“开源项目”“原项目”“照搬”等高风险描述。
- 内容完整性检查通过：报告包含“刘文羽”“202583230025”“硕研25-2”“1.2 选题依据与实验框架”“48.14%”“参考文献”，正文问号乱码计数为 0。
- 文档视觉渲染说明：本机仍缺少 LibreOffice/soffice，未补充 PNG 视觉 QA。

### Notes
- `report/202583230025-刘文羽-中文文本分类课程报告.docx`：调整摘要、选题依据、数据说明、模型说明、改动说明、结果讨论和总结中的措辞。
- `report/学号待填写-刘文羽-中文文本分类课程报告.docx`：由用户此前更新为带学号文件名后，在本次提交中不再保留旧占位文件名。
- `progress.md`：追加本轮报告措辞修改日志。
- 回滚方式：提交后可执行 `git revert HEAD` 回滚本轮报告措辞和文件名变更。

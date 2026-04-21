# 游戏本地化 Skill 测试指南

## 📋 测试文件说明

我已经为您创建了两个测试文件,基于辐射避难所的真实数据结构:

### 1. 参考文件 (Reference File)
**文件**: `examples/reference_fallout_shelter.csv`
- **用途**: 提取术语表,建立翻译一致性
- **内容**: 20 行已翻译的辐射避难所内容
- **包含术语**:
  - Vault → 避难所
  - Dweller → 居民
  - Deathclaw → 死爪
  - Stimpak → 治疗针
  - RadAway → 消辐宁
  - Wasteland → 废土
  - CAPS → 瓶盖
  - Nuka-Cola → 核子可乐
  - Radroach → 辐射蟑螂

### 2. 待翻译文件 (To Translate)
**文件**: `examples/to_translate_new_content.csv`
- **用途**: 测试翻译功能
- **内容**: 20 行新的游戏内容,需要翻译
- **包含占位符**:
  - `{0}`, `{1}` - 位置占位符
  - `%d` - Printf 风格占位符
  - `<color=red>`, `<color=yellow>` - Unity 富文本标签

---

## 🧪 测试步骤

### 测试 1: 基础翻译 (不使用术语表)

```powershell
cd /path/to/game-localization

python scripts/translate.py examples/to_translate_new_content.csv --target zh-CN
```

**预期结果**:
- ✅ 生成 `to_translate_new_content_translated.csv`
- ✅ 生成 `to_translate_new_content_qa_report.json`
- ✅ 脚本在完成后停止 (不循环)
- ⚠️ 术语可能不一致 (因为没有术语表)

---

### 测试 2: 使用参考文件提取术语表

```powershell
python scripts/translate.py examples/to_translate_new_content.csv --target zh-CN --reference examples/reference_fallout_shelter.csv
```

**预期结果**:
- ✅ 从参考文件提取术语表
- ✅ 翻译时使用术语表保持一致性
- ✅ "Deathclaw" 应翻译为 "死爪"
- ✅ "Stimpak" 应翻译为 "治疗针"
- ✅ "Dweller" 应翻译为 "居民"
- ✅ 占位符 `{0}`, `{1}`, `%d` 被保留
- ✅ 富文本标签 `<color=red>` 被保留

---

### 测试 3: 占位符保留验证

**检查点**:
1. 打开 `to_translate_new_content_translated.csv`
2. 查找包含占位符的行,例如:
   - 原文: `"Excellent work, {0}! You've earned {1} CAPS."`
   - 译文应该是: `"干得好，{0}!你获得了{1}个瓶盖。"` (占位符位置可能变化,但格式不变)

3. 查找 Printf 风格占位符:
   - 原文: `"Current Vault population: %d Dwellers"`
   - 译文应该是: `"当前避难所人口:%d 个居民"` (`%d` 保留)

4. 查找富文本标签:
   - 原文: `"<color=yellow>Caution:</color> Radiation levels are high!"`
   - 译文应该是: `"<color=yellow>注意:</color>辐射水平过高!"` (标签保留)

---

### 测试 4: QA 报告验证

打开 `to_translate_new_content_qa_report.json`,检查:

```json
{
  "total_rows": 20,
  "passed": 18,
  "warnings": 2,
  "critical_errors": 0,
  "issues": [
    {
      "row": 5,
      "type": "length_variance",
      "severity": "warning",
      "message": "Target text 160% longer than source"
    }
  ]
}
```

**验证点**:
- ✅ `critical_errors` 应该为 0
- ✅ `passed` 应该接近 `total_rows`
- ✅ `warnings` 可能有少量 (长度差异等)

---

### 测试 5: 防循环机制验证

**测试方法**:
1. 运行翻译命令
2. 观察脚本是否在完成后立即停止
3. 确认不会自动"优化"或"再次翻译"

**预期行为**:
```
✅ Translation complete!
   Output file: examples/to_translate_new_content_translated.csv
   QA report: examples/to_translate_new_content_qa_report.json

   Total rows: 20
   Passed: 18
   Warnings: 2
   Critical errors: 0

Ready for your review.
To make adjustments, please re-run with different parameters.

[脚本退出,不再执行任何操作]
```

---

## 📊 预期术语表提取结果

从 `reference_fallout_shelter.csv` 应该提取到:

| English | Chinese | Frequency |
|---------|---------|-----------|
| Vault | 避难所 | 3 |
| Dweller | 居民 | 3 |
| Stimpak | 治疗针 | 3 |
| RadAway | 消辐宁 | 3 |
| Deathclaw | 死爪 | 2 |
| Wasteland | 废土 | 2 |
| CAPS | 瓶盖 | 2 |
| Quest | 任务 | 2 |

---

## ✅ 验收标准

### 必须通过 (Critical)
- [ ] 脚本成功运行,无崩溃
- [ ] 生成翻译文件 (.csv)
- [ ] 生成 QA 报告 (.json)
- [ ] 所有占位符被保留 (数量和格式)
- [ ] 脚本在完成后停止,不循环
- [ ] Exit code 为 0 (成功)

### 应该通过 (Important)
- [ ] 使用参考文件时,术语翻译一致
- [ ] QA 报告中 critical_errors = 0
- [ ] 富文本标签被正确保留
- [ ] 输出文件格式与输入相同 (CSV)

### 可选优化 (Nice to have)
- [ ] 翻译质量高,语句通顺
- [ ] QA warnings 数量少
- [ ] 长度差异在合理范围内

---

## 🐛 常见问题排查

### 问题 1: 脚本报错 "ModuleNotFoundError"
**原因**: 缺少 Python 依赖
**解决**: 
```powershell
pip install pandas openpyxl
```

### 问题 2: 占位符丢失
**检查**: 查看 QA 报告的 `critical_errors`
**预期**: 应该为 0,如果不为 0 说明占位符保留逻辑有问题

### 问题 3: 脚本不停止,持续运行
**原因**: 缺少明确的 exit 逻辑
**检查**: `scripts/translate.py` 是否有 `sys.exit(0)`

### 问题 4: 术语不一致
**原因**: 术语表未正确提取或应用
**检查**: 
- 参考文件路径是否正确
- 术语频率是否达到阈值 (默认 2)

---

## 🎯 下一步

测试通过后,您可以:
1. **实现实际翻译逻辑** - 集成 LLM API
2. **完善 QA 检查** - 添加更多验证规则
3. **添加更多测试用例** - 覆盖边界情况
4. **优化性能** - 批量翻译,缓存等

---

## 📝 测试报告模板

完成测试后,请记录:

```markdown
## 测试结果

- 测试日期: ____
- 测试人: ____

### 测试 1: 基础翻译
- [ ] 通过 / [ ] 失败
- 备注: ____

### 测试 2: 术语表提取
- [ ] 通过 / [ ] 失败
- 备注: ____

### 测试 3: 占位符保留
- [ ] 通过 / [ ] 失败
- 备注: ____

### 测试 4: QA 报告
- [ ] 通过 / [ ] 失败
- 备注: ____

### 测试 5: 防循环机制
- [ ] 通过 / [ ] 失败
- 备注: ____

### 总体评价
- 整体质量: ⭐⭐⭐⭐⭐
- 建议改进: ____
```

---

现在您可以开始测试了! 🚀

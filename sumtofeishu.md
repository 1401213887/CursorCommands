---
description: 总结当前对话，生成技术文档并上传至飞书云文档
---

# 任务

请按照以下步骤执行：

## 第一步：总结对话

回顾本次对话的完整内容，提取以下信息：

1. **问题背景**：用户面临的问题或需求是什么
2. **分析过程**：进行了哪些关键的分析、调查或探索
3. **解决方案**：最终采取了什么方案，做了哪些代码修改
4. **关键代码变更**：列出修改的文件和核心改动（用代码块展示关键 diff）
5. **结论与经验**：总结经验教训或注意事项

## 第二步：生成技术文档

根据总结内容，在工作区根目录生成一个 Markdown 技术文档。

**文件命名规则：**
- 格式：`AiDoc_<具体问题描述>_<YYYYMMDD>.md`
- `<具体问题描述>` 应精确反映本次对话解决的核心问题，使用英文短语，用驼峰命名法
- 命名必须具有**唯一性和辨识度**，避免泛化（如不要用 "BugFix"、"CodeChange" 这类模糊名称）
- 好的示例：`AiDoc_LandscapeGrassCrashNullPtr_20260210.md`、`AiDoc_SkeletalMeshLODSwitchStutter_20260210.md`
- 不好的示例：`AiDoc_Fix_20260210.md`、`AiDoc_Update_20260210.md`
- 创建前先检查工作区根目录是否已存在同名文件，若存在则在文件名末尾追加序号（如 `_2`、`_3`）

文档模板如下：

```markdown
# <主题标题>

> 日期：<YYYY-MM-DD>
> 作者：DjangoZjgPku (AI 辅助)

## 背景

<问题背景描述>

## 分析过程

<分析过程，包含关键发现>

## 解决方案

<具体方案描述>

## 关键代码变更

<涉及的文件和代码改动>

## 总结

<经验教训和注意事项>
```

## 第三步：上传至飞书

**重要：由于 Windows PowerShell 中文编码问题，必须通过 JSON 配置文件传参。**

1. 在工作区根目录创建临时配置文件 `_feishu_upload_config.json`：

```json
{
    "file": "生成的文档绝对路径",
    "title": "文档标题（取自 Markdown 一级标题）",
    "folder": "LftxfwYm3lttjjdtO3DcscIEncA",
    "raw": true
}
```

2. 执行上传脚本：

```powershell
$env:PYTHONIOENCODING="utf-8"; python d:\GR_release\Tools\feishu_upload.py --json d:\GR_release\_feishu_upload_config.json
```

3. 上传完成后清理临时文件：
   - 删除 `_feishu_upload_config.json`
   - 删除工作区根目录下生成的 `AiDoc_*.md` 文档

4. 确认结果，向用户报告：
   - 文档标题
   - 飞书文档链接
   - 文档包含的主要内容概要

## 注意事项

- 文档语言使用**中文**
- 代码块保持原始语言（C++、Python 等）
- 敏感信息（Token、密码等）**不要**包含在文档中
- **不要**直接在命令行传中文参数，必须通过 JSON 配置文件传参
- 如果对话中没有实质性的技术内容，告知用户并询问是否仍要生成
- 如果上传失败提示权限问题，提醒用户在飞书中将应用 CursorFeishuDoc 添加为该文件夹的协作者

---
description: 将指定文档上传至飞书云文档文件夹
---

# 任务

将用户指定的文档文件上传至飞书云文档。

**默认目标文件夹：** https://sarosgame.feishu.cn/drive/folder/LftxfwYm3lttjjdtO3DcscIEncA

## 执行步骤

### 第一步：确认上传文件

1. 确认用户指定的文档文件路径，支持以下情况：
   - 用户通过 `@` 引用的文件
   - 用户在对话中提到的文件路径
   - 当前打开的文件
2. 确认文件存在且为文本文件（.md、.txt、.json 等）

### 第二步：生成 JSON 配置并上传

**重要：由于 Windows PowerShell 中文编码问题，必须通过 JSON 配置文件传参。**

1. 在工作区根目录创建临时配置文件 `_feishu_upload_config.json`：

```json
{
    "file": "文件的绝对路径",
    "title": "文档标题（可选，默认用文件名）",
    "folder": "LftxfwYm3lttjjdtO3DcscIEncA"
}
```

2. 执行上传脚本：

```
python d:\GR_release\Tools\feishu_upload.py --json d:\GR_release\_feishu_upload_config.json
```

3. 上传完成后删除临时配置文件 `_feishu_upload_config.json`

### 第三步：确认结果

1. 检查命令输出是否包含"创建成功"和文档链接
2. 向用户报告上传结果，包含：
   - 文档标题
   - 飞书文档链接

## 注意事项

- **不要**直接在终端执行 feishu-docx 命令（Windows 下中文编码会乱码）
- **不要**直接在命令行传中文参数，必须通过 JSON 配置文件传参
- 如果用户想上传到其他文件夹，让用户提供飞书文件夹 URL，从中提取 folder token，填入 JSON 的 folder 字段
- 如果上传失败提示权限问题，提醒用户在飞书中将应用 CursorFeishuDoc 添加为该文件夹的协作者

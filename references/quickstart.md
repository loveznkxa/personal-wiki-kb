# 快速启动指南

## 1. 创建目录结构
在项目根目录下执行：

```bash
mkdir -p raw wiki/entities wiki/topics wiki/comparisons wiki/archive
touch wiki/index.md wiki/log.md
```

## 2. 初始化索引和日志
- 在 `wiki/index.md` 中写入：

  # 知识库索引

  （空知识库，等待首次摄入）

- `wiki/log.md` 保持为空，系统会自动追加操作记录。

## 3. 首次摄入
1. 将你的原始资料（`.md` 或 `.json`）放入 `raw/` 目录
2. 对 AI 说：**“请根据 raw/ 下的文件，执行 ingest_source 模块”**
3. AI 会分析并给出创建/更新页面计划，收到报告后回复 **Y** 确认写入

## 4. 开始查询与自动回写
- 提问示例：**“帮我查一下 Wiki 中关于 XXX 的信息”**
- 如果 AI 的答案产生了跨文档新结论，它会自动起草新页面并请你确认回写（Y/N）

## 5. 定期健康检查
- 建议每摄入 10 个文件或每 30 天执行一次：**“运行 audit_wiki”**
- 审计报告会列出问题、建议行动，并询问你是否执行归档等操作

## 下一步
- 自定义知识类型：参考 `schema-extensions/` 目录（若有示例文件）
- 遇到问题：查阅 `references/troubleshooting.md` 或直接对 AI 描述现象
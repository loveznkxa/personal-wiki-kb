# 自定义知识类型示例：食谱 (Recipe)

> 本文件演示如何为项目添加新的知识分类。Schema 扩展允许系统在摄入时自动识别并结构化特定领域知识。

## 类型定义
- **类型名**: `recipe`
- **页面目录**: `wiki/recipes/`
- **索引章节**: `## 📖 食谱 (Recipes)`
- **触发识别**: 当源文件标题或内容包含 `菜谱`、`配方`、`食材`、`烹饪` 等关键词，或 YAML frontmatter 中声明 `type: recipe` 时，摄入模块自动归类到此类型。

## 字段结构
每个食谱页面必须按以下模板生成：

\`\`\`markdown
---
type: recipe
created: YYYY-MM-DD
confidence: high
source: raw/xxx.md
---

# 菜品名称

> 一句话口味/特色描述

## 食材
- 材料 A (用量)
- 材料 B (用量)

## 步骤
1. 第一步...
2. 第二步...

## 来源
- [[raw/xxx.md]]
\`\`\`

## 索引规则
在 `wiki/index.md` 中添加新章节：

\`\`\`markdown
## 📖 食谱 (Recipes)
- [宫保鸡丁](recipes/gongbao-chicken.md) — 家常川菜 `[置信度:高]`
\`\`\`

## 摄入自动识别逻辑
1. 检查源文件是否包含 `type: recipe`（YAML frontmatter） → 强制归类
2. 否则，扫描内容关键词（`食材`、`烹饪步骤` 等） → 建议归类
3. 匹配到食谱类型后，按模板生成页面并放入 `wiki/recipes/`
4. 索引更新时使用 `📖 食谱` 章节

## 扩展练习
- 可由此衍生出 `cocktail`（鸡尾酒）、`exercise`（健身动作）等任何需要固定格式的类型。
- 只需复制本文件，修改类型名、字段和关键词即可。
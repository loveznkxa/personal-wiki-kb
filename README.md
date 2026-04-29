# 个人 Wiki 知识库 (Personal Wiki KB)

## 概述
将知识管理从一次性的"信息存储"升级为持续的"知识工程"。系统像编译器一样工作：**Raw Sources (源文件) → Compiled Wiki (结构化知识网络)**。

### 核心概念
- **Raw Sources** (`raw/` 目录): 原始输入材料，**不可修改 (Immutable)**，是信息的"真相之源"
- **The Wiki** (`wiki/` 目录): LLM 主动生成的 Markdown 文件集合，包括实体页、主题总结页、对比分析页
- **The Schema**: 定义页面格式、更新逻辑、冲突处理、版本规则的元规则集
- **Schema 扩展点**: 用户可通过 `schema-extensions/` 目录自定义新知识类型

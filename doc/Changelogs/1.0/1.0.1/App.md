# 📝 版本更新日志
## [version-1.0.0] - 2026-04-13
**完整版本号：** 1.0.1
**Git Tag：** v1.0.1

### ✨ 新增功能
- 🔧 新增 changelog 工作流中的 PAT 认证处理逻辑
- 🛠️ 为 changelog 工作流添加权限配置
- 📅 增强 changelog 生成器，支持动态日期和版本占位符

### 🐛 问题修复
- 🔍 修复工作流中重复的 token 配置项
- 🔑 修复 PAT 认证步骤中缺失的 `with` 键
- 🔄 修复 changelog 工作流中检出步骤的顺序问题

### 📚 文档更新
- 📖 更新 README 中的先决条件和环境变量说明

### 🔧 配置与构建
- 🏗️ 为 Python 和构建产物添加 .gitignore 文件
- 🔐 为 changelog 工作流添加 token 密钥配置
- ⚙️ 在 changelog 工作流中添加 `continue-on-error` 配置项
- 🤖 为 v1.0.0 版本自动生成 changelog
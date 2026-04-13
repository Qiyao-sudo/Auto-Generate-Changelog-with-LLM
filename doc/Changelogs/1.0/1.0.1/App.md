# 📝 版本更新日志
## [version-1.0.0] - 2026-04-13
**完整版本号：** 1.0.1
**Git Tag：** v1.0.1

### ✨ 新增功能
- 🔧 新增 changelog 生成工作流的 PAT 认证处理逻辑
- 📅 增强 changelog 生成器，支持动态日期和版本占位符替换
- 🔐 为 changelog 工作流添加权限配置

### 🐛 问题修复
- 🧹 移除工作流中重复的 token 配置项
- 🔑 修复 PAT 认证步骤中缺失的 `with` 关键字
- 🔄 修正 changelog 工作流中检出步骤的顺序

### 🚀 功能优化
- 📄 更新 README 中的先决条件和环境变量说明

### 🔧 持续集成
- 🔐 为 changelog 工作流添加 token 密钥
- 🛡️ 为 changelog 工作流添加 `continue-on-error` 配置
- 🔐 为 changelog 工作流添加 PAT token

### 📦 依赖更新
- 🗂️ 新增针对 Python 和构建产物的 .gitignore 文件
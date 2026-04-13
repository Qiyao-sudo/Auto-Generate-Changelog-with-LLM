# 📝 版本更新日志
## [version-1.0.0] - 2026-04-13

### ✨ 新增功能
- 🔧 新增 changelog 生成器，支持动态日期和版本占位符替换
- 🔐 为 changelog 工作流添加 PAT 认证处理逻辑
- 🔑 为 changelog 工作流添加权限配置

### 🐛 问题修复
- 🔧 修复 changelog 工作流中重复的令牌配置
- 🔑 修复 PAT 认证步骤中缺失的 `with` 关键字
- 🔄 修复 changelog 工作流中检出步骤的顺序问题
- 📂 修复 changelog 模板的绝对路径引用问题

### 🚀 功能优化
- 📄 更新 README 中的先决条件和环境变量说明
- 📄 更新 PR 说明中的仓库 URL

### 🔄 代码重构
- 🧹 添加 .gitignore 文件，忽略 Python 和构建产物

### 📦 依赖更新
- ⚙️ 为 changelog 工作流添加令牌密钥
- ⚙️ 为 changelog 工作流添加 `continue-on-error` 配置
- 🧹 从 changelog 工作流中移除 PAT 凭据步骤
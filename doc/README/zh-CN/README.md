# 🚀 使用LLM自动生成Changelog

🌐 **Languages**: [English](doc/README/en-US/README.md) | [中文](doc/README/zh-CN/README.md) | [日本語](doc/README/ja-JP/README.md) | [Deutsch](doc/README/de-DE/README.md) | [Español](doc/README/es-ES/README.md) | [Русский](doc/README/ru-RU/README.md) | [العربية](doc/README/ar-SA/README.md) | [繁體中文](doc/README/zh-TW/README.md)

一个基于 GitHub Actions 的自动化工具，利用 DeepSeek 等大语言模型（LLM）自动生成规范、结构化的版本更新日志（Changelog）。无需手动编写，只需触发工作流，即可获得专业级别的更新文档。

## ✨ 功能特性

- 🤖 **智能分析**：基于 LLM 智能分析 Git 提交记录，自动归类为新增功能、性能优化、Bug 修复等类别
- 🏷️ **自动标签管理**：支持自动创建 Git Tag，并智能计算版本区间差异
- 📁 **结构化存储**：按主版本、子版本分层存储生成的 Changelog 文档
- 🎨 **专业模板**：提供标准化的 Markdown 模板，输出格式美观统一
- 🔧 **高度可配置**：支持自定义 LLM API、模型、提示词模板等参数
- ⚡ **一键触发**：通过 GitHub Actions 手动触发，输入版本号即可自动生成

## 🚀 快速开始

### 前提条件

1. **GitHub 仓库**：已启用 GitHub Actions
2. **LLM API 密钥**：DeepSeek 或其他兼容 OpenAI API 的 LLM 服务 API Key
3. **Python 环境**：GitHub Actions 中的 Ubuntu 环境（已自动配置）

### 安装步骤

1. **复制工作流文件**：将 `.github/workflows/generate-changelog.yml` 复制到你的仓库相同路径
2. **复制脚本文件**：将 `.github/workflows/scripts/` 目录复制到你的仓库
3. **配置 Secrets**：在仓库 Settings → Secrets and variables → Actions 中添加以下 Secrets：
   - `LLM_API_KEY`: 你的 LLM API 密钥
   - （可选）`LLM_API_ENDPOINT`: LLM API 端点，默认使用 DeepSeek
   - （可选）`LLM_API_MODEL`: 使用的模型名称，默认 `deepseek-chat`

## 📖 使用方法

### 手动触发工作流

1. 进入 GitHub 仓库的 **Actions** 页面
2. 选择 **Auto Generate Changelog with DeepSeek** 工作流
3. 点击 **Run workflow** 按钮
4. 填写以下参数：
   - **main_version**: 主版本号（如 `1.X`）
   - **sub_version**: 完整版本号（如 `1.X.X`）
   - **current_tag**: 当前 Git Tag 名称（如 `v1.X.X`，不存在则会自动创建）

### 工作流执行过程

1. **检出代码**：拉取完整 Git 历史记录和所有 Tags
2. **标签处理**：检查指定 Tag 是否存在，不存在则自动创建
3. **提交差异分析**：智能计算与上一个版本之间的提交差异
4. **LLM 生成**：调用 DeepSeek API 生成结构化 Changelog
5. **文件保存**：将生成的文档保存到 `doc/Changelogs/{主版本}/{子版本}/App.md`
6. **自动提交**：将生成的 Changelog 文件提交到仓库
7. **标签推送**：如为新创建 Tag，则自动推送到远程仓库

## ⚙️ 配置说明

### 环境变量配置

在 GitHub Secrets 中配置以下变量：

| 变量名 | 必填 | 默认值 | 说明 |
|--------|------|--------|------|
| `LLM_API_KEY` | ✅ | 无 | LLM API 密钥 |
| `LLM_API_ENDPOINT` | ❌ | `https://api.deepseek.com/chat/completions/` | API 端点地址 |
| `LLM_API_MODEL` | ❌ | `deepseek-chat` | 使用的模型名称 |

### 自定义提示词模板

如需修改输出格式，可编辑 `.github/workflows/scripts/template.txt` 文件：

```txt
你是一名软件开发文档工程师，请根据以下Git提交记录，生成一份规范、易读、结构化的版本更新日志Changelog：
要求：
1. 输出使用Markdown格式，适配App项目文档风格
2. 分类整理：新增功能、性能优化、Bug修复、代码重构、依赖更新
3. 语言简洁正式，剔除无效merge、wip提交描述
4. 头部标注主版本号、完整版本号、当前Tag、更新日期
5. 最终只输出纯Markdown正文，不要额外解释、不要开场白
...
```

### 输出文件路径

生成的 Changelog 文件路径格式为：
```
doc/Changelogs/{main_version}/{sub_version}/App.md
```

例如，`main_version=1`, `sub_version=1.2.3` 时，文件路径为：
```
doc/Changelogs/1/1.2.3/App.md
```

## 📁 项目结构

```
.github/
├── workflows/
│   ├── generate-changelog.yml    # GitHub Actions 工作流定义
│   └── scripts/
│       ├── gen_changelog.py      # 主生成脚本
│       └── template.txt          # LLM 提示词模板
README.md                         # 项目说明文档
```

### 核心文件说明

- **generate-changelog.yml**: GitHub Actions 工作流定义，包含完整的自动生成流程
- **gen_changelog.py**: Python 脚本，负责读取提交记录、调用 LLM API、保存生成结果
- **template.txt**: 提示词模板，控制 LLM 输出格式和内容结构

## 🎯 使用示例

### 示例工作流触发

1. **输入参数**：
   - main_version: `1`
   - sub_version: `1.2.0`
   - current_tag: `v1.2.0`

2. **执行结果**：
   - 自动创建 Tag `v1.2.0`（如不存在）
   - 生成 `doc/Changelogs/1/1.2.0/App.md` 文件
   - 自动提交生成的文件到仓库

### 生成的 Changelog 示例

```markdown
# 📝 版本更新日志
## [v1.2.0] - 2026-04-13

### ✨ 新增功能
- 🌓 新增明暗主题切换功能，优化界面视觉体验
- 🎨 新增画板颜色切换逻辑，支持随主题动态适配

### 🐛 问题修复
- 🔧 修复跨平台后端启动命令兼容性问题
- 📂 修正 app.py 中错误的路径指向配置

### 🚀 功能优化
- 🎭 统一界面硬编码颜色值为主题变量，提升视觉风格一致性
- 🎛️ 调整语言切换图标位置，优化操作交互逻辑
```

## ⚠️ 注意事项

1. **API 调用成本**：使用 LLM API 可能会产生费用，请确保了解所用服务的计费方式
2. **网络稳定性**：需要确保 GitHub Actions 可以访问配置的 LLM API 端点
3. **提交记录质量**：生成的 Changelog 质量取决于提交信息的清晰度和完整性
4. **标签命名规范**：建议使用语义化版本命名，如 `v1.0.0`、`v2.1.3` 等
5. **权限要求**：工作流需要写入仓库的权限，请确保 GitHub Actions 有足够的权限

## ❓ 常见问题

### Q1: 如果没有上一个版本 Tag，会发生什么？
A: 工作流设计了智能回退机制。如果找不到上一个有效 Tag，会自动使用最近的 20 条提交记录作为生成依据，确保始终能生成 Changelog。

### Q2: 可以使用其他 LLM 服务吗？
A: 可以。本项目兼容任何提供 OpenAI 格式 API 的 LLM 服务。只需在 Secrets 中配置对应的 `LLM_API_ENDPOINT` 和 `LLM_API_MODEL` 即可。

### Q3: 生成的 Changelog 文件会提交到哪个分支？
A: 默认提交到触发工作流时所在的分支（通常为 `main` 分支）。工作流配置中已指定 `ref: main`，确保在主分支操作。

### Q4: 如何修改 Changelog 的分类方式？
A: 编辑 `.github/workflows/scripts/template.txt` 文件中的提示词，调整分类要求即可。例如，可以增加「安全更新」「文档改进」等分类。

### Q5: API 调用失败怎么办？
A: GitHub Actions 会自动显示错误日志。常见原因包括：API 密钥无效、网络无法访问 API 端点、API 响应格式不符等。请检查 Secrets 配置和网络连通性。

### Q6: 可以同时生成多个版本的 Changelog 吗？
A: 可以。每次手动触发工作流时输入不同的版本号参数，即可为不同版本生成独立的 Changelog 文件。

### Q7: 为什么需要拉取完整的 Git 历史记录？
A: 完整的 Git 历史记录（`fetch-depth: 0`）是为了确保能准确计算 Tag 之间的提交差异。这是生成精确版本变更日志的基础。

## 🔄 自定义扩展

### 支持其他 LLM 服务

如需切换其他 LLM 服务（如 OpenAI、Claude 等），只需修改以下配置：

1. 更新 `LLM_API_ENDPOINT` 为对应服务的 API 地址
2. 更新 `LLM_API_MODEL` 为对应模型名称
3. 确保 API 响应格式与 DeepSeek 兼容（返回 `choices[0].message.content`）

### 修改输出格式

编辑 `template.txt` 文件可以完全自定义输出格式，例如：
- 调整分类方式
- 修改表情符号
- 添加自定义章节
- 改变文档风格

## 📄 许可证

本项目采用 MIT 许可证。详见 [LICENSE](https://github.com/Qiyao-sudo/Auto-Generate-Changelog-with-LLM/blob/main/LICENSE) 文件。

## 🤝 贡献指南

欢迎提交 Issue 和 Pull Request 来改进这个项目！

1. Fork 本仓库
2. 创建功能分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add Changelog generation feature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启一个 Pull Request

## 🙏 致谢

- 感谢 [DeepSeek](https://www.deepseek.com/) 提供优秀的 LLM 服务
- 感谢 GitHub Actions 提供的强大自动化能力
- 感谢所有开源社区的贡献者

---

**如果这个项目对你有帮助，请给个 ⭐ Star 支持一下！**
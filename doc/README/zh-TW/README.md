# 🚀 使用LLM自動生成Changelog

🌐 **Languages**: [English](doc/README/en-US/README.md) | [中文](doc/README/zh-CN/README.md) | [日本語](doc/README/ja-JP/README.md) | [Deutsch](doc/README/de-DE/README.md) | [Español](doc/README/es-ES/README.md) | [Русский](doc/README/ru-RU/README.md) | [العربية](doc/README/ar-SA/README.md) | [繁體中文](doc/README/zh-TW/README.md)

一個基於 GitHub Actions 的自動化工具，利用 DeepSeek 等大語言模型（LLM）自動生成規範、結構化的版本更新日誌（Changelog）。無需手動編寫，只需觸發工作流，即可獲得專業級別的更新文檔。

## ✨ 功能特性

- 🤖 **智能分析**：基於 LLM 智能分析 Git 提交記錄，自動歸類為新增功能、性能優化、Bug 修復等類別
- 🏷️ **自動標籤管理**：支持自動創建 Git Tag，並智能計算版本區間差異
- 📁 **結構化存儲**：按主版本、子版本分層存儲生成的 Changelog 文檔
- 🎨 **專業模板**：提供標準化的 Markdown 模板，輸出格式美觀統一
- 🔧 **高度可配置**：支持自定義 LLM API、模型、提示詞模板等參數
- ⚡ **一鍵觸發**：通過 GitHub Actions 手動觸發，輸入版本號即可自動生成

## 🚀 快速開始

### 前提條件

1. **GitHub 倉庫**：已啟用 GitHub Actions
2. **LLM API 密鑰**：DeepSeek 或其他兼容 OpenAI API 的 LLM 服務 API Key
3. **Python 環境**：GitHub Actions 中的 Ubuntu 環境（已自動配置）

### 安裝步驟

1. **複製工作流文件**：將 `.github/workflows/generate-changelog.yml` 複製到你的倉庫相同路徑
2. **複製腳本文件**：將 `.github/workflows/scripts/` 目錄複製到你的倉庫
3. **配置 Secrets**：在倉庫 Settings → Secrets and variables → Actions 中添加以下 Secrets：
   - `LLM_API_KEY`: 你的 LLM API 密鑰
   - （可選）`LLM_API_ENDPOINT`: LLM API 端點，默認使用 DeepSeek
   - （可選）`LLM_API_MODEL`: 使用的模型名稱，默認 `deepseek-v4-flash`

## 📖 使用方法

### 手動觸發工作流

1. 進入 GitHub 倉庫的 **Actions** 頁面
2. 選擇 **Auto Generate Changelog with DeepSeek** 工作流
3. 點擊 **Run workflow** 按鈕
4. 填寫以下參數：
   - **main_version**: 主版本號（如 `1.X`）
   - **sub_version**: 完整版本號（如 `1.X.X`）
   - **current_tag**: 當前 Git Tag 名稱（如 `v1.X.X`，不存在則會自動創建）

### 工作流執行過程

1. **檢出代碼**：拉取完整 Git 歷史記錄和所有 Tags
2. **標籤處理**：檢查指定 Tag 是否存在，不存在則自動創建
3. **提交差異分析**：智能計算與上一個版本之間的提交差異
4. **LLM 生成**：調用 DeepSeek API 生成結構化 Changelog
5. **文件保存**：將生成的文檔保存到 `doc/Changelogs/{主版本}/{子版本}/App.md`
6. **自動提交**：將生成的 Changelog 文件提交到倉庫
7. **標籤推送**：如為新創建 Tag，則自動推送到遠程倉庫

## ⚙️ 配置說明

### 環境變量配置

在 GitHub Secrets 中配置以下變量：

| 變量名 | 必填 | 默認值 | 說明 |
|--------|------|--------|------|
| `LLM_API_KEY` | ✅ | 無 | LLM API 密鑰 |
| `LLM_API_ENDPOINT` | ❌ | `https://api.deepseek.com/chat/completions/` | API 端點地址 |
| `LLM_API_MODEL` | ❌ | `deepseek-v4-flash` | 使用的模型名稱 |

### 自定義提示詞模板

如需修改輸出格式，可編輯 `.github/workflows/scripts/template.txt` 文件：

```txt
你是一名軟件開發文檔工程師，請根據以下Git提交記錄，生成一份規範、易讀、結構化的版本更新日誌Changelog：
要求：
1. 輸出使用Markdown格式，適配App專案文檔風格
2. 分類整理：新增功能、性能優化、Bug修復、代碼重構、依賴更新
3. 語言簡潔正式，剔除無效merge、wip提交描述
4. 頭部標注主版本號、完整版本號、當前Tag、更新日期
5. 最終只輸出純Markdown正文，不要額外解釋、不要開場白
...
```

### 輸出文件路徑

生成的 Changelog 文件路徑格式為：
```
doc/Changelogs/{main_version}/{sub_version}/App.md
```

例如，`main_version=1`, `sub_version=1.2.3` 時，文件路徑為：
```
doc/Changelogs/1/1.2.3/App.md
```

## 📁 專案結構

```
.github/
├── workflows/
│   ├── generate-changelog.yml    # GitHub Actions 工作流定義
│   └── scripts/
│       ├── gen_changelog.py      # 主生成腳本
│       └── template.txt          # LLM 提示詞模板
README.md                         # 專案說明文檔
```

### 核心文件說明

- **generate-changelog.yml**: GitHub Actions 工作流定義，包含完整的自動生成流程
- **gen_changelog.py**: Python 腳本，負責讀取提交記錄、調用 LLM API、保存生成結果
- **template.txt**: 提示詞模板，控制 LLM 輸出格式和內容結構

## 🎯 使用示例

### 示例工作流觸發

1. **輸入參數**：
   - main_version: `1`
   - sub_version: `1.2.0`
   - current_tag: `v1.2.0`

2. **執行結果**：
   - 自動創建 Tag `v1.2.0`（如不存在）
   - 生成 `doc/Changelogs/1/1.2.0/App.md` 文件
   - 自動提交生成的文件到倉庫

### 生成的 Changelog 示例

```markdown
# 📝 版本更新日誌
## [v1.2.0] - 2026-04-13

### ✨ 新增功能
- 🌓 新增明暗主題切換功能，優化界面視覺體驗
- 🎨 新增畫板顏色切換邏輯，支持隨主題動態適配

### 🐛 問題修復
- 🔧 修復跨平台後端啟動命令兼容性問題
- 📂 修正 app.py 中錯誤的路徑指向配置

### 🚀 功能優化
- 🎭 統一界面硬編碼顏色值為主題變數，提升視覺風格一致性
- 🎛️ 調整語言切換圖標位置，優化操作交互邏輯
```

## ⚠️ 注意事項

1. **API 調用成本**：使用 LLM API 可能會產生費用，請確保了解所用服務的計費方式
2. **網路穩定性**：需要確保 GitHub Actions 可以訪問配置的 LLM API 端點
3. **提交記錄質量**：生成的 Changelog 質量取決於提交信息的清晰度和完整性
4. **標籤命名規範**：建議使用語義化版本命名，如 `v1.0.0`、`v2.1.3` 等
5. **權限要求**：工作流需要寫入倉庫的權限，請確保 GitHub Actions 有足夠的權限

## ❓ 常見問題

### Q1: 如果沒有上一個版本 Tag，會發生什麼？
A: 工作流設計了智能回退機制。如果找不到上一個有效 Tag，會自動使用最近的 20 條提交記錄作為生成依據，確保始終能生成 Changelog。

### Q2: 可以使用其他 LLM 服務嗎？
A: 可以。本專案兼容任何提供 OpenAI 格式 API 的 LLM 服務。只需在 Secrets 中配置對應的 `LLM_API_ENDPOINT` 和 `LLM_API_MODEL` 即可。

### Q3: 生成的 Changelog 文件會提交到哪個分支？
A: 默認提交到觸發工作流時所在的分支（通常為 `main` 分支）。工作流配置中已指定 `ref: main`，確保在主分支操作。

### Q4: 如何修改 Changelog 的分類方式？
A: 編輯 `.github/workflows/scripts/template.txt` 文件中的提示詞，調整分類要求即可。例如，可以增加「安全更新」「文檔改進」等分類。

### Q5: API 調用失敗怎麼辦？
A: GitHub Actions 會自動顯示錯誤日誌。常見原因包括：API 密鑰無效、網路無法訪問 API 端點、API 響應格式不符等。請檢查 Secrets 配置和網路連通性。

### Q6: 可以同時生成多個版本的 Changelog 嗎？
A: 可以。每次手動觸發工作流時輸入不同的版本號參數，即可為不同版本生成獨立的 Changelog 文件。

### Q7: 為什麼需要拉取完整的 Git 歷史記錄？
A: 完整的 Git 歷史記錄（`fetch-depth: 0`）是為了確保能準確計算 Tag 之間的提交差異。這是生成精確版本變更日誌的基礎。

## 🔄 自定義擴展

### 支持其他 LLM 服務

如需切換其他 LLM 服務（如 OpenAI、Claude 等），只需修改以下配置：

1. 更新 `LLM_API_ENDPOINT` 為對應服務的 API 地址
2. 更新 `LLM_API_MODEL` 為對應模型名稱
3. 確保 API 響應格式與 DeepSeek 兼容（返回 `choices[0].message.content`）

### 修改輸出格式

編輯 `template.txt` 文件可以完全自定義輸出格式，例如：
- 調整分類方式
- 修改表情符號
- 新增自定義章節
- 改變文檔風格

## 📄 許可證

本專案採用 MIT 許可證。詳見 [LICENSE](https://github.com/Qiyao-sudo/Auto-Generate-Changelog-with-LLM/blob/main/LICENSE) 文件。

## 🤝 貢獻指南

歡迎提交 Issue 和 Pull Request 來改進這個專案！

1. Fork 本倉庫
2. 創建功能分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add Changelog generation feature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 開啟一個 Pull Request

## 🙏 致謝

- 感謝 [DeepSeek](https://www.deepseek.com/) 提供優秀的 LLM 服務
- 感謝 GitHub Actions 提供的強大自動化能力
- 感謝所有開源社區的貢獻者

---

**如果這個專案對你有幫助，請給個 ⭐ Star 支持一下！**
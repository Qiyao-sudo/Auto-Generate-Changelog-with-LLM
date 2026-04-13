# 🚀 Auto Generate Changelog with LLM

🌐 **Languages**: [English](doc/README/en-US/README.md) | [中文](doc/README/zh-CN/README.md) | [日本語](doc/README/ja-JP/README.md) | [Deutsch](doc/README/de-DE/README.md) | [Español](doc/README/es-ES/README.md) | [Русский](doc/README/ru-RU/README.md) | [العربية](doc/README/ar-SA/README.md) | [繁體中文](doc/README/zh-TW/README.md)

An automated GitHub Actions tool that uses DeepSeek or other large language models (LLMs) to automatically generate standardized, structured version changelogs. No manual writing required—simply trigger the workflow to obtain professional-level update documentation.

## ✨ Features

- 🤖 **Intelligent Analysis**: Uses LLMs to intelligently analyze Git commit records, automatically categorizing them into new features, performance optimizations, bug fixes, etc.
- 🏷️ **Automatic Tag Management**: Supports automatic Git Tag creation and intelligent calculation of version range differences
- 📁 **Structured Storage**: Stores generated Changelog documents in a hierarchical structure by major and minor versions
- 🎨 **Professional Templates**: Provides standardized Markdown templates for aesthetically uniform output
- 🔧 **Highly Configurable**: Supports custom LLM APIs, models, prompt templates, and other parameters
- ⚡ **One-Click Trigger**: Manual trigger via GitHub Actions, input version numbers to automatically generate

## 🚀 Quick Start

### Prerequisites

1. **GitHub Repository**: GitHub Actions enabled
2. **LLM API Key**: DeepSeek or other OpenAI API-compatible LLM service API Key
3. **Python Environment**: Ubuntu environment in GitHub Actions (automatically configured)

### Installation Steps

1. **Copy workflow file**: Copy `.github/workflows/generate-changelog.yml` to the same path in your repository
2. **Copy script files**: Copy the `.github/workflows/scripts/` directory to your repository
3. **Configure Secrets**: Add the following Secrets in repository Settings → Secrets and variables → Actions:
   - `LLM_API_KEY`: Your LLM API key
   - (Optional) `LLM_API_ENDPOINT`: LLM API endpoint, defaults to DeepSeek
   - (Optional) `LLM_API_MODEL`: Model name to use, defaults to `deepseek-chat`

## 📖 Usage

### Manual Workflow Trigger

1. Go to your GitHub repository's **Actions** page
2. Select the **Auto Generate Changelog with DeepSeek** workflow
3. Click the **Run workflow** button
4. Fill in the following parameters:
   - **main_version**: Major version number (e.g., `1.X`)
   - **sub_version**: Full version number (e.g., `1.X.X`)
   - **current_tag**: Current Git Tag name (e.g., `v1.X.X`, will be automatically created if it doesn't exist)

### Workflow Execution Process

1. **Checkout Code**: Pulls complete Git history and all Tags
2. **Tag Processing**: Checks if the specified Tag exists, creates it automatically if not
3. **Commit Difference Analysis**: Intelligently calculates commit differences from the previous version
4. **LLM Generation**: Calls DeepSeek API to generate structured Changelog
5. **File Saving**: Saves generated document to `doc/Changelogs/{major_version}/{sub_version}/App.md`
6. **Auto Commit**: Commits generated Changelog file to the repository
7. **Tag Push**: If a new Tag was created, automatically pushes it to the remote repository

## ⚙️ Configuration

### Environment Variables Configuration

Configure the following variables in GitHub Secrets:

| Variable Name | Required | Default Value | Description |
|---------------|----------|---------------|-------------|
| `LLM_API_KEY` | ✅ | None | LLM API key |
| `LLM_API_ENDPOINT` | ❌ | `https://api.deepseek.com/` | API endpoint address |
| `LLM_API_MODEL` | ❌ | `deepseek-chat` | Model name to use |

### Custom Prompt Template

To modify output format, edit the `.github/workflows/scripts/template.txt` file:

```txt
You are a software development documentation engineer. Please generate a standardized, readable, and structured version changelog based on the following Git commit records:
Requirements:
1. Output in Markdown format, adapted to App project documentation style
2. Categorize: new features, performance optimizations, bug fixes, code refactoring, dependency updates
3. Language concise and formal, remove invalid merge/wip commit descriptions
4. Header includes major version, full version, current Tag, update date
5. Only output pure Markdown body, no extra explanations, no opening remarks
...
```

### Output File Path

Generated Changelog file path format:
```
doc/Changelogs/{main_version}/{sub_version}/App.md
```

For example, when `main_version=1`, `sub_version=1.2.3`, the file path is:
```
doc/Changelogs/1/1.2.3/App.md
```

## 📁 Project Structure

```
.github/
├── workflows/
│   ├── generate-changelog.yml    # GitHub Actions workflow definition
│   └── scripts/
│       ├── gen_changelog.py      # Main generation script
│       └── template.txt          # LLM prompt template
README.md                         # Project documentation
```

### Core File Descriptions

- **generate-changelog.yml**: GitHub Actions workflow definition, includes complete automatic generation process
- **gen_changelog.py**: Python script, responsible for reading commit records, calling LLM API, saving generated results
- **template.txt**: Prompt template, controls LLM output format and content structure

## 🎯 Usage Example

### Example Workflow Trigger

1. **Input parameters**:
   - main_version: `1`
   - sub_version: `1.2.0`
   - current_tag: `v1.2.0`

2. **Execution results**:
   - Automatically creates Tag `v1.2.0` (if it doesn't exist)
   - Generates `doc/Changelogs/1/1.2.0/App.md` file
   - Automatically commits generated file to repository

### Generated Changelog Example

```markdown
# 📝 Version Changelog
## [v1.2.0] - 2026-04-13

### ✨ New Features
- 🌓 Added light/dark theme switching functionality, optimizing visual experience
- 🎨 Added palette color switching logic, supporting dynamic adaptation with themes

### 🐛 Bug Fixes
- 🔧 Fixed cross-platform backend startup command compatibility issues
- 📂 Corrected incorrect path configuration in app.py

### 🚀 Feature Optimizations
- 🎭 Unified hardcoded color values to theme variables, improving visual consistency
- 🎛️ Adjusted language switch icon position, optimizing interaction logic
```

## ⚠️ Important Notes

1. **API Call Costs**: Using LLM APIs may incur costs, please understand the billing method of your chosen service
2. **Network Stability**: Ensure GitHub Actions can access the configured LLM API endpoint
3. **Commit Record Quality**: Generated Changelog quality depends on the clarity and completeness of commit messages
4. **Tag Naming Convention**: Recommended to use semantic version naming, e.g., `v1.0.0`, `v2.1.3`
5. **Permission Requirements**: Workflow requires write permissions to the repository, ensure GitHub Actions has sufficient permissions

## ❓ Frequently Asked Questions

### Q1: What happens if there's no previous version Tag?
A: The workflow has an intelligent fallback mechanism. If no valid previous Tag is found, it automatically uses the most recent 20 commit records as the basis for generation, ensuring a Changelog can always be generated.

### Q2: Can I use other LLM services?
A: Yes. This project is compatible with any LLM service that provides OpenAI-format API. Simply configure the corresponding `LLM_API_ENDPOINT` and `LLM_API_MODEL` in Secrets.

### Q3: Which branch will the generated Changelog file be committed to?
A: By default, it's committed to the branch where the workflow was triggered (usually the `main` branch). The workflow configuration specifies `ref: main` to ensure operation on the main branch.

### Q4: How can I modify Changelog categorization?
A: Edit the prompt in `.github/workflows/scripts/template.txt` file to adjust categorization requirements. For example, you can add "Security Updates" or "Documentation Improvements" categories.

### Q5: What if API calls fail?
A: GitHub Actions will automatically display error logs. Common causes include: invalid API key, network unable to access API endpoint, incompatible API response format, etc. Check Secrets configuration and network connectivity.

### Q6: Can I generate Changelogs for multiple versions simultaneously?
A: Yes. Each time you manually trigger the workflow with different version parameters, you can generate independent Changelog files for different versions.

### Q7: Why is full Git history required?
A: Complete Git history (`fetch-depth: 0`) is necessary to accurately calculate commit differences between Tags. This is fundamental for generating precise version change logs.

## 🔄 Custom Extensions

### Supporting Other LLM Services

To switch to other LLM services (e.g., OpenAI, Claude, etc.), simply modify the following configurations:

1. Update `LLM_API_ENDPOINT` to the corresponding service's API address
2. Update `LLM_API_MODEL` to the corresponding model name
3. Ensure API response format is compatible with DeepSeek (returns `choices[0].message.content`)

### Modifying Output Format

Editing the `template.txt` file allows complete customization of output format, for example:
- Adjust categorization methods
- Modify emoji symbols
- Add custom sections
- Change documentation style

## 📄 License

This project uses the MIT License. See the [LICENSE](LICENSE) file for details.

## 🤝 Contribution Guidelines

Welcome to submit Issues and Pull Requests to improve this project!

1. Fork this repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 🙏 Acknowledgments

- Thanks to [DeepSeek](https://www.deepseek.com/) for providing excellent LLM services
- Thanks to GitHub Actions for powerful automation capabilities
- Thanks to all contributors in the open-source community

---

**If this project helps you, please give it a ⭐ Star!**
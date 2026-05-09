# 使用 VSCode/Cursor 进行本地 LaTeX 构建

本指南将介绍如何在您的本地计算机上使用 VSCode 或 Cursor 设置并构建 LaTeX 简历。

## 本地构建的优势

- **极速体检**: 直接编译，无容器开销
- **极简体验**: 配置完成后，这是最快的工作流
- **无缝集成**: 享受完整的编辑器特性，包含 PDF 实时预览、语法高亮以及自动补全
- **离线可用**: 编译和构建过程无需连接互联网

## 环境设置流程

### 1. 安装 LaTeX Workshop 插件

1. 打开 VSCode/Cursor 
2. 跳转到扩展 (Extensions) 面板 (Ctrl+Shift+X 或 Cmd+Shift+X)
3. 搜索 "LaTeX Workshop"
4. 点击 "安装" (Install)

### 2. 安装 LaTeX 发行版

您的系统必须安装 LaTeX 发行版（如 TeX Live, MiKTeX, MacTeX 等）。

#### macOS 系统:

```bash
# 使用 Homebrew 安装
brew install texlive
```

#### Windows 系统:
- 安装 [TeX Live](https://tug.org/texlive/windows.html)
- 请确保系统已安装 Perl（某些 LaTeX 宏包需要依赖此环境）

#### Linux 系统:
```bash
# Ubuntu/Debian
sudo apt install texlive-full

# Fedora
sudo dnf install texlive-scheme-full
```

### 3. 配置 VSCode

使用我们的自动配置脚本即可一键配置您的 VSCode 环境：

```bash
python config_vscode_local.py
```

该脚本将自动为您创建包含最优 LaTeX 配置的 `.vscode/settings.json` 文件。

#### 手动配置

如果您更倾向于手动配置，请将以下内容添加到您的 VSCode settings.json 中：

```json
"latex-workshop.latex.tools": [
    {
        "name": "pdflatex",
        "command": "pdflatex",
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "%DOC%"
        ]
    }
],
"latex-workshop.latex.recipes": [
    {
        "name": "pdflatex",
        "tools": [
            "pdflatex"
        ]
    }
],
"latex-workshop.latex.autoBuild.run": "onSave",
"latex-workshop.view.pdf.viewer": "tab",
"latex-workshop.latex.magic.args": [
    "-synctex=1",
    "-interaction=nonstopmode",
    "-file-line-error",
    "%DOC%"
],
"latex-workshop.message.error.show": true,
"latex-workshop.message.warning.show": true
```

## 构建简历

设置完成后，构建操作非常简单直观：

1. 在 VSCode/Cursor 中打开 `main.tex` 文件
2. 执行以下任一操作:
   - 点击 "Build LaTeX" 按钮 (LaTeX Workshop 侧边栏中的绿色播放按钮)
   - 保存文件 (我们的配置文件已默认启用“保存时自动构建”)
   - 使用键盘快捷键 (Ctrl+Alt+B 或 Cmd+Alt+B)
3. 直接在 VSCode 中预览生成的 PDF (推荐使用分屏视图)

## 故障排除

### 常见问题

- **缺少宏包 (Missing Packages)**: 如果您看到关于宏包缺失的错误，请使用您的 LaTeX 发行版包管理器进行安装：
  ```bash
  # 针对 TeX Live
  tlmgr install [宏包名称]
  ```

- **构建错误 (Build Errors)**: 请检查 VSCode 输出面板 (Output) 中的 LaTeX Workshop 日志以获取具体的报错信息。

- **PDF 预览异常**: 如果 PDF 无法正常显示，尝试将查看器更改为浏览器：
  ```json
  "latex-workshop.view.pdf.viewer": "browser"
  ```

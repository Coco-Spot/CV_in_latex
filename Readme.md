[中文](./Readme.md) | [English](./Readme-EN.md)

# LaTeX 简历构建器 (LaTeX CV Builder)

本仓库展示了一个支持**多种构建方法**的 LaTeX 简历模板，旨在为不同平台和工作流提供极高的灵活性与兼容性。

## 为什么需要多种构建方法？

通过多种方式构建同一份文档，可以带来以下几个关键优势：

- **容灾备份**: 如果某一种方法失败，您可以轻松切换到另一种方法
- **跨平台独立性**: 在 Windows、macOS 和 Linux 上均可完美运行
- **灵活性**: 自由选择最适合您当前环境和需求的构建方式
- **团队协作兼容性**: 不同的团队成员可以根据习惯使用不同的方法

## 构建方法对比

| 构建方法 | 优势 | 劣势 | 最佳适用场景 |
|--------------|------------|--------------|----------|
| **本地构建 (Local)** | - 编译速度快<br>- 直接反馈<br>- 无需网络连接<br>- 完整的编辑器集成 | - 需要安装 LaTeX 套件<br>- 需要配置环境<br>- 依赖操作系统 | 日常开发<br>高频修改<br>个人项目 |
| **Docker 构建** | - 环境统一<br>- 本地无需安装 LaTeX<br>- 跨平台兼容<br>- 构建过程可完全复现 | - 必须安装 Docker<br>- 首次启动可能较慢<br>- 容器存在一定性能开销 | 团队项目<br>复杂的文档宏包<br>环境隔离 |
| **GitHub Actions** | - 全自动化运行<br>- 持续集成 (CI)<br>- 自动管理 PDF Release 发布<br>- 本地零配置 | - 必须联网<br>- 不适合高频迭代修改<br>- 依赖 GitHub 平台 | 生产/最终构建<br>公开发布/分享<br>版本控制 |

## 构建方法文档

每种构建方法均附有详细的独立指南：

1. **[本地构建](./Readme-Local.md)** - 在您的本机使用 VSCode/Cursor 以及 LaTeX Workshop 插件直接构建
2. **[Docker 构建](./Readme-Docker.md)** - 在不安装 LaTeX 的情况下通过 Docker 进行构建
3. **[GitHub Actions](./Readme-GitHub-Actions.md)** - 利用 GitHub Actions 实现自动化构建与 Release 发布

## 自动配置脚本

本仓库附带了多款实用脚本以简化初始设置：

- `config_vscode_local.py` - 生成用于本地 LaTeX 开发的 `.vscode/settings.json`
- `config_vscode_devcontainer.py` - 生成基于 Docker 容器开发的 `.devcontainer/devcontainer.json`
- `docker_build.py` - 用于跨平台 Docker 构建的 Python 脚本
- `docker_build.sh` - 用于 Docker 构建的 Shell 脚本（适用于类 Unix 系统）

同时提供 Python 和 Shell 脚本是为了确保在所有平台上的可用性：
- Python 脚本兼容所有平台（Windows, macOS, Linux）
- Shell 脚本在类 Unix 系统中提供近乎原生的运行体验
- 丰富的工具链保证了无论您在哪种环境下，都能成功编译您的简历。

### 使用方法

```bash
# 配置 VSCode 以进行本地构建
python config_vscode_local.py

# 配置 VSCode DevContainer
python config_vscode_devcontainer.py

# 使用 Docker 构建 (Python 脚本 - 跨平台)
python docker_build.py

# 使用 Docker 构建 (Shell 脚本 - 类 Unix 系统)
./docker_build.sh
```

## 示例简历

本仓库默认包含了一份示例简历以供演示和测试。

## 其他模板来源

您也可以在这里探索并发现更多精美的模板：
- https://www.overleaf.com/gallery/tagged/cv

## 项目仓库

https://github.com/reveurmichael/cv_latex

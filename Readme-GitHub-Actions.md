# 使用 GitHub Actions 自动构建简历

本指南将介绍如何利用 GitHub Actions 为您的 LaTeX 简历实现持续集成和自动化的 PDF 文件生成流程。

## GitHub Actions 的优势

- **全自动化**: 每次推送 (Push) 提交代码后，您的简历都将自动在云端进行构建
- **版本控制**: 追踪查阅、打包保存所有发生过的修改与变动记录
- **PDF 自动发布 (Release)**: 自动管理及生成每一个迭代节点的 PDF 发行版本
- **零本地配置**: 在本机开发机上无论您是否安装或运行了 LaTeX 或 Docker 等扩展均不受任何影响
- **CI/CD 持续集成**: 专业的研发工作流环境部署体验

## 工作原理

向工作代码分支 `main`（或 `master`） 推送代码更改时，GitHub Actions 会自动执行以下任务：

1. 在远端云服务器中配置及初始化 LaTeX 编译环境
2. 根据源代码自动编译生成 PDF 格式简历
3. 创建独立用于发行发布的分支 (release branch)，并将生成的 PDF 文档存入其中
4. 利用该生成的 PDF 组装出一份标准的 GitHub Release 发布动态

## 配置及使用方法

整个 GitHub 仓库实际上在初始化阶段已经预先配置好了所有必备的自动化 YAML 通用工作流配置文件： `.github/workflows/build-cv.yml`

### 工作流配置详解

工作流文件的核心模块及代码结构说明如下：

```yml
name: Build LaTeX CV (构建 LaTeX 简历)

on:
  push:
    branches:
      - main
      - master

jobs:
  build-and-release:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - name: Checkout repository (拉取/检出项目仓库代码)
        uses: actions/checkout@v3
        with:
          fetch-depth: 0

      - name: Set up LaTeX (部署 LaTeX 编译环境)
        uses: xu-cheng/latex-action@v2
        with:
          root_file: main.tex

      - name: Prepare PDF for release branch (将编译后的 PDF 归档保存至暂存区)
        run: |
          mkdir -p /tmp/cv_release
          cp main.pdf /tmp/cv_release/main.pdf

      - name: Create release branch with only PDF (切换至独立部署分支并提交最新的单 PDF 简历文件)
        run: |
          git config --local user.email "action@github.com"
          git config --local user.name "GitHub Action"
          git checkout --orphan release
          # 仅保留必需组件，清除其余多余的所有源文件以便存放最后产物
          find . -mindepth 1 -maxdepth 1 ! -name '.git' ! -name '.' -exec rm -rf {} +
          cp /tmp/cv_release/main.pdf .
          git add main.pdf
          git commit -m "Update CV PDF"
          git push -f origin release

      - name: Create GitHub Release (触发系统正式发布更新并生成带版本标签的新 Release)
        id: create_release
        uses: actions/create-release@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          tag_name: v${{ github.run_number }}
          release_name: Release v${{ github.run_number }}
          body: "Automated CV PDF build. (由自动化系统全流程跟进自动生成的简历新版本)"
          draft: false
          prerelease: false

      - name: Upload Release Asset (上传挂载编译成品 PDF 文件)
        uses: actions/upload-release-asset@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          upload_url: ${{ steps.create_release.outputs.upload_url }}
          asset_path: ./main.pdf
          asset_name: main.pdf
          asset_content_type: application/pdf 
```

## 故障排除与日志查询
1. 进入 GitHub 项目主页界面顶部导航栏处的 "Actions" 面板板块
2. 在左列表查找并点击获取最新工作流日志
3. 展开并进入出现红色 `X` 号标记的任务报错区块节点即可进行底层错误回溯溯源排查。

# 基于 Docker 的 LaTeX 简历构建

本指南将介绍如何使用 Docker 来构建 LaTeX 简历，从而免去在本地系统直接安装庞大的 LaTeX 环境。

## Docker 构建的优势

- **环境一致性**: 无论宿主机操作系统为何，编译环境保持绝对一致
- **免安装架构 (Zero LaTeX Installation)**: 无需在本地安装和维护庞大的 LaTeX 套件
- **跨平台支持**: 在 Windows、macOS 和 Linux 上拥有一致的表现
- **依赖管理**: 容器内已预装所有必备依赖及宏包
- **环境隔离**: 编译构建过程绝对安全，不会对您的本地系统产生任何影响

## 准备工作

- 已在系统上正确安装 Docker 及 Docker Desktop
- 对于 Windows 用户，必须启用并配置 WSL 2 支持

## 构建方法

### 方法 1: 使用 Python 脚本 (跨平台推荐)

此 Python 脚本兼容所有主流操作系统并会自动处理错误及异常反馈：

```bash
python docker_build.py
```

### 方法 2: 使用 Shell 脚本 (类 Unix 系统)

对于类 Unix 系统 (如 Linux, macOS)，更推荐使用原生 Shell 脚本：

```bash
./docker_build.sh
```

### 方法 3: 直接使用 Docker Compose

适用于在构建过程中需要进行手动干预与控制的场景：

```bash
# 构建 Docker 镜像
docker compose build

# 运行容器并生成 PDF
docker compose run --rm cv-builder
```

### 方法 4: 直接使用 Docker CLI

如果由于环境原因没有 Docker Compose：

```bash
# 构建 Docker 镜像
docker build -t latex-cv-builder .

# 运行容器并生成 PDF
docker run --rm -v $(pwd):/latex latex-cv-builder
```

以上所有方法在执行成功后，都会在当前目录中生成 `main.pdf` 文件。

## 使用 DevContainer 集成 VSCode 应用

为了享受极致的开发体验，您可以使用 VSCode 的 DevContainer (开发容器) 功能，以纯容器化的交互形式在其中获得包括语言支持、排版预览等在内的全面 LaTeX 功能。

### DevContainer 设置指南

1. 安装必要的 VSCode 扩展:
   - [Dev Containers (开发容器)](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
   - [LaTeX Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop)

2. 运行专用的脚本自动设置开发容器环境:
   ```bash
   python config_vscode_devcontainer.py
   ```

3. 在 VSCode 中重新打开当前项目所在的顶层文件夹

4. 点击左下角的绿色标徽（或按 F1 并选择 "Dev Containers: Reopen in Container / 开发容器: 在容器中重新打开"）

5. VSCode 将自动:
   - 构建 Docker 镜像及对应的容器
   - 与启动后的容器进行实时连接
   - 在容器内部部署与配置 LaTeX Workshop

6. 完成后您便可以:
   - 在享有完整的语法高亮、自动检查及智能代码补全等功能的前提下直接编辑 LaTeX 源文件
   - 按下保存自动触发云端的编译构建
   - 直接在 VSCode 编辑器内查阅最新渲染排版的 PDF
   - 实时掌握语法错误的反馈

## 定制化扩展 (Customization)

### 自定义 Dockerfile 

需要加载其它的预定义 LaTeX 宏包支持:

1. 打开并编辑代码仓库底部的 Dockerfile:
   ```dockerfile
   # 将所需的宏包包名追加添加至该行之后
   RUN tlmgr install        moderncv        ulem        xcolor        # 这里可以补充添加您的宏包列表
   ```

2. 确保更改后重新构建容器镜像方可生效执行:
   ```bash
   docker compose build
   ```

## 故障排除

### Docker 运行异常

- **Docker 未运行/已退出**: 务必检查 Docker Desktop 后台服务是否正常稳定开启运行状态
  ```bash
  # Check if Docker is running (用于验证 Docker 引擎状态)
  docker info
  ```

- **访问及授权权限错误**: 注意，如果您身在 Linux 这类拥有严密安全机制策略的发行版上，通常需要把您当前执行操作所属账户添加至相关 docker 组内进行授权。
  ```bash
  sudo usermod -aG docker $USER
  # （注）：操作结束后须注销并再次登录方可生效！
  ```

- **Windows 本地专有故障排除**: 确认 WSL 2 环境已被成功启用并与 Docker Desktop 完全集成应用。

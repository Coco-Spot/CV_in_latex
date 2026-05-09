[中文](./Readme.md) | [English](./Readme-EN.md)

# LaTeX CV Builder

This repository demonstrates a LaTeX resume template that supports **multiple build methods**, designed to provide high flexibility and compatibility for different platforms and workflows.

## Why multiple build methods?

Building the same document in multiple ways offers several key advantages:

- **Redundancy & Backup**: If one method fails, you can easily switch to another
- **Cross-platform Independence**: Works perfectly on Windows, macOS, and Linux
- **Flexibility**: Freely choose the build method that best suits your current environment and needs
- **Team Collaboration Compatibility**: Different team members can use different methods according to their habits

## Build Methods Comparison

| Build Method | Advantages | Disadvantages | Best Use Cases |
|--------------|------------|--------------|----------|
| **Local Build** | - Fast compilation<br>- Immediate feedback<br>- No internet required<br>- Full editor integration | - Requires LaTeX suite installation<br>- Requires environment configuration<br>- OS-dependent | Daily development<br>High-frequency edits<br>Personal projects |
| **Docker Build** | - Unified environment<br>- No local LaTeX installation needed<br>- Cross-platform compatibility<br>- Fully reproducible build process | - Docker installation required<br>- Initial startup might be slower<br>- Slight performance overhead from container | Team projects<br>Complex document packages<br>Environment isolation |
| **GitHub Actions** | - Fully automated operation<br>- Continuous Integration (CI)<br>- Automatically manages PDF Release publishing<br>- Zero local configuration | - Requires internet connection<br>- Not suitable for high-frequency iterative edits<br>- Dependent on GitHub platform | Production/Final build<br>Public publishing/Sharing<br>Version control |

## Build Method Documentation

Each build method comes with detailed, independent guides:

1. **[Local Build](./Readme-Local.md)** - Build directly on your local machine using VSCode/Cursor and the LaTeX Workshop extension
2. **[Docker Build](./Readme-Docker.md)** - Build via Docker without installing LaTeX natively
3. **[GitHub Actions](./Readme-GitHub-Actions.md)** - Automated building and Release publishing using GitHub Actions

## Auto-configuration Scripts

This repository includes several utility scripts to simplify the initial setup:

- `config_vscode_local.py` - Generates `.vscode/settings.json` for local LaTeX development
- `config_vscode_devcontainer.py` - Generates `.devcontainer/devcontainer.json` for Docker container-based development
- `docker_build.py` - Python script for cross-platform Docker building
- `docker_build.sh` - Shell script for Docker building (for Unix-like systems)

Providing both Python and Shell scripts ensures usability across all platforms:
- Python scripts are compatible with all platforms (Windows, macOS, Linux)
- Shell scripts provide a near-native runtime experience on Unix-like systems
- A rich toolchain guarantees that you can successfully compile your resume in any environment.

### Usage

```bash
# Configure VSCode for local build
python config_vscode_local.py

# Configure VSCode DevContainer
python config_vscode_devcontainer.py

# Build using Docker (Python script - Cross-platform)
python docker_build.py

# Build using Docker (Shell script - Unix-like systems)
./docker_build.sh
```

## Example Resume

This repository includes an example resume by default for demonstration and testing purposes.

## Other Template Sources

You can also explore and discover more beautiful templates here:
- https://www.overleaf.com/gallery/tagged/cv

## Project Repository

https://github.com/reveurmichael/cv_latex
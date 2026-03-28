---
title: 使用nvm管理node多版本
date: 2024-07-15 14:14:04
tags:
---

### **macOS系统上安装nvm步骤如下：注意在macOS系统上可以用安装Homebrew安装nvm**

1. **安装 Homebrew**（如果尚未安装）： `nvm` 可以通过 Homebrew 轻松安装。Homebrew 是 macOS（或 Linux）上的包管理器。在终端执行以下命令来安装 Homebrew：

   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

2. **使用 Homebrew 安装 nvm**： 安装完 Homebrew 后，可以通过以下命令来安装 `nvm`：

   ```bash
   brew install nvm
   ```

3. **验证安装**： 安装完成后，可以通过运行以下命令来验证 `nvm` 是否正确安装：

   ```bash
   nvm --version
   ```

4. **初始化 nvm**： 为了让 `nvm` 每次在新的终端会话中自动加载，您需要将以下命令添加到您的 shell 配置文件（如 `.bash_profile`, `.zshrc`, 等）中：

   ```bash
   echo "source $(brew --prefix nvm)/nvm.sh" >> ~/.zshrc
   ```

   或者，如果您使用的是 bash shell：

   ```bash
   echo "source $(brew --prefix nvm)/nvm.sh" >> ~/.bash_profile
   ```

5. **应用更改**： 为了使更改生效，您需要重新加载配置文件。根据您使用的 shell，执行以下命令之一：

   ```
   source ~/.zshrc
   ```

   或者

   ```
   source ~/.bash_profile
   ```

6. **安装和使用 Node.js 版本**： 现在您可以使用 `nvm` 来安装和切换 Node.js 版本了。例如，要安装最新版本的 Node.js，可以使用：

   ```
   nvm install node
   ```

   要安装特定版本的 Node.js，可以使用：

   ```
   nvm install 14
   ```

   要切换到特定版本，可以使用：

   ```
   nvm use 14
   ```

7. **查看可用的 Node.js 版本**： 您可以使用以下命令来列出所有已安装的 Node.js 版本：

   ```
   nvm ls
   ```

8. **设置默认 Node.js 版本**： 如果您想设置默认的 Node.js 版本，可以使用：

   ```
   nvm alias default 14
   ```


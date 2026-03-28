---
title: 云服务器搭建SVN远程仓库
date: 2024-07-12 10:57:23
tags:
---

在CentOS服务器上安装和配置Subversion（SVN）版本控制系统，废话不多说直接上步骤：

### 1. 安装Subversion

首先，确保你的系统软件包是最新的：

```bash
sudo yum update
```

然后，安装Subversion软件包：

```bash
sudo yum install subversion
```

### 2. 创建一个版本库

选择一个目录来存放你的版本库，例如 `/var/svn`，并创建该目录：

```bash
sudo mkdir -p /var/svn
```

接着，创建一个新的版本库：

```bash
sudo svnadmin create /var/svn/myrepo
```

### 3. 配置版本库

#### 编辑 `svnserve.conf` 文件

编辑位于 `/var/svn/myrepo/conf` 目录下的 `svnserve.conf` 文件，启用和配置基本选项：

```bash
sudo nano /var/svn/myrepo/conf/svnserve.conf
```

取消以下行的注释并进行配置：

```plaintext
anon-access = none
auth-access = write
password-db = passwd
```

#### 配置用户认证

编辑 `passwd` 文件，添加用户信息：

```bash
sudo nano /var/svn/myrepo/conf/passwd
```

添加类似以下格式的用户信息：

```
[users]
user1 = password1
user2 = password2
```

### 4. 启动Subversion服务

使用以下命令启动Subversion服务：

```bash
sudo svnserve -d -r /var/svn
```

### 5. 验证和使用

你现在可以使用以下命令检查是否能够连接到新的SVN版本库：

```bash
svn checkout svn://你的云服务器IP/myrepo
```

使用你在 `passwd` 文件中设置的用户名和密码进行验证。

### 6. 配置防火墙（如果需要）

确保防火墙允许Subversion流量。默认情况下，SVN使用端口3690。可以使用以下命令配置防火墙：

```bash
sudo firewall-cmd --permanent --add-port=3690/tcp
sudo firewall-cmd --reload
```

### 7. 设置开机自启动（可选）

你可以在 `/etc/rc.local` 文件中添加以下行，以便在系统启动时自动启动Subversion服务：

```bash
sudo nano /etc/rc.local
```

添加以下行：

```bash
同上启动Svn服务的命令
svnserve -d -r /var/svn
```

保存并关闭文件后，确保 `rc.local` 文件具有可执行权限：

```bash
sudo chmod +x /etc/rc.local
```

完成以上步骤后，好的大功告成你的CentOS云服务器有svn仓库了。

# Git 同时推送到多个远程仓库配置指南

本文档介绍如何配置 Git 同时推送到 Gitee 和 GitHub 两个远程仓库。

## 1. 查看当前远程仓库配置

首先查看当前已配置的远程仓库：

```bash
git remote -v
```

输出示例：
```
origin  git@gitee.com:jayroyin/qsrobot-system.git (fetch)
origin  git@gitee.com:jayroyin/qsrobot-system.git (push)
```

## 2. 添加第二个远程仓库（GitHub）

将 GitHub 仓库添加为第二个远程仓库：

```bash
git remote add github git@github.com:your-org/your-repo.git
```

将 `your-org` 替换为你的 GitHub 组织名，`your-repo` 替换为仓库名。

## 3. 配置同时推送到两个仓库

使用 `pushurl` 配置让 `origin` 同时推送到两个仓库：

```bash
git config --add remote.origin.pushurl git@gitee.com:jayroyin/qsrobot-system.git
git config --add remote.origin.pushurl git@github.com:your-org/your-repo.git
```

## 4. 验证配置结果

查看远程仓库配置：

```bash
git remote -v
```

输出示例：
```
github   git@github.com:your-org/your-repo.git (fetch)
github   git@github.com:your-org/your-repo.git (push)
origin   git@gitee.com:jayroyin/qsrobot-system.git (fetch)
origin   git@gitee.com:jayroyin/qsrobot-system.git (push)
origin   git@github.com:your-org/your-repo.git (push)
```

## 5. 添加 GitHub Host Key（首次配置需要）

如果首次连接 GitHub，需要添加 host key：

```bash
ssh-keyscan github.com >> ~/.ssh/known_hosts 2>/dev/null
```

## 6. 推送代码

配置完成后，只需使用一条命令即可同时推送到两个仓库：

```bash
git push origin dev
```

## 7. 推送到其他分支

如果需要推送到其他分支：

```bash
git push origin main
```

## 8. SSH 密钥配置（如遇权限问题）

### 8.1 检查现有 SSH 密钥

```bash
ls -la ~/.ssh/
```

### 8.2 查看公钥内容

```bash
cat ~/.ssh/id_ed25519.pub
```

### 8.3 将公钥添加到 GitHub

1. 打开 GitHub → Settings → SSH and GPG keys → New SSH key
2. 将公钥复制粘贴进去
3. 保存

### 8.4 将公钥添加到 Gitee（如需要）

1. 打开 Gitee → 设置 → SSH 公钥
2. 将公钥复制粘贴进去
3. 保存

## 9. 其他常用命令

### 9.1 单独推送到 GitHub

```bash
git push github dev
```

### 9.2 单独推送到 Gitee

```bash
git push origin dev
```

### 9.3 删除 GitHub 远程仓库

```bash
git remote remove github
```

### 9.4 修改 pushurl 配置

```bash
git remote set-url --add --push origin git@github.com:new-org/new-repo.git
```

## 10. 注意事项

1. 确保 SSH 密钥已添加到对应的 GitHub/Gitee 账户
2. 首次推送可能需要确认 host key
仓库的分支3. 两个名建议保持一致
4. 如果推送失败，单独测试每个仓库的连接性

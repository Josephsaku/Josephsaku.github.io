---
title: How-to-Setup-Quartz
---

# 🛠️ Quartz 数字化花园建站与维护指南

> **Status**: Live 🚀  
> **Philosophy**: Clean Fit & Focused  
> **Stack**: Quartz v4, GitHub Actions, Node.js 22

这是关于如何从零构建并在 GitHub Pages 上部署 Quartz 数字化花园的实践记录。

---

## 1. 基础环境配置

在开始之前，确保你的本地开发环境（如 MacBook Air）已安装以下工具：
- **Node.js**: >= v22 (Quartz v4.5+ 的硬性要求)
- **Git**: 用于版本管理与同步

---

## 2. 仓库初始化与分支策略

为了适配 Quartz `npx quartz sync` 命令的硬编码逻辑，我们采用了 **`v4`** 作为主分支名，而非默认的 `main`。

### 分支迁移步骤：
```bash
# 1. 将本地分支重命名为 v4
git branch -m main v4

# 2. 关联远程仓库
git remote add origin [https://github.com/](https://github.com/)<YourUsername>/<YourUsername>.github.io.git

# 3. 推送到远程 v4
git push -u origin v4
````

> **注意**：去 GitHub 仓库设置 `Settings -> Branches`，将 `v4` 设为 **Default Branch**。

---

## 3. 自动化部署配置 (GitHub Actions)

由于 Quartz 需要在云端将 Markdown 编译为 HTML，必须配置 `.github/workflows/deploy.yml`。

### 关键配置项：

- **Node 版本**: 必须指定 `node-version: 22`。
    
- **分支监听**: `on: push: branches: - v4`。
    

### 权限开启（重要）：

在 GitHub 仓库 `Settings -> Actions -> General`：

1. 将 **Workflow permissions** 改为 **Read and write permissions**。
    
2. 在 `Settings -> Environments -> github-pages` 中，确保 **Deployment branches** 允许 `v4` 分支发布。
    

---

## 4. 内容管理工作流

为了保持笔记库（Obsidian）与代码库（Quartz）的独立性，采用“物理同步”策略。

1. **创作**: 在独立文件夹（如 `repo-for-knowledge`）中编写笔记。
    
2. **搬运**: 将需要发布的 `.md` 文件和附件物理拷贝/替换到 `quartz/content`。
    
3. **预览**:

    ```
    npx quartz build --serve
    ```
    
---

## 5. 站点维护：npx quartz sync

这是 Quartz 最核心的维护指令，它集成了备份、拉取与推送功能。

### 命令解析：

Bash

```
npx quartz sync
```

**执行逻辑：**

1. **Backing up**: 自动执行 `git add .` 和 `git commit`。
    
2. **Pulling**: 尝试从远程 `v4` 分支拉取更新（避免多设备冲突）。
    
3. **Pushing**: 将本地改动推送到 GitHub，触发自动构建。
    
### 常见报错处理：

- **找不到远程引用 v4**: 确保本地和远程分支名均已统一为 `v4`。
    
- **Node 版本错误**: 检查并更新 `deploy.yml` 中的 `node-version` 为 22。
    
- **部署拒绝**: 检查 Environment 权限，确保 `v4` 被允许发布到 `github-pages`。
    

---

## 6. 日常更新流程 (Checklist)

- [ ] 在知识库完成笔记撰写。
    
- [ ] 物理拷贝文件至 `quartz/content`。
    
- [ ] 终端执行 `npx quartz sync`。
    
- [ ] 检查 GitHub Actions 状态（绿色代表成功）。
    
- [ ] 访问 `https://<YourUsername>.github.io` 查看更新。
    

---



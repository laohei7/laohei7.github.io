# 如何同时关联Gitee和Github

要**同时关联 GitHub 和 Gitee 仓库**，你可以通过配置一个 Git 本地仓库，让它同步推送到两个远程仓库。以下是完整步骤：

---

## ✅ 方法一：一个本地仓库推送到两个远程（推荐）

### 步骤 1：初始化或进入你的项目目录

```bash
cd your-project
# 如果还没初始化 Git，则先执行
git init
```

---

### 步骤 2：添加 GitHub 和 Gitee 两个远程仓库

```bash
# 添加 GitHub 远程仓库
git remote add github https://github.com/yourname/your-repo.git

# 添加 Gitee 远程仓库
git remote add gitee https://gitee.com/yourname/your-repo.git
```

> 如果提示已存在 `origin`，你可以选择重命名或保留它为 GitHub：

```bash
git remote set-url origin https://github.com/yourname/your-repo.git
git remote add gitee https://gitee.com/yourname/your-repo.git
```

---

### 步骤 3：推送到两个远程

每次提交后可以这样推送：

```bash
git push github main
git push gitee main
```

---

### 可选：设置一次推送到多个远程

你也可以设置 `origin` 同时推送到两个远程地址：

```bash
git remote set-url --add origin https://github.com/yourname/your-repo.git
git remote set-url --add origin https://gitee.com/yourname/your-repo.git
```

然后你只需：

```bash
git push origin main
```

但注意：**这在 Gitee 不稳定或 GitHub 限速时容易出错**，建议保留两个独立 remote（`github` 和 `gitee`）分别推送更安全。

---

## 🧠 提示：如何验证远程配置

```bash
git remote -v
```

输出应类似：

```
github https://github.com/yourname/your-repo.git (fetch)
github https://github.com/yourname/your-repo.git (push)
gitee  https://gitee.com/yourname/your-repo.git  (fetch)
gitee  https://gitee.com/yourname/your-repo.git  (push)
```
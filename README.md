# ✦ 创意想法库

一个基于 GitHub Pages 的个人创意管理工具，支持推理类、音乐类、文学类的想法追踪，数据存储在私有仓库。

## 功能

- **推理类**：推理小说 / 剧本杀 / 海龟汤
- **音乐类**：统一管理
- **文学类**：散文 / 通俗小说 / 诗歌
- 每个类型均有 **想法区 → 半成品区 → 成品区** 三阶段
- 自动同步（改动后 2 秒自动保存）+ 手动同步
- 数据存于私有仓库 `writing.json`，网页完全静态

---

## 部署步骤

### 1. 创建公开仓库（网页）

```bash
# 新建一个 public repo，例如：writing-repo
git init writing-repo
cd writing-repo
# 将 index.html 放入根目录
git add index.html README.md
git commit -m "init: writing ideas repo"
git remote add origin https://github.com/YOUR_USERNAME/writing-repo.git
git push -u origin main
```

在仓库 Settings → Pages → Source 选择 `main` 分支，保存后即可访问：
`https://YOUR_USERNAME.github.io/writing-repo/`

### 2. 准备私有仓库（数据）

在私有仓库中创建 `writing.json` 文件（首次打开网页会自动创建）。

如果希望手动初始化，可将本项目提供的 `writing.json` 模版上传到私有仓库根目录。

### 3. 创建 Fine-Grained Personal Access Token

1. 前往 GitHub → Settings → Developer Settings → Personal access tokens → **Fine-grained tokens**
2. 点击 **Generate new token**
3. 配置：
   - **Token name**：`writing-repo-sync`
   - **Expiration**：按需选择（推荐 1 年）
   - **Repository access**：选择 **Only select repositories** → 选择你的私有数据仓库
   - **Permissions → Contents**：选择 **Read and Write**
4. 生成并复制 token（格式：`github_pat_xxxx...`）

### 4. 在网页中配置

打开网页后，点击右上角 **⚙ 设置**，填写：
- GitHub Token
- 私有仓库 Owner（你的用户名）
- 私有仓库名称

点击"保存并测试连接"，成功后数据自动加载。

---

## 数据格式（writing.json）

数据以 JSON 格式存储于私有仓库：

```json
{
  "_meta": { "version": "1.0", "last_updated": "2026-05-30T..." },
  "mystery": {
    "detective_novel": { "ideas": [...], "wip": [...], "done": [...] },
    "murder_mystery":  { "ideas": [...], "wip": [...], "done": [...] },
    "turtle_soup":     { "ideas": [...], "wip": [...], "done": [...] }
  },
  "music": { "ideas": [...], "wip": [...], "done": [...] },
  "literature": {
    "essay":         { "ideas": [...], "wip": [...], "done": [...] },
    "popular_novel": { "ideas": [...], "wip": [...], "done": [...] },
    "poetry":        { "ideas": [...], "wip": [...], "done": [...] }
  }
}
```

每条条目结构：
```json
{
  "id": "uuid",
  "title": "标题",
  "content": "内容/笔记",
  "tags": ["标签1", "标签2"],
  "created_at": "2026-05-30T12:00:00Z",
  "updated_at": "2026-05-30T12:00:00Z"
}
```

---

## 隐私说明

- GitHub Token 仅存储在**本地浏览器 localStorage**，不会上传到任何服务器
- 网页为纯静态页面，所有 API 请求直接从浏览器发往 GitHub API
- 私有仓库数据仅有 Token 持有人可访问

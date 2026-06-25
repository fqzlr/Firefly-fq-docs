# CMS 后台管理指南

Firefly Blog 内置了轻量级 CMS 后台管理面板，采用 SPA 单页面应用架构，支持在线管理说说动态、笔记、友情链接、影视追番等内容，数据存储在 GitHub Gist 中，无需数据库，完全免费。

## 访问后台

后台管理面板地址：`/admin/`

例如你的博客地址是 `https://example.com`，则后台地址为 `https://example.com/admin/`。

登录后左侧侧边栏常驻，通过 hash 路由切换各功能模块（如 `#moments`、`#notebooks`、`#friends`、`#bangumi`、`#settings`），页面无需刷新。

::: tip
需要在 `siteConfig.ts` 中开启 `pages.admin: true`（默认已开启）才能访问后台页面。
:::

## 密码设置

### 默认密码
后台默认密码为：`admin123`

::: danger
首次使用前请务必修改默认密码！
:::

### 修改密码（推荐：在线修改）

后台已内置在线修改密码功能，无需手动计算哈希和修改代码：

1. 登录后台后，点击左侧菜单「🔧 接口配置」
2. 在「🔐 管理密码修改」区域输入新密码（至少4位）
3. 再次输入确认密码
4. 点击「修改密码」按钮

系统会自动计算 SHA-256 哈希并通过 GitHub API 更新配置文件。修改后等待自动重新部署完成（通常1-3分钟）即可使用新密码登录。

::: warning 关于 Token 权限
在线修改密码需要你的 GitHub Token 拥有 **repo** 权限（因为需要修改仓库中的配置文件）。如果你的 Token 只有 `gist` 权限，可以勾选「使用单独的 Repo Token」，输入一个具有 `repo` 权限的 Token 来执行密码修改操作。
:::

### 修改密码（手动方式）

如果在线修改不可用，也可以手动计算哈希并修改配置文件。密码使用 SHA-256 哈希存储，无法从哈希反推明文。

**生成哈希方法：**

**方法一：PowerShell（Windows）**
```powershell
$bytes = [System.Text.Encoding]::UTF8.GetBytes("你的新密码")
[BitConverter]::ToString([System.Security.Cryptography.SHA256]::Create().ComputeHash($bytes)).Replace("-", "").ToLower()
```

**方法二：浏览器控制台**
打开任意网页按 F12 打开控制台，执行：
```javascript
await crypto.subtle.digest('SHA-256', new TextEncoder().encode('你的新密码')).then(b=>[...new Uint8Array(b)].map(x=>x.toString(16).padStart(2,'0')).join(''))
```

**方法三：Node.js**
```bash
node -e "const c=require('crypto');console.log(c.createHash('sha256').update('你的新密码').digest('hex'))"
```

将生成的 64 位十六进制字符串替换 `externalMomentsConfig.ts` 中的 `adminPasswordHash` 值。

## 接口配置（统一 GitHub Token）

所有功能模块（说说、笔记、友情链接、影视追番）共用同一个 GitHub Token，在后台「🔧 接口配置」页面统一管理：

1. 登录后台，进入「🔧 接口配置」
2. 在「🔑 GitHub Token」输入框中粘贴你的 Token
3. 点击「保存」按钮
4. 可点击「🧪 测试连接」验证 Token 是否有效

Token 保存在浏览器本地（localStorage），不会上传到服务器，一处配置全后台生效。

::: tip
如果部署时配置了 `GITHUB_TOKEN` 环境变量，Token 会自动读取，无需手动输入。
:::

## 前置配置

使用后台管理前需要完成以下配置：

### 1. 创建 GitHub Token

1. 访问 GitHub [Settings → Developer settings → Personal access tokens → Tokens (classic)](https://github.com/settings/tokens)
2. 点击 **Generate new token (classic)**
3. Note 填写任意名称（如 `blog-admin`）
4. Expiration 选择有效期（建议选择 `No expiration` 或较长时间）
5. 勾选以下权限：
   - **gist** 权限（必须，用于读写 Gist 数据）
   - **repo** 权限（可选，用于在线修改密码等仓库操作）
6. 点击 Generate token，复制生成的 Token（只显示一次！）

### 2. 创建 Gist 仓库

需要为各功能模块创建独立的 Secret Gist：

**说说 Gist：**
- 文件名：`moments.json`
- 初始内容：`[]`

**笔记本 Gist：**
每个笔记本对应一个 Gist：
- 文件名：`notebooks-entries.json`
- 初始内容：`[]`

**友情链接 Gist：**
- 文件名：`friends.json`
- 初始内容：`[]`

**影视追番 Gist：**
- 文件名：`bangumi.json`
- 初始内容：`[]`

::: warning
请务必创建 **Secret Gist**，不要创建公开 Gist，否则你的数据可能被他人看到或篡改。
:::

### 3. 配置环境变量（可选）

在部署平台的环境变量中添加：
```
GITHUB_TOKEN=你刚才复制的GitHubToken
```

本地开发时可在项目根目录 `.env` 文件中添加。配置后无需在浏览器中手动输入 Token。

### 4. 配置 Gist ID

从创建的 Gist URL 中获取 ID（如 `https://gist.github.com/user/abc123` 中的 `abc123`），填入对应配置文件：

- 说说：`externalMomentsConfig.ts` 中的 `gistId`
- 笔记：`externalNotebooksConfig.ts` 中的 `notebookGists`
- 友情链接：`externalFriendsConfig.ts` 中的 `gistId`
- 影视追番：`externalBangumiConfig.ts` 中的 `gistId`

## 功能介绍

### ✍️ 说说管理

进入「说说动态」页面，可以：

- **发布说说**：输入内容，支持 Markdown 格式，可添加图片和标签
- **编辑说说**：修改已发布的说说内容
- **删除说说**：删除不需要的说说
- **预览**：发布前预览渲染效果

### 📓 笔记管理

进入「笔记管理」页面，支持多个笔记本，每个笔记本独立管理：

- **切换笔记本**：在顶部选择要编辑的笔记本
- **使用模板**：快速创建每日总结、日记、读书笔记、灵感、待办等类型笔记
- **编辑笔记**：支持 Markdown 实时编辑
- **删除笔记**：删除不需要的笔记
- **模板变量**：`{name}` 会自动替换为当天日期

### 🔗 友情链接

进入「友情链接」页面，可以：

- **添加友链**：支持粘贴友链信息自动解析（站点名称、描述、链接、头像）
- **编辑友链**：修改已有友链信息
- **删除友链**：移除失效或不需要的友链
- **状态管理**：标记友链为正常/失联/待审核状态

### 🎬 影视追番

进入「影视追番」页面，可以：

- **搜索影视**：同时搜索 TMDB、豆瓣、Bangumi，点击结果自动填充信息
- **添加条目**：记录正在追番/观看/已完成的影视、动漫、书籍、游戏、音乐
- **评分评价**：给作品打分并写评价
- **进度追踪**：记录观看进度和状态

### 🔧 接口配置

统一管理 GitHub Token 等接口凭据，一处配置全后台生效：
- GitHub Token 保存、清除、连接测试
- 在线修改管理密码

### 数据同步

- 所有修改实时保存到 GitHub Gist
- 前端页面访问时动态拉取最新数据
- **无需重新构建和部署**博客即可看到更新

## 数据备份

建议定期备份 Gist 数据：

1. 访问你的 Gist 页面
2. 下载每个 Gist 的 JSON 文件
3. 或使用 Git 克隆 Gist 仓库进行版本管理

## 常见问题

### Q: 登录提示密码错误？
A: 检查配置文件中的 `adminPasswordHash` 是否为密码的正确 SHA-256 哈希。注意密码区分大小写。

### Q: 发布内容失败？
A: 检查：
1. 是否已在「接口配置」中正确保存 GitHub Token
2. Token 是否有 `gist` 权限
3. Gist ID 是否填写正确
4. Gist 文件名是否正确（各模块对应不同的文件名）

### Q: 在线修改密码失败？
A: 检查：
1. Token 是否有 `repo` 权限（修改仓库文件需要）
2. 如使用单独 Repo Token，确保其有效且具有 `repo` 权限
3. 网络能否正常访问 GitHub API

### Q: 本地开发可以使用后台吗？
A: 可以，但需要在 `.env` 文件中配置 `GITHUB_TOKEN`，本地修改会真实写入你的 Gist。

### Q: 可以禁用后台吗？
A: 可以，在 `siteConfig.ts` 中设置 `pages.admin: false` 即可关闭后台页面。同时可以在各配置文件中设置 `enable: false` 禁用对应外部数据源。

### Q: Gist 有容量限制吗？
A: GitHub Gist 每个文件限制为 1MB，对于博客内容来说完全足够使用。如果数据量较大，可以定期归档旧内容。

## 安全建议

1. **务必修改默认密码**，使用强密码
2. 使用 **Secret Gist**，不要创建公开 Gist
3. 妥善保管 GitHub Token，不要提交到代码仓库
4. Token 建议同时勾选 `gist` 和 `repo` 权限，以便使用全部后台功能
5. 定期轮换 Token 和密码
6. 不要在后台输入敏感信息（如银行卡号等）

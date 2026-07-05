# 快速开始

Fqzlr 的博客是一款基于 **Astro 框架** 开发的现代化个人博客主题，基于 Firefly 主题二次开发。专为技术爱好者和内容创作者设计，提供了丰富的功能模块和高度可定制的界面。

---

## 环境要求

在开始之前，请确保你的开发环境满足以下要求：

- **Node.js**: 22.0 或更高版本
- **包管理器**: 推荐使用 [pnpm](https://pnpm.io/)（项目已配置 pnpm 作为默认包管理器）
- **Git**: 用于版本控制和克隆仓库

::: tip 提示
你可以使用 `node -v` 命令检查 Node.js 版本，使用 `pnpm -v` 检查 pnpm 版本。
:::

---

## 安装

### 1. 克隆仓库

```bash
git clone https://github.com/fqzlr/fqzlr-bk.git
cd dumplingandcakeblog
```

### 2. 安装依赖

```bash
pnpm install
```

### 3. 启动开发服务器

```bash
pnpm dev
```

启动成功后，在浏览器中访问 `http://localhost:4321` 即可预览你的博客。

### 4. 构建生产版本

```bash
pnpm build
```

构建产物将生成在 `dist/` 目录下，可以直接部署到任何静态托管服务。

### 5. 预览生产构建

```bash
pnpm preview
```

---

## 项目结构

```
dumplingandcakeblog/
├── _backup/                  # 备份文件（旧内容、原始文档、临时文件等）
├── api/                      # 服务端 API 脚本
├── public/                   # 静态资源目录（不经过构建优化）
│   ├── assets/              # 静态资源（图片、JS 等）
│   ├── favicon/             # 网站图标
│   ├── gallery/             # 相册图片
│   └── pio/                 # Live2D/Spine 模型资源
├── scripts/                  # 工具脚本
│   ├── fetch-media/         # 媒体资源抓取脚本
│   ├── fetch-music/         # 音乐下载脚本
│   ├── generate-icons/      # 图标生成脚本
│   ├── new-post/            # 新建文章脚本
│   └── sync/                # 笔记同步脚本
├── src/
│   ├── assets/              # 资源文件（会被构建优化）
│   │   └── images/          # 图片资源
│   ├── components/          # 组件目录
│   │   ├── comment/         # 评论系统组件
│   │   ├── common/          # 通用组件
│   │   ├── controls/        # 控制组件（搜索、返回顶部等）
│   │   ├── edit/            # 在线编辑组件
│   │   ├── features/        # 功能组件（音乐播放器、看板娘等）
│   │   ├── guestbook/       # 留言板组件
│   │   ├── layout/          # 布局组件
│   │   ├── life/            # 生活相关组件
│   │   ├── moments/         # 动态组件
│   │   ├── pages/           # 页面特定组件
│   │   └── widget/          # 侧边栏小部件
│   ├── config/              # ⭐ 配置文件目录（所有可配置项都在这里）
│   ├── constants/           # 常量定义
│   ├── content/             # 内容目录（文章、页面、数据）
│   │   ├── posts/           # 博客文章
│   │   ├── album/           # 相册
│   │   ├── bangumi/         # 追番/影视/书籍/音乐
│   │   ├── life/            # 生活记录
│   │   │   ├── notebooks/   # 笔记本
│   │   │   ├── places/      # 足迹
│   │   │   └── routines/    # 日常规划
│   │   ├── moments/         # 说说动态
│   │   ├── changelog/       # 更新日志
│   │   ├── friends/         # 友情链接
│   │   ├── daohang/         # 网址导航
│   │   ├── spec/            # 特殊页面
│   │   ├── ziyuan/          # 资源（公告、名言等）
│   │   └── danmu/           # 弹幕
│   ├── content.config.ts    # 内容集合配置
│   ├── i18n/                # 国际化配置
│   ├── layouts/             # 布局模板
│   ├── pages/               # ⭐ 页面路由目录
│   ├── styles/              # 样式文件
│   ├── types/               # TypeScript 类型定义
│   └── utils/               # 工具函数
├── astro.config.mjs         # Astro 配置文件
├── package.json             # 项目依赖配置
└── README.md                # 项目说明
```

---

## 快速配置

要快速定制你的博客，主要修改以下配置文件：

### 1. 站点基础配置

编辑 `src/config/siteConfig.ts`：

```typescript
export const siteConfig: SiteConfig = {
  title: "你的博客标题",
  subtitle: "你的博客副标题",
  site_url: "https://example.com/",
  description: "博客描述",
  // ... 更多配置
}
```

### 2. 个人资料配置

编辑 `src/config/profileConfig.ts` 来设置头像、个人简介、社交链接等：

```typescript
export const profileConfig: ProfileConfig = {
  name: "你的名字",
  avatar: "assets/images/avatar.webp",
  bio: ["你的个人签名"],
  links: [
    { name: "GitHub", icon: "simple-icons:github", url: "https://github.com/yourname" },
    // ... 更多社交链接
  ],
}
```

### 3. 导航栏配置

编辑 `src/config/navBarConfig.ts` 来定制导航链接。

::: tip 提示
所有配置文件都有详细的注释说明，你可以直接参考注释进行配置。更多配置选项请查看 [配置指南](/zh/config/site-config) 章节。
:::

---

## 创建第一篇文章

在 `src/content/posts/` 目录下创建一个新的 Markdown 文件，例如 `my-first-post.md`：

```markdown
---
title: 我的第一篇文章
published: 2024-01-01
description: 这是我的第一篇博客文章
tags:
  - 随笔
category: 随笔
---

# 欢迎来到我的博客！

这是我的第一篇文章，开始写作吧！
```

文章的 Frontmatter 支持以下字段：

| 字段 | 说明 | 类型 | 默认值 |
|------|------|------|--------|
| `title` | 文章标题 | `string` | - |
| `published` | 发布日期 | `Date` | - |
| `updated` | 更新日期 | `Date` | 可选 |
| `description` | 文章描述 | `string` | `""` |
| `tags` | 标签列表 | `string[]` | `[]` |
| `category` | 文章分类 | `string` | `""` |
| `image` | 封面图片 | `string` | `""` |
| `draft` | 是否为草稿 | `boolean` | `false` |
| `pinned` | 是否置顶 | `boolean` | `false` |
| `comment` | 是否允许评论 | `boolean` | `true` |
| `lang` | 文章语言 | `string` | `""` |
| `author` | 作者 | `string` | `""` |

::: warning 注意
文章文件名建议使用英文和数字，避免特殊字符。
:::

---

## 内容编辑方式

### 方式一：直接编辑文件（推荐）

直接在 `src/content/` 对应目录下创建/编辑 Markdown 文件。

### 方式二：在线编辑

网站内置了在线编辑功能，访问 `/admin.html`（需先在 `editConfig.ts` 中配置密码）。

支持在线编辑的内容：
- 博客文章
- 说说动态
- 友情链接
- 相册
- 笔记本
- 日常规划
- 足迹
- 赞助
- 网址导航
- 番组计划

### 方式三：命令行创建

```bash
pnpm new-post 文章标题
```

---

## 下一步

- 📖 了解完整的 [项目结构](/zh/guide/project-structure)
- ⚙️ 查看所有 [配置选项](/zh/config/site-config)
- 🎵 配置 [音乐播放器](/zh/guide/music-player)
- 💬 配置 [评论系统](/zh/guide/comments)
- ✍️ 使用 [CMS后台管理](/zh/guide/admin-panel)
- 🚀 学习如何 [部署](/zh/guide/deployment) 你的博客

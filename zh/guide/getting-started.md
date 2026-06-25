# 快速开始

Firefly 是一款基于 **Astro 框架** 开发的清新美观且现代化个人博客主题，专为技术爱好者和内容创作者设计。该主题融合了现代 Web 技术栈，提供了丰富的功能模块和高度可定制的界面，让您能够轻松打造出专业且美观的个人博客网站。

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
git clone https://github.com/fqzlr/my-blog.git
cd my-blog
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
my-blog/
├── docs/                    # VitePress 文档目录（本教程所在位置）
│   ├── .vitepress/         # VitePress 配置
│   └── zh/                 # 中文文档
│       ├── guide/          # 指南文档
│       ├── config/         # 配置文档
│       └── components/     # 组件文档
├── public/                 # 静态资源目录
│   ├── assets/            # 静态资源（CSS、图片、JS、音乐文件等）
│   ├── favicon/           # 网站图标
│   ├── fonts/             # 字体文件
│   ├── models/            # 3D模型文件
│   └── pio/               # Live2D/Spine 模型资源
├── src/
│   ├── assets/            # 资源文件（图片等）
│   ├── components/        # 组件目录
│   │   ├── about/         # 关于页面组件
│   │   ├── analytics/     # 统计分析组件
│   │   ├── comment/       # 评论系统组件
│   │   ├── common/        # 通用组件
│   │   ├── controls/      # 控制组件（搜索、主题切换等）
│   │   ├── features/      # 功能组件（音乐播放器、看板娘等）
│   │   ├── layout/        # 布局组件
│   │   ├── misc/          # 杂项组件
│   │   ├── moments/       # 动态组件
│   │   ├── pages/         # 页面特定组件
│   │   └── widget/        # 侧边栏小部件
│   ├── config/            # ⭐ 配置文件目录（所有可配置项都在这里）
│   ├── constants/         # 常量定义
│   ├── content/           # 内容目录（文章、页面、数据）
│   │   ├── posts/         # 博客文章
│   │   ├── bangumi/       # 追番/影视/书籍/音乐
│   │   ├── life/          # 生活记录（日记、笔记等）
│   │   ├── moments/       # 动态
│   │   ├── changelog/     # 更新日志
│   │   └── spec/          # 特殊页面
│   ├── i18n/              # 国际化配置
│   ├── layouts/           # 布局模板
│   ├── pages/             # ⭐ 页面路由目录
│   ├── plugins/           # 插件配置（Markdown 扩展等）
│   ├── styles/            # 样式文件
│   ├── types/             # TypeScript 类型定义
│   ├── utils/             # 工具函数
│   └── workers/           # Web Workers
├── scripts/               # 构建脚本
├── astro.config.mjs       # Astro 配置文件
├── package.json           # 项目依赖配置
└── tsconfig.json          # TypeScript 配置
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
  description: "博客描述",
  author: "你的名字",
  // ... 更多配置
}
```

### 2. 导航栏配置

编辑 `src/config/navBarConfig.ts` 来定制导航链接。

### 3. 个人资料配置

编辑 `src/config/profileConfig.ts` 来设置头像、个人简介、社交链接等。

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

| 字段 | 说明 | 类型 |
|------|------|------|
| `title` | 文章标题 | `string` |
| `published` | 发布日期 | `Date` |
| `description` | 文章描述 | `string` |
| `tags` | 标签列表 | `string[]` |
| `category` | 文章分类 | `string` |
| `image` | 封面图片 | `string` |
| `draft` | 是否为草稿 | `boolean` |

::: warning 注意
文章文件名不要包含中文和特殊字符，建议使用英文和数字。
:::

---

## 下一步

- 📖 了解完整的 [项目结构](/zh/guide/project-structure)
- ⚙️ 查看所有 [配置选项](/zh/config/site-config)
- 🎵 配置 [音乐播放器](/zh/guide/music-player)
- 🎮 体验 [3D音乐可视化](/zh/guide/music-visualizer)
- 🚀 学习如何 [部署](/zh/guide/deployment) 你的博客


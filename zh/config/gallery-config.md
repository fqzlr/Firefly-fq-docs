# 相册配置详解

相册功能用于展示图片集合，支持标签分类、地点信息、照片说明等。

数据目录：`src/content/album/`
图片目录：`public/gallery/`

Schema 定义：`src/content.config.ts` 中的 `albumCollection`

## 数据格式

每个相册为一个独立的 `.md` 或 `.json` 文件，通过 frontmatter 配置元数据，正文为相册描述（支持 Markdown）。

## Frontmatter 字段

| 字段 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| `title` | `string` | 是 | - | 相册标题 |
| `subtitle` | `string` | 否 | `""` | 相册副标题 |
| `cover` | `string` | 否 | - | 封面图片路径 |
| `date` | `date` | 是 | - | 相册日期，用于排序 |
| `location` | `string` | 否 | `""` | 拍摄地点 |
| `photos` | `Photo[]` | 否 | `[]` | 照片列表 |
| `tags` | `string[]` | 否 | `[]` | 标签数组 |
| `draft` | `boolean` | 否 | `false` | 是否为草稿 |

## Photos 字段说明

`photos` 字段支持两种格式：

### 1. 字符串格式（推荐）

直接写图片路径：

```yaml
photos:
  - /gallery/album-name/photo1.webp
  - /gallery/album-name/photo2.webp
```

### 2. 对象格式

支持设置 alt 和 caption：

```yaml
photos:
  - src: /gallery/album-name/photo1.webp
    alt: 照片1描述
    caption: 照片1标题
  - src: /gallery/album-name/photo2.webp
    alt: 照片2描述
    caption: 照片2标题
```

对象格式支持的属性：

| 属性 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `src` | `string` | 是 | 图片路径 |
| `alt` | `string` | 否 | 图片替代文本 |
| `caption` | `string` | 否 | 图片说明文字 |

## 图片存放位置

相册图片放置在 `public/gallery/` 目录下，建议每个相册创建独立的子目录：

```
public/gallery/
├── ai-2026/
│   ├── 001.webp
│   ├── 002.webp
│   └── ...
├── mc-2026/
│   ├── 00001.webp
│   └── ...
└── gpt-img2-2026/
    ├── 1.webp
    └── ...
```

::: tip
图片路径以 `/gallery/` 开头表示 `public/gallery/` 目录。
:::

## 封面图规则

- 如果设置了 `cover` 字段，使用指定图片作为封面
- 如果没有设置 `cover`，使用 `photos` 数组中的第一张图片作为封面

## 示例

### 基础相册

```markdown
---
title: 2024 旅行记录
subtitle: 今年去过的地方
cover: /gallery/travel-2024/cover.webp
date: 2024-07-15
location: 云南
tags:
  - 旅行
  - 风景
photos:
  - /gallery/travel-2024/1.webp
  - /gallery/travel-2024/2.webp
  - /gallery/travel-2024/3.webp
draft: false
---

这是 2024 年夏天去云南旅行的照片合集~
```

### 带详细说明的相册

```markdown
---
title: GPT生图
subtitle: GPT生成的图片
cover: /gallery/gpt-img2-2026/1.webp
date: 2026-05-24
location: gpt
tags:
  - AI
  - GPT生图
photos:
  - src: /gallery/gpt-img2-2026/1.webp
    alt: AI生成的风景图
    caption: 梦幻山谷
  - src: /gallery/gpt-img2-2026/2.webp
    alt: AI生成的人物图
    caption: 未来少女
draft: false
---

GPT 生成的各种风格的图片
```

### 草稿相册

设置 `draft: true` 的相册不会在页面上显示：

```markdown
---
title: 未完成的相册
date: 2024-06-01
tags:
  - 草稿
photos: []
draft: true
---

这个相册还在整理中...
```

## 文件命名规范

建议使用有意义的名称作为文件名：

```
src/content/album/
├── 旅行2024.md
├── 日常生活.md
└── AI生图.md
```

## 排序规则

相册按 `date` 字段降序排列，最新的相册显示在最前面。

## 支持的图片格式

- JPG / JPEG
- PNG
- WebP（推荐，体积小质量好）
- AVIF（体积最小，兼容性稍低）
- GIF（动图）

::: tip
- 图片建议使用 WebP 格式以减小加载体积
- 相册按 `date` 降序排列，最新的显示在前面
- 本地图片会在构建时被 Astro 自动优化
- 草稿相册（`draft: true`）不会被展示
:::

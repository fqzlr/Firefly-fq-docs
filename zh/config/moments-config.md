# 说说动态配置详解

说说动态是类似朋友圈的短内容发布功能，支持图片、标签、定位、设备信息等。

数据目录：`src/content/moments/`

Schema 定义：`src/content.config.ts` 中的 `momentsCollection`

## 数据格式

每个说说为一个独立的 `.md` 文件，通过 frontmatter 配置元数据，正文为说说内容（支持 Markdown）。

## Frontmatter 字段

| 字段 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| `published` | `date` | 是 | - | 发布时间 |
| `author` | `string` | 否 | `""` | 作者名称 |
| `avatar` | `string` | 否 | `""` | 作者头像 URL |
| `pinned` | `boolean` | 否 | `false` | 是否置顶 |
| `images` | `string[]` | 否 | `[]` | 图片 URL 数组 |
| `tags` | `string[]` | 否 | `[]` | 标签数组 |
| `location` | `string` | 否 | `""` | 定位/地点信息 |
| `device` | `string` | 否 | `""` | 发布设备信息 |

## 图片字段说明

`images` 字段支持以下格式：

1. **字符串数组**（推荐）：
```yaml
images:
  - https://example.com/photo1.jpg
  - https://example.com/photo2.jpg
```

2. **单张图片**（字符串形式）：
```yaml
images: https://example.com/photo.jpg
```

::: tip
图片支持远程 URL 和本地路径。本地路径以 `/` 开头表示 `public` 目录。
:::

## 示例

### 基础说说

```markdown
---
published: 2024-06-20 10:30:00
author: 博主
avatar: https://example.com/avatar.png
tags:
  - 日常
  - 心情
---

今天天气真好！☀️

出去走走看看~
```

### 带图片的说说

```markdown
---
published: 2026-06-30 14:11:59
author: fqzlr
avatar: https://re.tsh520.cn/zl/tx.webp
tags:
  - 日常
  - 动漫
location: 郑州市-河南省
images:
  - https://ph.0824.uk/file/手机uu/1782828685076_237413170.jpg
---

大夏境内，神明禁行
```

### 置顶说说

```markdown
---
published: 2024-01-01 00:00:00
author: 博主
pinned: true
tags:
  - 公告
---

🎊 博客上线啦！欢迎来访~
```

### 带设备信息的说说

```markdown
---
published: 2024-06-20 18:00:00
author: 博主
device: iPhone 15 Pro
location: 北京市
tags:
  - 生活
---

刚下班，今天的晚霞真美~
```

## 文件命名规范

建议使用日期作为文件名，方便管理和排序：

```
src/content/moments/
├── 2026-06-30.md
├── 2026-06-29.md
├── 2026-06-20.md
└── ...
```

如果同一天有多条说说，可以在日期后加序号：

```
2026-06-10.md
2026-06-10-2.md
2026-06-10-3.md
```

## 排序规则

说说按 `published` 字段降序排列，最新发布的显示在最前面。置顶的说说（`pinned: true`）会优先显示。

::: tip
- 说说内容支持完整的 Markdown 语法
- 可以在正文中使用表情符号
- 图片支持点击放大查看
- 标签可用于分类筛选
:::

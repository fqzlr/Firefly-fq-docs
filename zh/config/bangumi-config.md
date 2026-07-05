# 番组计划配置详解

番组计划功能用于管理和展示你的动漫、书籍、游戏、音乐、影视等收藏内容。

数据目录：`src/content/bangumi/`
Schema 定义：`src/content.config.ts` 中的 `bangumiCollection`

## 目录结构

番组数据按分类存放在 `src/content/bangumi/` 目录下，每个分类一个子目录：

```
src/content/bangumi/
├── anime/          # 动漫/动画
├── book/           # 书籍
├── game/           # 游戏
├── music/          # 音乐
└── real/           # 影视（电影、电视剧等）
```

每个作品创建一个独立的文件，支持以下格式：
- `.md` / `.mdx` - Markdown 格式，使用 frontmatter 定义元数据
- `.yaml` / `.yml` - YAML 格式

## 分类说明

| 分类 | 目录名 | 说明 |
|------|--------|------|
| 动漫 | `anime` | 动画、番剧 |
| 书籍 | `book` | 图书、小说、漫画 |
| 游戏 | `game` | 电子游戏 |
| 音乐 | `music` | 音乐专辑、单曲 |
| 影视 | `real` | 电影、电视剧、纪录片 |

## 状态说明

状态使用数字 1-5 表示：

| 状态值 | 含义 | 说明 |
|--------|------|------|
| `1` | 想看 | 计划观看/阅读/游玩 |
| `2` | 看过 | 已完成 |
| `3` | 在看 | 进行中 |
| `4` | 搁置 | 暂时搁置 |
| `5` | 抛弃 | 已放弃 |

## 字段说明

### 通用字段

| 字段 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| `title` | `string` | 是 | - | 作品标题 |
| `name_cn` | `string` | 否 | - | 中文译名 |
| `category` | `string` | 否 | `"anime"` | 分类，可选值：`anime`、`book`、`game`、`music`、`real` |
| `subcategory` | `string` | 否 | - | 子分类，可选值：`movie`、`tv`、`anime`、`documentary` |
| `status` | `number` | 否 | `2` | 状态，1-5 |
| `image` | `string` | 是 | - | 封面图片 URL 或路径 |
| `link` | `string` | 否 | - | 对应文章链接；为空时自动从文件路径推导 |
| `score` | `number` | 否 | - | 评分，范围 0-10 |
| `comment` | `string` | 否 | - | 评语/观后感 |
| `tags` | `string[]` | 否 | `[]` | 标签数组 |
| `published` | `date` | 否 | - | 观看/阅读/发布日期 |

### 音乐专属字段

当 `category` 为 `music` 时，还支持以下字段：

| 字段 | 类型 | 说明 |
|------|------|------|
| `artist` | `string` | 艺术家/歌手 |
| `audioUrl` | `string` | 音频文件 URL |
| `lrcUrl` | `string` | 歌词文件 URL（LRC 格式） |
| `metingServer` | `string` | Meting API 音乐平台 |
| `metingId` | `string` | Meting API 歌曲 ID |

## 示例

### Markdown 格式示例（动漫）

```markdown
---
title: 阿甘正传
category: anime
status: 2
image: https://example.com/cover.jpg
score: 10
tags:
  - 剧情
  - 爱情
  - 传记
published: 2025-01-09
---

这里可以写观后感或详细介绍...
```

### Markdown 格式示例（音乐）

```markdown
---
title: 十年
category: music
status: 2
image: https://example.com/cover.jpg
artist: 陈奕迅
audioUrl: https://example.com/song.m4a
lrcUrl: https://example.com/song.lrc
score: 9
published: 2026-06-15
---
```

### YAML 格式示例

```yaml
title: 活着
category: book
status: 2
image: https://example.com/cover.jpg
score: 9.5
tags:
  - 文学
  - 小说
comment: 感人至深的作品
published: 2024-03-15
```

## 子目录支持

每个分类目录下可以创建子目录进行更细粒度的分类，例如：

```
src/content/bangumi/
└── book/
    ├── 个人成长/
    │   └── 人性的弱点.md
    ├── 文学/
    │   └── 追风筝的人.md
    └── 精品小说/
        └── 恶意.md
```

子目录仅用于组织文件，不影响数据读取，系统会递归读取所有文件。

::: tip
- `category` 字段如果不设置，默认值为 `anime`
- `status` 字段如果不设置，默认值为 `2`（看过）
- 封面图片 `image` 是必填字段
- 音乐类内容可以在音乐页面播放
:::

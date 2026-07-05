# 友链配置详解

友链配置文件用于管理友链页面的显示设置、申请方式和本站信息。友链数据存放在 `src/content/friends/` 目录下，每个 `.md` 文件代表一个友链。

配置文件路径：`src/config/friendsConfig.ts`

友链数据目录：`src/content/friends/`

## 友链页面配置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `columns` | `number` | `2` | 显示列数：2 列或 3 列 |
| `applyLink` | `boolean` | `true` | 是否开启友链申请功能 |

## GitHub Issue 申请配置

支持通过 GitHub Issue 自动申请友链，审核通过后自动添加：

| 配置项 | 类型 | 说明 |
|--------|------|------|
| `githubIssue.repo` | `string` | GitHub 仓库地址（格式：`owner/repo`） |
| `githubIssue.labels` | `string[]` | Issue 标签数组，用于筛选友链申请 |

### 示例

```ts
githubIssue: {
  repo: "fqzlr/my-blog",
  labels: ["friend-link"],
},
```

## 本站信息配置

用于在友链申请指南中展示本站信息：

| 配置项 | 类型 | 说明 |
|--------|------|------|
| `siteInfo.name` | `string` | 站点名称 |
| `siteInfo.desc` | `string` | 站点描述 |
| `siteInfo.url` | `string` | 站点 URL |
| `siteInfo.avatar` | `string` | 站点头像 URL |

### 示例

```ts
siteInfo: {
  name: "Fqzlr的博客",
  desc: "躬身入局，心为主理，行有尺度，自持本心",
  url: "https://fqzlr.com",
  avatar: "https://q1.qlogo.cn/g?b=qq&nk=20447289&s=640",
},
```

## 注意事项配置

友链申请页面中的注意事项列表，每项包含 `title` 和 `content`：

### 示例

```ts
notes: [
  {
    title: "互换原则",
    content: "请先将本站添加到您的友链页面，确认后会添加您的友链",
  },
  {
    title: "链接维护",
    content: "友链网站长期无法访问或内容违规，将会被移除",
  },
],
```

## 友链数据格式

友链数据存放在 `src/content/friends/` 目录下，每个 `.md` 文件代表一个友链，通过 frontmatter 配置：

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `title` | `string` | 是 | 友链站点名称 |
| `imgurl` | `string` | 是 | 站点头像 URL |
| `desc` | `string` | 是 | 站点描述 |
| `siteurl` | `string` | 是 | 站点链接 |
| `tags` | `string[]` | 否 | 标签数组，如 `["Blog", "技术"]` |
| `weight` | `number` | 否 | 排序权重，数值越大越靠前，默认 0 |
| `enabled` | `boolean` | 否 | 是否启用，默认 true |

### 示例：添加友链

在 `src/content/friends/` 目录下创建 `example.md`：

```markdown
---
title: 示例博客
imgurl: https://example.com/avatar.png
desc: 这是一个示例博客
siteurl: https://example.com/
tags: ["Blog", "技术"]
weight: 50
enabled: true
---
```

## 在线编辑友链

网站内置了在线编辑功能，支持在后台管理友链：

1. 访问 `/admin.html` 进入后台管理
2. 进入「友情链接」管理页面
3. 可以添加、编辑、删除友链
4. 修改后保存到本地，需要重新构建部署

::: tip
- 友链按 `weight` 降序排列，权重相同则按文件名字母顺序排列
- 设置 `enabled: false` 可以暂时隐藏友链而不删除
- 自定义底部内容可在 `src/content/spec/friends.md` 中编写
- 友链申请的 Issue 模板需要在 GitHub 仓库的 `.github/ISSUE_TEMPLATE/` 中配置
:::

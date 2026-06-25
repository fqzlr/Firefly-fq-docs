# 评论系统配置详解

评论系统配置文件用于设置博客使用的评论系统，支持多种主流评论系统。

配置文件路径：`commentConfig.ts`

## 基础配置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `type` | `"none" \| "twikoo" \| "waline" \| "giscus" \| "disqus" \| "artalk"` | `"twikoo"` | 评论系统类型，`none` 表示不启用评论系统 |

::: tip
同一时间只能启用一种评论系统。需要使用哪种评论系统，将 `type` 设置为对应值并填写相应配置即可。
:::

## Twikoo 配置

Twikoo 是一个简洁、安全、免费的静态网站评论系统，基于腾讯云开发或 Vercel 部署。

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `twikoo.envId` | `string` | `"https://pl.fqzlr.com"` | Twikoo 环境 ID 或服务地址 |
| `twikoo.lang` | `string` | `"zh-CN"` | 评论系统语言 |
| `twikoo.visitorCount` | `boolean` | `true` | 是否启用文章访问量统计 |

### 示例

```ts
type: "twikoo",
twikoo: {
  envId: "https://your-twikoo-domain.com",
  lang: "zh-CN",
  visitorCount: true,
},
```

::: tip
Twikoo 版本：1.7.4
:::

## Waline 配置

Waline 是一个简单、安全的评论系统，支持多种部署方式。

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `waline.serverURL` | `string` | `"https://comment.mmzhiku.xyz/"` | Waline 后端服务地址 |
| `waline.lang` | `string` | `"zh-CN"` | 评论系统语言 |
| `waline.emoji` | `string[]` | 见源文件 | 表情地址数组 |
| `waline.login` | `"enable" \| "force" \| "disable"` | `"enable"` | 评论登录模式 |
| `waline.visitorCount` | `boolean` | `true` | 是否启用文章访问量统计 |

### 登录模式说明

| 模式 | 说明 |
|------|------|
| `enable` | 默认，允许访客匿名评论和第三方 OAuth 登录，兼容性最佳 |
| `force` | 强制必须登录后才能评论，适合严格社区，关闭匿名评论 |
| `disable` | 禁止所有登录和 OAuth，仅允许匿名评论（填写昵称/邮箱） |

### 默认表情配置

```ts
emoji: [
  "https://unpkg.com/@waline/emojis@1.4.0/weibo",
  "https://unpkg.com/@waline/emojis@1.4.0/bilibili",
  "https://unpkg.com/@waline/emojis@1.4.0/bmoji",
],
```

### 示例

```ts
type: "waline",
waline: {
  serverURL: "https://waline.your-domain.com",
  lang: "zh-CN",
  emoji: [
    "https://unpkg.com/@waline/emojis@1.4.0/weibo",
  ],
  login: "enable",
  visitorCount: true,
},
```

## Artalk 配置

Artalk 是一款简洁的自托管评论系统。

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `artalk.server` | `string` | `"https://artalk.example.com/"` | Artalk 后端 API 地址 |
| `artalk.locale` | `string` | `"zh-CN"` | 评论系统语言 |
| `artalk.visitorCount` | `boolean` | `true` | 是否启用文章访问量统计 |

### 示例

```ts
type: "artalk",
artalk: {
  server: "https://artalk.your-domain.com",
  locale: "zh-CN",
  visitorCount: true,
},
```

## Giscus 配置

Giscus 是基于 GitHub Discussions 的评论系统，完全免费。

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `giscus.repo` | `string` | `"fqzlr/my-blog"` | GitHub 仓库名（格式：username/repo） |
| `giscus.repoId` | `string` | `"R_kgDOSXWjBQ"` | 仓库 ID |
| `giscus.category` | `string` | `"General"` | Discussions 分类名 |
| `giscus.categoryId` | `string` | `"DIC_kwDOSXWjBc4C8jP5"` | 分类 ID |
| `giscus.mapping` | `string` | `"pathname"` | 页面与 discussion 的映射方式 |
| `giscus.strict` | `string` | `"0"` | 是否启用严格模式 |
| `giscus.reactionsEnabled` | `string` | `"1"` | 是否启用反应功能 |
| `giscus.emitMetadata` | `string` | `"0"` | 是否发送讨论元数据 |
| `giscus.inputPosition` | `"top" \| "bottom"` | `"top"` | 评论输入框位置 |
| `giscus.lang` | `string` | `"zh-CN"` | 评论系统语言 |
| `giscus.loading` | `"lazy" \| "eager"` | `"lazy"` | 加载方式 |

### 映射方式说明

常用映射方式：
- `"pathname"`：使用页面路径
- `"url"`：使用页面 URL
- `"title"`：使用页面标题
- `"og:title"`：使用 OpenGraph title
- `"specific"`：使用特定术语
- `"number"`：使用特定 discussion 编号

### 获取配置参数

在 [Giscus 官网](https://giscus.app/) 按照指引配置后，可以获取到所有需要的参数。

### 示例

```ts
type: "giscus",
giscus: {
  repo: "yourname/your-blog",
  repoId: "your-repo-id",
  category: "General",
  categoryId: "your-category-id",
  mapping: "pathname",
  strict: "0",
  reactionsEnabled: "1",
  emitMetadata: "0",
  inputPosition: "top",
  lang: "zh-CN",
  loading: "lazy",
},
```

## Disqus 配置

Disqus 是一个全球流行的第三方评论系统。

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `disqus.shortname` | `string` | `"firefly"` | Disqus 站点短名 |

### 示例

```ts
type: "disqus",
disqus: {
  shortname: "your-disqus-shortname",
},
```

## 不启用评论系统

如果不需要评论功能，将 `type` 设置为 `"none"` 即可：

```ts
type: "none",
```

::: warning
注意：留言板页面（guestbook）需要配置评论系统才能正常使用。如果关闭评论系统，请在 [siteConfig.ts](./site-config.md) 中将 `pages.guestbook` 设为 `false`。
:::

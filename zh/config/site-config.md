# 站点配置详解

站点配置文件用于设置博客的基本信息、主题、布局、功能开关等全局配置。

配置文件路径：[siteConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/siteConfig.ts)

## 基本信息配置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `title` | `string` | `"Fqzlr的博客"` | 站点标题 |
| `subtitle` | `string` | `"Fqzlr"` | 站点副标题 |
| `site_url` | `string` | `"https://fqzlr.com/"` | 站点完整 URL，用于 SEO 和 RSS |
| `description` | `string` | 见源文件 | 站点描述，用于 SEO |
| `keywords` | `string[]` | 见源文件 | 站点关键词数组，用于 SEO |
| `lang` | `string` | `"zh_CN"` | 站点语言代码，如 `zh_CN`、`zh_TW`、`en`、`ja`、`ru` |

### 示例

```ts
title: "我的博客",
subtitle: "My Blog",
site_url: "https://example.com/",
description: "这是我的个人技术博客",
keywords: ["博客", "技术", "编程"],
lang: "zh_CN",
```

## 主题色配置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `themeColor.hue` | `number` | `165` | 主题色的色相值，范围 0-360。红色：0，青色：200，蓝绿色：250，粉色：345 |
| `themeColor.fixed` | `boolean` | `false` | 是否对访问者隐藏主题色选择器 |
| `themeColor.defaultMode` | `"light" \| "dark"` | `"light"` | 默认主题模式：`light` 亮色，`dark` 暗色 |

### 示例

```ts
themeColor: {
  hue: 200, // 青色主题
  fixed: false,
  defaultMode: "dark", // 默认暗色模式
},
```

## 页面布局配置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `pageWidth` | `number` | `100` | 页面整体宽度（单位：rem），数值越大内容区域越宽 |
| `card.border` | `boolean` | `true` | 是否开启卡片边框和阴影，开启后更有立体感 |
| `card.followTheme` | `boolean` | `false` | 是否让卡片风格跟随主题色相 |

### 示例

```ts
pageWidth: 90,
card: {
  border: true,
  followTheme: true,
},
```

## Favicon 配置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `favicon` | `Array` | 见源文件 | Favicon 图标数组，支持不同尺寸 |

每个 Favicon 项包含：
- `src`: 图标路径
- `sizes`: 图标尺寸，如 `"32x32"`、`"192x192"`、`"any"`

### 示例

```ts
favicon: [
  { src: "/favicon/favicon.ico", sizes: "32x32" },
  { src: "/favicon/favicon.svg", sizes: "any" },
],
```

## 导航栏配置

导航栏配置在 `navbar` 对象中，详细配置请参考 [导航栏配置](./navbar-config.md)。

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `navbar.logo` | `object` | 见源文件 | 导航栏 Logo，支持图标、本地图片、网络图片 |
| `navbar.title` | `string` | `"Fqzlr的博客"` | 导航栏标题 |
| `navbar.widthFull` | `boolean` | `false` | 导航栏是否占满屏幕宽度 |
| `navbar.menuAlign` | `"left" \| "center"` | `"center"` | 导航菜单对齐方式 |
| `navbar.followTheme` | `boolean` | `false` | 导航栏图标和标题是否跟随主题色 |
| `navbar.stickyNavbar` | `boolean` | `true` | 导航栏是否固定在顶部 |

## 时间配置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `siteStartDate` | `string` | `"2026-04-12"` | 站点开始日期，用于统计运行天数 |
| `timezone` | `string` | `"Asia/Shanghai"` | 站点时区（IANA 时区字符串），如 `"Asia/Shanghai"`、`"UTC"`，留空则使用服务器时区 |
| `workHours.start` | `number` | `9` | 上班时间（24小时制），用于首页头像动效 |
| `workHours.end` | `number` | `18` | 下班时间（24小时制） |
| `workHours.workDays` | `number[]` | `[1,2,3,4,5,6]` | 工作日范围，0=周日，1=周一...6=周六 |

### 示例

```ts
siteStartDate: "2024-01-01",
timezone: "Asia/Shanghai",
workHours: {
  start: 9,
  end: 18,
  workDays: [1, 2, 3, 4, 5], // 周一到周五
},
```

## 提醒框配置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `rehypeCallouts.theme` | `"github" \| "obsidian" \| "vitepress"` | `"github"` | 提醒框主题风格，修改后需重启开发服务器 |

### 示例

```ts
rehypeCallouts: {
  theme: "vitepress",
},
```

::: tip
每个主题风格和语法不同，可根据个人喜好选择。
:::

## 文章配置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `showLastModified` | `boolean` | `true` | 文章页底部是否显示"上次编辑时间"卡片 |
| `outdatedThreshold` | `number` | `30` | 文章过期阈值（天数），超过此天数才显示"上次编辑"卡片 |
| `sharePoster` | `boolean` | `true` | 是否开启分享海报生成功能 |
| `generateOgImages` | `boolean` | `false` | 是否开启 OpenGraph 图片生成，开启后构建时间较长 |
| `defaultOgImage` | `string` | `"/assets/images/aut.webp"` | 默认 OG 图片路径 |

::: warning
`generateOgImages` 开启后渲染时间很长，不建议本地调试时开启。
:::

## 页面开关配置

通过 `pages` 对象控制特定页面的访问权限，设为 `false` 会返回 404：

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `pages.friends` | `boolean` | `true` | 友链页面开关 |
| `pages.sponsor` | `boolean` | `true` | 赞助页面开关 |
| `pages.guestbook` | `boolean` | `true` | 留言板页面开关（需配置评论系统） |
| `pages.gallery` | `boolean` | `true` | 相册页面开关 |
| `pages.collections` | `boolean` | `true` | 收藏 API 页面开关 |
| `pages.stats` | `boolean` | `true` | 统计页面开关 |
| `pages.calendar` | `boolean` | `true` | 日历页面开关 |
| `pages.bangumi` | `boolean` | `true` | 番剧页面开关 |
| `pages.books` | `boolean` | `true` | 书架页面开关 |
| `pages.moviesGames` | `boolean` | `true` | 影视游戏页面开关 |
| `pages.musicPage` | `boolean` | `true` | 音乐页面开关 |
| `pages.changelog` | `boolean` | `true` | 更新日志页面开关 |
| `pages.moments` | `boolean` | `true` | 动态页面开关 |
| `pages.admin` | `boolean` | `true` | 后台管理页面开关 |
| `pages.lifeRoutines` | `boolean` | `true` | 日常规划页面开关 |
| `pages.lifePlaces` | `boolean` | `true` | 旅行足迹页面开关 |
| `pages.lifeNotebooks` | `boolean` | `true` | 笔记本页面开关 |

## 分类导航栏

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `categoryBar` | `boolean` | `true` | 是否在首页和归档页顶部显示分类快捷导航 |

## 文章列表布局配置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `postListLayout.defaultMode` | `"list" \| "grid"` | `"list"` | 默认布局模式：`list` 列表模式（单列），`grid` 网格模式（多列） |
| `postListLayout.mobileDefaultMode` | `"list" \| "grid"` | `"list"` | 移动端默认布局模式，不设置则跟随 `defaultMode` |
| `postListLayout.showTags` | `boolean` | `true` | 是否在文章列表中显示标签 |
| `postListLayout.descriptionLines` | `number` | `2` | 文章简介显示行数，设为 0 则不截断 |
| `postListLayout.allowSwitch` | `boolean` | `true` | 是否允许用户切换布局 |
| `postListLayout.grid.masonry` | `boolean` | `false` | 是否开启瀑布流布局，有封面图和无封面图混合时推荐开启 |
| `postListLayout.grid.columnWidth` | `number` | `320` | 网格模式卡片最小宽度(px)，浏览器自动计算列数 |

### 示例

```ts
postListLayout: {
  defaultMode: "grid",
  mobileDefaultMode: "list",
  showTags: true,
  descriptionLines: 3,
  allowSwitch: true,
  grid: {
    masonry: true,
    columnWidth: 300,
  },
},
```

## 分页配置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `pagination.postsPerPage` | `number` | `10` | 每页显示的文章数量 |

## 统计分析配置

### Google Analytics

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `analytics.googleAnalyticsId` | `string` | `""` | Google Analytics ID |

### Microsoft Clarity

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `analytics.microsoftClarityId` | `string` | `""` | Microsoft Clarity ID |

### Umami 统计

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `analytics.umamiAnalytics.websiteId` | `string` | 见源文件 | Umami 网站 ID |
| `analytics.umamiAnalytics.shareId` | `string` | 见源文件 | Umami 分享 ID |
| `analytics.umamiAnalytics.scriptUrl` | `string` | 见源文件 | Umami 脚本 URL |
| `analytics.umamiAnalytics.trackOutboundLinks` | `boolean` | `true` | 是否追踪出站链接 |
| `analytics.umamiAnalytics.collectWebVitals` | `boolean` | `false` | 是否收集浏览器性能指标 |
| `analytics.umamiAnalytics.relpays.enabled` | `boolean` | `false` | 是否启用会话回放 |
| `analytics.umamiAnalytics.relpays.sampleRate` | `number` | `0.15` | 录制会话采样率，范围 0-1，0.15 表示记录 15% 的会话 |
| `analytics.umamiAnalytics.relpays.maskLevel` | `"moderate" \| "strict"` | `"moderate"` | 隐私遮罩级别：`moderate` 遮罩所有输入框；`strict` 额外遮罩全部文本 |
| `analytics.umamiAnalytics.relpays.maxDuration` | `number` | `300000` | 单次录制最大时长（毫秒） |
| `analytics.umamiAnalytics.relpays.blockSelector` | `string` | `""` | 需要排除录制的元素 CSS 选择器 |

### 51la 统计

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `analytics.la51Analytics.Id` | `string` | `""` | 51la 统计 ID |
| `analytics.la51Analytics.sdkUrl` | `string` | `""` | 自定义 SDK JS 地址，防止 DNS 污染，留空使用默认地址 |
| `analytics.la51Analytics.ck` | `string` | `""` | 多个统计 ID 的数据分离标识，留空则使用 Id |
| `analytics.la51Analytics.autoTrack` | `boolean` | `false` | 是否开启事件分析功能 |
| `analytics.la51Analytics.hashMode` | `boolean` | `false` | Hash 路由模式，项目使用 History API 路由，不必开启 |
| `analytics.la51Analytics.screenRecord` | `boolean` | `true` | 是否开启网站录屏功能 |

## 热力图配置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `heatmap.github.enabled` | `boolean` | `true` | 是否启用 GitHub 贡献热力图 |
| `heatmap.github.username` | `string` | `"fqzlr"` | GitHub 用户名 |

## 图像优化配置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `imageOptimization.formats` | `"avif" \| "webp" \| "both"` | `"webp"` | 输出图片格式：`avif` 体积最小兼容性低；`webp` 体积适中兼容性好；`both` 同时输出两种格式（推荐） |
| `imageOptimization.quality` | `number` | `85` | 图片压缩质量 (1-100)，推荐 70-85 |
| `imageOptimization.noReferrerDomains` | `string[]` | `[]` | 为指定域名图片添加 `referrerpolicy="no-referrer"` 属性，支持通配符 `*`，可解决防盗链 403 问题 |

::: tip
Astro 仅能对 `src` 目录下的图像进行优化，`src` 目录下的图像越多，构建时间会越长。
:::

### 示例

```ts
imageOptimization: {
  formats: "both",
  quality: 80,
  noReferrerDomains: ["i0.hdslb.com", "*.bilibili.com"],
},
```

## 备案号配置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `beian` | `string` | `"萌ICP备20268200号"` | ICP 备案号，留空则不显示 |
| `policeBeian` | `string` | `"萌ICP备20268200号"` | 公安网备号，留空则不显示 |

## 字体配置

字体配置引用自 `fontConfig`，详细配置请参考 [字体配置](./font-config.md)。

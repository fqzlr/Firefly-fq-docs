# 项目结构详解

Firefly 博客是基于 Astro 框架构建的现代化静态博客主题，采用 TypeScript、Svelte 组件和 Tailwind CSS 技术栈。本文档将详细介绍项目的目录结构和各模块功能。

## 目录概览

```
my-blog/
├── .github/                    # GitHub 配置（CI/CD、Issue 模板等）
├── docs/                       # VitePress 文档目录
├── public/                     # 静态资源目录（不经过构建处理）
├── scripts/                    # 构建辅助脚本
├── src/                        # 源代码目录（核心）
├── .env.example                # 环境变量示例
├── astro.config.mjs            # Astro 配置文件
├── biome.json                  # Biome 代码格式化配置
├── package.json                # 项目依赖配置
└── pnpm-lock.yaml              # pnpm 依赖锁定文件
```

## 核心目录详解

### src/ - 源代码目录

这是项目的核心目录，包含所有业务逻辑、组件和配置。

#### src/config/ - 配置文件目录

所有站点配置集中在此目录，采用 TypeScript 编写，提供完整的类型提示。

| 配置文件 | 功能说明 |
|---------|---------|
| [siteConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/siteConfig.ts) | 站点基础配置（标题、URL、主题色、页面开关等） |
| [musicConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/musicConfig.ts) | 音乐播放器配置（Meting API、本地歌单等） |
| [commentConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/commentConfig.ts) | 评论系统配置（Giscus、Twikoo、Waline等） |
| [navBarConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/navBarConfig.ts) | 导航栏菜单配置 |
| [sidebarConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/sidebarConfig.ts) | 侧边栏组件配置 |
| [profileConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/profileConfig.ts) | 个人资料卡片配置 |
| [footerConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/footerConfig.ts) | 页脚链接配置 |
| [fontConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/fontConfig.ts) | 自定义字体配置 |
| [friendsConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/friendsConfig.ts) | 友链页面配置 |
| [galleryConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/galleryConfig.ts) | 相册页面配置 |
| [adConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/adConfig.ts) | 广告组件配置 |
| [announcementConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/announcementConfig.ts) | 公告组件配置 |
| [sponsorConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/sponsorConfig.ts) | 赞助页面配置 |
| [licenseConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/licenseConfig.ts) | 文章默认版权配置 |
| [coverImageConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/coverImageConfig.ts) | 文章封面图配置 |
| [backgroundWallpaper.ts](file:///e:/AItool/zzwork/my-blog/src/config/backgroundWallpaper.ts) | 背景壁纸配置 |
| [pioConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/pioConfig.ts) | Live2D/Spine 看板娘配置 |
| [aiSearchConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/aiSearchConfig.ts) | AI 搜索配置 |
| [calendarConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/calendarConfig.ts) | 日历功能配置 |
| [plantumlConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/plantumlConfig.ts) | PlantUML 图表配置 |
| [expressiveCodeConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/expressiveCodeConfig.ts) | 代码块高亮配置 |
| [index.ts](file:///e:/AItool/zzwork/my-blog/src/config/index.ts) | 配置统一导出入口 |

::: tip 配置修改提示
大部分配置文件修改后会自动热更新，但部分配置（如 `rehypeCallouts` 主题）需要重启开发服务器才能生效。
:::

#### src/components/ - 组件目录

采用 Astro + Svelte 混合组件架构，按功能模块组织。

```
src/components/
├── about/              # 关于页面组件
│   └── AboutCanvas.svelte
├── analytics/          # 统计分析组件
│   ├── GoogleAnalytics.astro
│   ├── La51Analytics.astro
│   ├── MicrosoftClarity.astro
│   └── UmamiAnalytics.astro
├── comment/            # 评论系统组件
│   ├── Artalk.astro
│   ├── Disqus.astro
│   ├── Giscus.astro
│   ├── Twikoo.astro
│   ├── Waline.astro
│   └── index.astro     # 评论组件入口
├── common/             # 通用基础组件
│   ├── ButtonLink.astro
│   ├── ButtonTag.astro
│   ├── ClientPagination.astro
│   ├── CoverImage.astro
│   ├── DropdownItem.astro
│   ├── FloatingButton.astro
│   ├── GlassSurface.svelte
│   ├── Icon.astro
│   ├── Icon.svelte
│   ├── ImageWrapper.astro
│   ├── Markdown.astro
│   ├── NumberTicker.svelte
│   ├── PageTitle.astro
│   ├── Pagination.astro
│   ├── PioMessageBox.astro
│   ├── VisitorCount.astro
│   ├── WidgetLayout.astro
│   └── portal.ts
├── controls/           # 交互控制组件
│   ├── AISearch.svelte
│   ├── AnimatedTabs.svelte
│   ├── ArchivePanel.svelte
│   ├── FloatingDock.astro
│   ├── LightDarkSwitch.svelte
│   ├── Search.svelte
│   └── SearchModal.svelte
├── features/           # 核心功能组件
│   ├── music-visualizer/   # 3D 音乐可视化
│   │   ├── AudioAnalyzer.ts
│   │   ├── LyricsOverlay.svelte
│   │   ├── MusicVisualizer.svelte
│   │   ├── ThreeScene.svelte
│   │   └── VisualizerControls.svelte
│   ├── BubbleMenu.svelte
│   ├── CategoryBubbleMenu.svelte
│   ├── DataMetricCard.astro
│   ├── EncryptedPost.astro      # 加密文章
│   ├── FancyboxManager.astro    # 图片灯箱
│   ├── FloatingLyrics.astro     # 悬浮歌词
│   ├── FontManager.astro        # 字体管理
│   ├── FriendCard.astro
│   ├── KatexManager.astro       # 数学公式
│   ├── Live2DWidget.astro       # Live2D 看板娘
│   ├── LogoLoop.svelte
│   ├── MusicManager.astro       # 音乐管理器
│   ├── MusicPlayer.astro        # 音乐播放器
│   ├── PageLoader.astro
│   ├── PrivacyModal.astro
│   ├── SpineModel.astro         # Spine 模型
│   └── TypewriterText.astro
├── layout/             # 布局相关组件
│   ├── CategoryBar.astro
│   ├── ConfigCarrier.astro
│   ├── DropdownMenu.astro
│   ├── Footer.astro
│   ├── HomeDataLayer.astro
│   ├── HomeHero.astro
│   ├── HomeTicker.astro
│   ├── NavMenuPanel.astro
│   ├── Navbar.astro
│   ├── PostCard.astro
│   ├── PostListActions.astro
│   ├── PostMeta.astro
│   └── PostPage.astro
├── misc/               # 杂项组件
│   ├── License.astro
│   ├── PostFooterActions.astro
│   ├── RecommendedPost.astro
│   └── SharePoster.svelte
├── moments/            # 动态/说说组件
│   ├── MomentCard.astro
│   └── MomentsCover.astro
├── pages/              # 页面级组件
│   ├── bangumi/        # 番剧页面
│   ├── books/          # 书架页面
│   ├── calendar/       # 日历页面
│   ├── gallery/        # 相册页面
│   ├── movies-games/   # 影视游戏页面
│   ├── music/          # 音乐页面
│   ├── AdvancedSearch.svelte
│   └── ArticleVirtualList.svelte
└── widget/             # 侧边栏/首页小部件
    ├── Advertisement.astro
    ├── Announcement.astro
    ├── ArchiveHeatmap.astro
    ├── Calendar.astro
    ├── Categories.astro
    ├── CategoryRose.astro
    ├── GithubHeatmap.astro
    ├── Music.astro
    ├── PostHeatmap.astro
    ├── Profile.astro
    ├── SidebarTOC.astro
    ├── TagBubble.astro
    ├── TagCardWall.astro
    ├── TagGraph.astro
    ├── TagWordcloud.astro
    ├── Tags.astro
    └── TerrariumModel.astro
```

::: tip 组件类型说明
- `.astro` 文件：Astro 组件，适合静态内容渲染，在构建时执行
- `.svelte` 文件：Svelte 组件，适合需要客户端交互的动态功能
- `.ts` 文件：TypeScript 工具类和逻辑模块
:::

#### src/content/ - 内容集合目录

使用 Astro Content Collections 管理所有内容，支持类型安全的数据验证。

```
src/content/
├── posts/              # 博客文章（Markdown/MDX）
│   ├── blog/
│   ├── Firefly-set/
│   ├── guide/
│   ├── images/
│   ├── code-examples.md
│   ├── markdown-tutorial.md
│   └── ...
├── spec/               # 特殊页面（关于、友链、留言板等）
│   ├── about.md
│   ├── friends.mdx
│   ├── guestbook.md
│   └── privacy.md
├── moments/            # 动态/说说
│   ├── assets/
│   └── YYYY-MM-DD.md
├── bangumi/            # 番剧/影视/音乐/游戏/书籍
│   ├── anime/
│   ├── book/
│   ├── game/
│   └── music/
├── changelog/          # 更新日志
│   └── YYYY-MM-DD-xxx.md
└── life/               # 生活记录（笔记本、日程、旅行等）
    ├── meta/
    ├── notebooks/
    ├── places/
    └── routines/
```

内容 Schema 定义在 [content.config.ts](file:///e:/AItool/zzwork/my-blog/src/content.config.ts) 中，包含完整的字段类型验证。

#### src/pages/ - 页面路由目录

Astro 基于文件的路由系统，每个 `.astro` 文件对应一个页面路由。

```
src/pages/
├── index.astro             # 首页
├── posts/[...slug].astro   # 文章详情页
├── list.astro              # 文章列表页
├── archive.astro           # 归档页
├── categories.astro        # 分类页
├── tags/                   # 标签相关
├── about.astro             # 关于页
├── friends.astro           # 友链页
├── guestbook.astro         # 留言板页
├── gallery/                # 相册页
├── music/                  # 音乐页
├── bangumi/                # 番剧页
├── books/                  # 书架页
├── calendar/               # 日历页
├── changelog/              # 更新日志页
├── moments/                # 动态页
├── sponsor/                # 赞助页
├── search.astro            # 搜索页
├── collections.astro       # API 收藏页
├── admin/                  # 后台管理页
├── life/                   # 生活相关页面
│   ├── notebooks/
│   ├── places.astro
│   └── routines.astro
├── api/                    # API 端点
│   ├── allPostMeta.json.ts
│   └── holidays.json.ts
├── og/[...slug].png.ts     # OG 图片生成
├── rss.xml.ts              # RSS 订阅
├── robots.txt.ts           # robots.txt
└── 404.astro               # 404 页面
```

#### src/styles/ - 样式目录

使用 Stylus 和 CSS 组织样式，按功能模块拆分。

```
src/styles/
├── components/         # 组件样式
├── features/           # 功能页面样式
├── layout/             # 布局样式
├── pages/              # 页面样式
├── widgets/            # 小部件样式
├── main.css            # 全局主样式
├── variables.styl      # Stylus 变量
├── markdown.css        # Markdown 内容样式
├── markdown-extend.styl # Markdown 扩展样式
├── custom-scrollbar.css # 自定义滚动条
├── transition.css      # 页面过渡动画
└── ...
```

#### src/i18n/ - 国际化目录

支持多语言切换，目前包含中文简体、中文繁体、英文、日文、俄文。

```
src/i18n/
├── languages/
│   ├── zh_CN.ts
│   ├── zh_TW.ts
│   ├── en.ts
│   ├── ja.ts
│   └── ru.ts
├── i18nKey.ts          # 国际化键定义
└── translation.ts      # 翻译工具函数
```

#### src/utils/ - 工具函数目录

```
src/utils/
├── about/              # 关于页面相关工具
├── cache-utils.ts      # 缓存工具
├── calendar-events.ts  # 日历事件处理
├── content-utils.ts    # 内容处理工具
├── crypto-utils.ts     # 加密工具
├── date-utils.ts       # 日期处理工具
├── gallery-utils.ts    # 相册工具
├── image-utils.ts      # 图片处理工具
├── navigation-utils.ts # 导航工具
├── setting-utils.ts    # 设置工具
├── tag-graph-data.ts   # 标签图谱数据
├── toc-utils.ts        # 目录生成工具
├── url-utils.ts        # URL 处理工具
└── ...
```

#### src/types/ - 类型定义目录

```
src/types/
├── config.ts           # 配置类型定义
├── post.ts             # 文章类型定义
├── bangumi.ts          # 番剧类型定义
└── guestbook.ts        # 留言板类型定义
```

#### 其他重要目录/文件

| 路径 | 说明 |
|------|------|
| [src/layouts/](file:///e:/AItool/zzwork/my-blog/src/layouts/) | 页面布局组件（Layout.astro、MainGridLayout.astro） |
| [src/plugins/](file:///e:/AItool/zzwork/my-blog/src/plugins/) | Markdown 插件（rehype/remark 插件） |
| [src/constants/](file:///e:/AItool/zzwork/my-blog/src/constants/) | 常量定义（图标、链接预设等） |
| [src/workers/](file:///e:/AItool/zzwork/my-blog/src/workers/) | Web Worker（AI 聊天、留言板） |
| [src/env.d.ts](file:///e:/AItool/zzwork/my-blog/src/env.d.ts) | Astro 环境类型声明 |
| [src/global.d.ts](file:///e:/AItool/zzwork/my-blog/src/global.d.ts) | 全局类型声明 |

### public/ - 静态资源目录

此目录下的文件会被直接复制到构建输出目录，不经过 Astro 处理。

```
public/
├── assets/
│   ├── css/            # 第三方 CSS（代码高亮、Twikoo 样式等）
│   ├── images/         # 图片资源（首页图片、加载动画等）
│   ├── js/             # 第三方 JS（marked、highlight.js 等）
│   └── music/          # 本地音乐文件和封面
├── favicon/            # 网站图标（各种尺寸）
├── fonts/              # 自定义字体文件
├── gallery/            # 相册图片
├── models/             # 3D 模型文件（.glb）
└── pio/                # Live2D/Spine 模型资源
```

### scripts/ - 构建脚本目录

| 脚本 | 功能 |
|------|------|
| [new-post.js](file:///e:/AItool/zzwork/my-blog/scripts/new-post.js) | 创建新文章的命令行工具 |
| [generate-icons.js](file:///e:/AItool/zzwork/my-blog/scripts/generate-icons.js) | 生成图标雪碧图 |
| [build-vectorize-index.js](file:///e:/AItool/zzwork/my-blog/scripts/build-vectorize-index.js) | 构建向量化搜索索引 |

使用 `pnpm new-post` 命令可以快速创建新文章。

### docs/ - 文档目录

使用 VitePress 构建的项目文档，即你正在阅读的文档所在位置。

```
docs/
├── .vitepress/
│   └── config.ts       # VitePress 配置
├── zh/guide/           # 中文指南文档
├── demo/               # 演示文件
└── images/             # 文档图片
```

## 配置文件说明

### 根目录配置文件

| 文件 | 说明 |
|------|------|
| [astro.config.mjs](file:///e:/AItool/zzwork/my-blog/astro.config.mjs) | Astro 核心配置（集成、Markdown、构建选项等） |
| [package.json](file:///e:/AItool/zzwork/my-blog/package.json) | 项目依赖和 npm 脚本 |
| [biome.json](file:///e:/AItool/zzwork/my-blog/biome.json) | Biome 代码检查和格式化配置 |
| [postcss.config.mjs](file:///e:/AItool/zzwork/my-blog/postcss.config.mjs) | PostCSS 配置 |
| [pagefind.yml](file:///e:/AItool/zzwork/my-blog/pagefind.yml) | Pagefind 全文搜索配置 |
| [.env.example](file:///e:/AItool/zzwork/my-blog/.env.example) | 环境变量示例文件 |
| [tsconfig.json](file:///e:/AItool/zzwork/my-blog/tsconfig.json) | TypeScript 配置 |

::: warning 注意事项
1. 修改 `astro.config.mjs` 后必须重启开发服务器
2. `src/config/` 下的配置大部分支持热更新
3. 新增内容集合需要在 `content.config.ts` 中定义 Schema
4. 静态资源放在 `public/` 目录，需要导入处理的资源放在 `src/assets/` 目录
:::

## 关键技术栈

| 技术 | 版本 | 用途 |
|------|------|------|
| [Astro](https://astro.build/) | 6.x | 静态站点框架 |
| [Svelte 5](https://svelte.dev/) | 5.x | 交互式组件 |
| [TypeScript](https://www.typescriptlang.org/) | 5.x | 类型安全 |
| [Tailwind CSS](https://tailwindcss.com/) | 4.x | 原子化 CSS |
| [Three.js](https://threejs.org/) | 0.184.x | 3D 音乐可视化 |
| [Expressive Code](https://expressive-code.com/) | 0.41.x | 代码块高亮 |
| [Pagefind](https://pagefind.app/) | 1.x | 客户端全文搜索 |
| [Swup](https://swup.js.org/) | 1.x | 页面无刷新过渡 |
| [KaTeX](https://katex.org/) | 0.16.x | 数学公式渲染 |
| [Mermaid](https://mermaid.js.org/) | - | 流程图/图表渲染 |
| [GSAP](https://greensock.com/gsap/) | 3.x | 动画库 |

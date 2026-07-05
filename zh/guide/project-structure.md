# 项目结构详解

Firefly 博客是基于 Astro 框架构建的现代化静态博客主题，采用 TypeScript、Svelte 组件和 Tailwind CSS 技术栈。本文档将详细介绍项目的目录结构和各模块功能。

## 目录概览

```
my-blog/
├── .github/                    # GitHub 配置（CI/CD、Issue 模板等）
├── api/                        # 服务端 API（GitHub 代理等）
├── public/                     # 静态资源目录（不经过构建处理）
├── scripts/                    # 构建辅助脚本
├── src/                        # 源代码目录（核心）
├── _backup/                    # 备份文件（内容备份、原始文档等）
├── .env.example                # 环境变量示例
├── astro.config.mjs            # Astro 配置文件
├── biome.json                  # Biome 代码格式化配置
├── package.json                # 项目依赖配置
├── pnpm-lock.yaml             # pnpm 依赖锁定文件
├── pagefind.yml                # Pagefind 搜索配置
├── postcss.config.mjs          # PostCSS 配置
└── tsconfig.json               # TypeScript 配置
```

## 核心目录详解

### src/ - 源代码目录

这是项目的核心目录，包含所有业务逻辑、组件和配置。

#### src/config/ - 配置文件目录

所有站点配置集中在此目录，采用 TypeScript 编写，提供完整的类型提示。

| 配置文件 | 功能说明 |
|---------|---------|
| `siteConfig.ts` | 站点基础配置（标题、URL、主题色、页面开关等） |
| `musicConfig.ts` | 音乐播放器配置（Meting API、本地歌单等） |
| `commentConfig.ts` | 评论系统配置（Giscus、Twikoo、Waline等） |
| `navBarConfig.ts` | 导航栏菜单配置 |
| `sidebarConfig.ts` | 侧边栏组件配置 |
| `profileConfig.ts` | 个人资料卡片配置 |
| `footerConfig.ts` | 页脚链接配置 |
| `fontConfig.ts` | 自定义字体配置 |
| `friendsConfig.ts` | 友链页面配置 |
| `adConfig.ts` | 广告组件配置 |
| `announcementConfig.ts` | 公告组件配置 |
| `sponsorConfig.ts` | 赞助页面配置 |
| `licenseConfig.ts` | 文章默认版权配置 |
| `coverImageConfig.ts` | 文章封面图配置 |
| `backgroundWallpaper.ts` | 背景壁纸配置 |
| `pioConfig.ts` | Live2D/Spine 看板娘配置 |
| `expressiveCodeConfig.ts` | 代码块高亮配置 |
| `editConfig.ts` | 在线编辑功能配置 |
| `categoryLinks.ts` | 分类链接配置 |
| `folderIconConfig.ts` | 文件夹图标配置 |
| `homePortfolioShutterConfig.ts` | 首页百叶窗效果配置 |
| `projectsConfig.ts` | 项目展示配置 |
| `relationshipConfig.ts` | 恋爱计时器配置 |
| `sakuraConfig.ts` | 樱花飘落特效配置 |
| `skillsConfig.ts` | 技能展示配置 |
| `index.ts` | 配置统一导出入口 |

::: tip 配置修改提示
大部分配置文件修改后会自动热更新，但部分配置（如 `rehypeCallouts` 主题）需要重启开发服务器才能生效。
:::

#### src/components/ - 组件目录

采用 Astro + Svelte 混合组件架构，按功能模块组织。

```
src/components/
├── analytics/          # 统计分析组件
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
│   ├── DropdownItem.svelte
│   ├── DropdownPanel.astro
│   ├── DropdownPanel.svelte
│   ├── FloatingButton.astro
│   ├── Icon.astro
│   ├── Icon.svelte
│   ├── ImageWrapper.astro
│   ├── Markdown.astro
│   ├── PageTitle.astro
│   ├── Pagination.astro
│   ├── PioMessageBox.astro
│   └── WidgetLayout.astro
├── controls/           # 交互控制组件
│   ├── ArchivePanel.svelte
│   ├── BackToHome.astro
│   ├── BackToTop.astro
│   ├── CategoryTools.astro
│   ├── DisplaySettings.svelte
│   ├── DisplaySettingsIntegrated.svelte
│   ├── FloatingControls.astro
│   ├── FloatingDock.astro
│   ├── LayoutSwitchButton.svelte
│   ├── LightDarkSwitch.svelte
│   ├── Search.svelte
│   └── WallpaperSwitch.svelte
├── edit/               # 在线编辑组件
│   ├── AlbumEditor.svelte
│   ├── BangumiEditor.svelte
│   ├── ChangelogEditor.svelte
│   ├── ConfigEditor.svelte
│   ├── DaohangEditor.svelte
│   ├── EditToast.svelte
│   ├── EditToolbar.svelte
│   ├── FriendsEditor.svelte
│   ├── MarkdownPageEditor.svelte
│   ├── MomentsEditor.svelte
│   ├── NotebooksEditor.svelte
│   ├── PlacesEditor.svelte
│   ├── RoutinesEditor.svelte
│   ├── SponsorEditor.svelte
│   └── WriteEditor.svelte
├── features/           # 核心功能组件
│   ├── music-visualizer/   # 3D 音乐可视化
│   │   ├── AudioAnalyzer.ts
│   │   ├── LyricsOverlay.svelte
│   │   ├── MusicVisualizer.svelte
│   │   ├── ThreeScene.svelte
│   │   └── VisualizerControls.svelte
│   ├── BounceCards.astro
│   ├── DataMetricCard.astro
│   ├── DomeGallery.astro
│   ├── FancyboxManager.astro    # 图片灯箱
│   ├── FontManager.astro        # 字体管理
│   ├── FriendCard.astro
│   ├── FriendRulesModal.astro
│   ├── GuestbookCardStack.svelte
│   ├── GuestbookDataProvider.svelte
│   ├── GuestbookDetailModal.astro
│   ├── KatexManager.astro       # 数学公式
│   ├── Live2DWidget.astro       # Live2D 看板娘
│   ├── LogoLoop.svelte
│   ├── MagicRings.ts
│   ├── MusicManager.astro       # 音乐管理器
│   ├── MusicPlayer.astro        # 音乐播放器
│   ├── PageLoader.astro
│   ├── SakuraEffect.astro       # 樱花特效
│   ├── SpineModel.astro         # Spine 模型
│   ├── TypewriterText.astro
│   ├── WelcomeCard.astro
│   └── WelcomeToast.astro
├── guestbook/          # 留言板组件
│   └── Danmaku.astro
├── layout/             # 布局相关组件
│   ├── CategoryBar.astro
│   ├── ConfigCarrier.astro
│   ├── DropdownMenu.astro
│   ├── Footer.astro
│   ├── HomeDataLayer.astro
│   ├── HomeHero.astro
│   ├── HomePortal.astro
│   ├── HomePortfolioShutterLayer.astro
│   ├── HomeTicker.astro
│   ├── MobilePostToolbar.astro
│   ├── NavMenuPanel.astro
│   ├── Navbar.astro
│   ├── PostCard.astro
│   ├── PostMeta.astro
│   ├── PostPage.astro
│   └── SideBar.astro
├── life/               # 生活记录组件
│   ├── ActivityHeatmap.astro
│   ├── CompareCard.astro
│   ├── MiniLineChart.astro
│   └── RingProgress.astro
├── misc/               # 杂项组件
│   ├── License.astro
│   ├── RelatedPosts.astro
│   └── SharePoster.svelte
├── moments/            # 动态/说说组件
│   ├── MomentCard.astro
│   └── MomentsCover.astro
├── pages/              # 页面级组件
│   ├── bangumi/        # 番剧页面
│   │   ├── BangumiSection.astro
│   │   ├── Card.astro
│   │   ├── FilterControls.astro
│   │   └── TabNav.astro
│   ├── books/          # 书架页面
│   │   ├── BookCard.astro
│   │   └── Bookshelf.astro
│   ├── movies-games/   # 影视游戏页面
│   │   └── MovieGameCard.astro
│   ├── music/          # 音乐页面
│   │   └── MusicCard.astro
│   └── AdvancedSearch.svelte
└── widget/             # 侧边栏/首页小部件
    ├── Advertisement.astro
    ├── AiSummary.astro
    ├── Announcement.astro
    ├── AnnouncementMarquee.astro
    ├── ArchiveHeatmap.astro
    ├── Calendar.astro
    ├── Categories.astro
    ├── DailyQuote.astro
    ├── EditPostButton.svelte
    ├── LifeStats.astro
    ├── Live2DWidget.astro
    ├── Music.astro
    ├── PortalStats.astro
    ├── PostDirectory.astro
    ├── PostDirectoryList.astro
    ├── Profile.astro
    ├── QuickNav.astro
    ├── QuoteOfTheDay.astro
    ├── RecentItems.astro
    ├── RelationshipTimer.astro
    ├── SidebarTOC.astro
    ├── SiteHeatmap.astro
    ├── SiteStats.astro
    ├── SiteVisitCounter.astro
    ├── SpineModel.astro
    └── Tags.astro
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
├── spec/               # 特殊页面（关于、友链、留言板、项目等）
│   ├── about.md
│   ├── friends.md
│   ├── guestbook.md
│   └── projects.md
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
├── life/               # 生活记录（笔记本、日程、足迹等）
│   ├── meta/
│   ├── notebooks/
│   ├── places/
│   └── routines/
├── album/              # 相册
│   └── xxx.md
├── daohang/            # 网址导航
│   └── xxx.md
├── friends/            # 友情链接
│   └── xxx.md
├── ziyuan/             # 资源（公告、每日名言等）
│   ├── announcement.md
│   └── quote.md
└── danmu/              # 弹幕数据
    └── .gitkeep
```

内容 Schema 定义在 `content.config.ts` 中，包含完整的字段类型验证。共有 13 个内容集合：posts、spec、moments、bangumi、changelog、life、notebooks、routines、album、daohang、ziyuan、friends、danmu。

#### src/pages/ - 页面路由目录

Astro 基于文件的路由系统，每个 `.astro` 文件对应一个页面路由。

```
src/pages/
├── index.astro             # 首页（[...page].astro）
├── posts/
│   ├── [...slug].astro     # 文章详情页
│   └── [...page].astro     # 文章列表分页
├── categories/
│   └── [category].astro    # 分类详情页
├── categories.astro        # 分类页
├── archive.astro           # 归档页
├── about.astro             # 关于页
├── friends.astro           # 友链页
├── guestbook.astro         # 留言板页
├── projects.astro          # 项目展示页
├── sponsor.astro           # 赞助页
├── search.astro            # 搜索页
├── moments.astro           # 动态/说说页
├── changelog.astro         # 更新日志页
├── bangumi.astro           # 番剧/影视页（旧版入口）
├── bangumi/[...slug].astro # 番剧详情页
├── books/                  # 书架页
│   ├── index.astro
│   └── [...slug].astro
├── movies-games/           # 影视游戏页
│   └── index.astro
├── music/                  # 音乐页
│   └── index.astro
├── album/                  # 相册页
│   ├── index.astro
│   └── [...slug].astro
├── life/                   # 生活相关页面
│   ├── notebooks/
│   │   ├── index.astro
│   │   └── [...slug].astro
│   ├── places.astro
│   ├── routines.astro
│   └── guestbook.astro
├── write/                  # 在线写文章
│   └── index.astro
├── config/                 # 配置页
│   └── index.astro
├── api/                    # API 端点
│   └── calendar.json.ts
├── og/[...slug].png.ts     # OG 图片生成
├── rss.xml.ts              # RSS 订阅
├── rss.astro               # RSS 页面
├── robots.txt.ts           # robots.txt
├── debug-urls.astro        # URL 调试页
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
├── content-utils.ts    # 内容处理工具
├── date-utils.ts       # 日期处理工具
├── draftHelpers.ts     # 草稿辅助工具
├── editMode.ts         # 在线编辑模式工具
├── hatch-effect.ts     # 孵化效果
├── home-data-layer.js  # 首页数据层
├── home-portfolio-shutter.js  # 首页百叶窗效果
├── icon-loader.ts      # 图标加载器
├── image-utils.ts      # 图片处理工具
├── language-utils.ts   # 语言工具
├── layout-utils.ts     # 布局工具
├── logo-loop.js        # Logo 循环动画
├── navigation-utils.ts # 导航工具
├── page-loader-controller.js  # 页面加载控制器
├── responsive-utils.ts # 响应式工具
├── sakura-manager.ts   # 樱花特效管理器
├── setting-utils.ts    # 设置工具
├── tocUtils.ts         # 目录生成工具
└── url-utils.ts        # URL 处理工具
```

#### src/types/ - 类型定义目录

```
src/types/
└── config.ts           # 配置类型定义
```

#### src/constants/ - 常量定义目录

```
src/constants/
├── constants.ts        # 通用常量
├── icon.ts             # 图标常量
├── icons.ts            # 图标集合
└── link-presets.ts     # 链接预设
```

#### 其他重要目录/文件

| 路径 | 说明 |
|------|------|
| `src/layouts/` | 页面布局组件（MainGridLayout.astro 等） |
| `src/assets/` | 静态资源（图片、图标等，会被 Astro 优化） |
| `src/env.d.ts` | Astro 环境类型声明 |
| `src/content.config.ts` | 内容集合 Schema 配置 |

### public/ - 静态资源目录

此目录下的文件会被直接复制到构建输出目录，不经过 Astro 处理。

```
public/
├── assets/
│   ├── images/         # 图片资源（首页图片、加载动画等）
│   │   ├── home/       # 首页相关图片
│   │   ├── sponsor/    # 赞助相关图片
│   │   └── ...
│   └── js/             # 第三方 JS（marked、highlight.js 等）
├── favicon/            # 网站图标（各种尺寸、明暗模式）
├── gallery/            # 相册图片
│   ├── ai-2026/
│   ├── bl-ll-2026/
│   ├── gpt-img2-2026/
│   ├── mc-2026/
│   └── ...
└── pio/                # Live2D/Spine 模型资源
    ├── models/spine/   # Spine 模型
    └── static/         # 静态资源（SDK 等）
```

### scripts/ - 构建脚本目录

```
scripts/
├── new-post/           # 创建新文章脚本
│   └── index.js
├── generate-icons/     # 生成图标脚本
│   └── index.js
├── fetch-music/        # 音乐抓取脚本
│   ├── batch-download.py
│   ├── fetch-lrc.py
│   └── extract-lrc.py
├── fetch-media/        # 媒体资源抓取脚本
│   └── index.py
├── sync/               # 同步脚本（笔记同步等）
│   ├── index.js
│   ├── sync.ps1
│   └── sync.sh
├── cli.js              # 命令行入口
└── README.md           # 脚本说明
```

使用 `pnpm new-post` 命令可以快速创建新文章。

## 配置文件说明

### 根目录配置文件

| 文件 | 说明 |
|------|------|
| `astro.config.mjs` | Astro 核心配置（集成、Markdown、构建选项等） |
| `package.json` | 项目依赖和 npm 脚本 |
| `biome.json` | Biome 代码检查和格式化配置 |
| `postcss.config.mjs` | PostCSS 配置 |
| `pagefind.yml` | Pagefind 全文搜索配置 |
| `.env.example` | 环境变量示例文件 |
| `tsconfig.json` | TypeScript 配置 |

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

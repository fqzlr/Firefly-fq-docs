# 导航栏配置

导航栏配置文件用于设置博客顶部导航栏的链接和搜索功能。

配置文件路径：`navBarConfig.ts`

## 导航栏链接配置

导航栏链接通过 `navBarConfig.links` 数组进行配置，支持普通链接和下拉菜单两种形式。

### 链接预设

主题内置了常用的链接预设，可直接使用：

| 预设 | 说明 |
|------|------|
| `LinkPreset.Home` | 首页 |
| `LinkPreset.NavPosts` | 文章（下拉菜单） |
| `LinkPreset.Archive` | 归档 |
| `LinkPreset.Categories` | 分类 |
| `LinkPreset.PostList` | 文章列表 |
| `LinkPreset.Collections` | 收藏 |
| `LinkPreset.ContactMe` | 联系我（下拉菜单） |
| `LinkPreset.Friends` | 友链 |
| `LinkPreset.Guestbook` | 留言板 |
| `LinkPreset.QQGroup` | QQ群 |
| `LinkPreset.NavMy` | 我的（下拉菜单） |
| `LinkPreset.Fhome` | 首页（个人） |
| `LinkPreset.Fnote` | 笔记 |
| `LinkPreset.Calendar` | 日历 |
| `LinkPreset.Bangumi` | 番剧 |
| `LinkPreset.Books` | 书架 |
| `LinkPreset.MoviesGames` | 影视游戏 |
| `LinkPreset.MusicPage` | 音乐 |
| `LinkPreset.Changelog` | 更新日志 |
| `LinkPreset.Moments` | 动态 |
| `LinkPreset.Routines` | 日常规划 |
| `LinkPreset.Places` | 旅行足迹 |
| `LinkPreset.Notebooks` | 笔记本 |
| `LinkPreset.Gallery` | 相册 |
| `LinkPreset.Sponsor` | 赞助 |
| `LinkPreset.About` | 关于 |

### 导航栏 Logo 配置

Logo 配置在 [siteConfig.ts](./site-config.md) 的 `navbar.logo` 中，支持四种类型：

| 类型 | 说明 | 示例 |
|------|------|------|
| `"icon"` | Astro 图标库图标 | `{ type: "icon", value: "material-symbols:home-pin-outline" }` |
| `"image"` | 本地图片（src 目录，自动优化） | `{ type: "image", value: "assets/images/logo.webp", alt: "Logo" }` |
| `"image"` | 本地图片（public 目录，不优化） | `{ type: "image", value: "/assets/images/logo.webp", alt: "Logo" }` |
| `"url"` | 网络图片 | `{ type: "url", value: "https://example.com/logo.png", alt: "Logo" }` |

### 默认配置示例

```ts
const links: (NavBarLink | LinkPreset)[] = [
  LinkPreset.Home,
  postsNav,
  ...(siteConfig.pages.collections ? [LinkPreset.Collections] : []),
  ...(contactNav ? [contactNav] : []),
  myNav,
];
```

### 自定义下拉菜单

你可以创建自定义的下拉菜单，格式如下：

```ts
const customNav: NavBarLink = {
  name: "菜单名称",
  icon: "material-symbols:menu-rounded", // 可选图标
  url: "/path/", // 可选，点击父菜单时跳转的链接
  children: [
    LinkPreset.Home,
    {
      name: "自定义链接",
      icon: "material-symbols:link-rounded",
      url: "https://example.com",
      external: true, // 是否为外部链接
    },
  ],
};
```

### 自定义单个链接

```ts
const customLink: NavBarLink = {
  name: "GitHub",
  icon: "fa7-brands:github",
  url: "https://github.com/yourname",
  external: true,
};
```

::: tip
- 页面开关（如 `siteConfig.pages.friends`）会自动控制对应链接的显示/隐藏
- 图标可以在 [Icones](https://icones.js.org/) 查找，支持的图标集包括：fa7-brands、fa7-regular、fa7-solid、material-symbols、simple-icons
:::

## 导航搜索配置

搜索功能通过 `navBarSearchConfig` 进行配置。

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `method` | `NavBarSearchMethod` | `NavBarSearchMethod.PageFind` | 搜索方式 |

### 搜索方式

| 方式 | 说明 |
|------|------|
| `NavBarSearchMethod.PageFind` | 使用 PageFind 本地搜索（推荐） |
| `NavBarSearchMethod.None` | 不启用搜索 |

### 示例

```ts
export const navBarSearchConfig: NavBarSearchConfig = {
  method: NavBarSearchMethod.PageFind,
};
```

::: warning
如果需要禁用搜索，将 `method` 设置为 `NavBarSearchMethod.None` 即可。
:::

## 导航栏样式配置

导航栏样式配置在 [siteConfig.ts](./site-config.md) 的 `navbar` 对象中：

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `navbar.title` | `string` | `"Fqzlr的博客"` | 导航栏标题 |
| `navbar.widthFull` | `boolean` | `false` | 是否全宽导航栏 |
| `navbar.menuAlign` | `"left" \| "center"` | `"center"` | 菜单对齐方式 |
| `navbar.followTheme` | `boolean` | `false` | 图标和标题是否跟随主题色 |
| `navbar.stickyNavbar` | `boolean` | `true` | 是否固定在顶部 |

### 示例

```ts
navbar: {
  logo: {
    type: "icon",
    value: "material-symbols:home-pin-outline",
  },
  title: "我的博客",
  widthFull: true,
  menuAlign: "left",
  followTheme: true,
  stickyNavbar: true,
},
```

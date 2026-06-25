# 个人资料配置

个人资料配置文件用于设置侧边栏用户资料卡片的头像、昵称、签名、社交链接等信息。

配置文件路径：[profileConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/profileConfig.ts)

## 基本信息配置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `avatar` | `string` | `"assets/images/shangban.png"` | 头像图片路径（上班时间显示） |
| `avatarOffWork` | `string` | `"assets/images/xiaban.gif"` | 下班时间头像，为空则始终使用 `avatar` |
| `name` | `string` | `"Fqzlr"` | 名字 |
| `displayName` | `string` | `"Fqzlrの博客"` | 首页展示名字，留空则使用 `name` |
| `nameBadge` | `string` | `"B站：番茄煮理人"` | 名字右侧徽章文字（如 QQ 号、平台 ID 等） |
| `occupation` | `string` | `"[啥都不会/ 无技术博主]"` | 职业/身份标签 |
| `bio` | `string[]` | `["躬身入局，心为主理，行有尺度，自持本心."]` | 个人签名，支持多条，会循环打字+删除效果 |

### 头像路径格式

头像图片路径支持三种格式：

| 格式 | 说明 | 示例 |
|------|------|------|
| public 目录 | 以 `/` 开头，不优化 | `"/assets/images/avatar.webp"` |
| src 目录 | 不以 `/` 开头，自动优化（推荐） | `"assets/images/avatar.webp"` |
| 远程 URL | 网络图片 | `"https://example.com/avatar.jpg"` |

::: tip
头像会根据 [siteConfig.ts](./site-config.md) 中的 `workHours` 配置自动切换上班/下班状态。
:::

### 个人签名示例

`bio` 支持多条签名，会循环显示打字机效果：

```ts
bio: [
  "欢迎来到我的博客！",
  "分享技术，记录生活。",
  "Stay hungry, stay foolish.",
],
```

## 社交链接配置

社交链接通过 `links` 数组配置，每个链接支持以下属性：

| 属性 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `name` | `string` | 是 | 链接名称 |
| `icon` | `string` | 是 | 图标名称，支持 Iconify 图标 |
| `url` | `string` | 是 | 链接地址 |
| `showName` | `boolean` | 否 | 是否同时显示图标和名称，`false` 时只显示图标 |

::: tip
- 已经预装的图标集：fa7-brands、fa7-regular、fa7-solid、material-symbols、simple-icons
- 访问 [Icones](https://icones.js.org/) 获取图标代码
- 如果需要使用其他图标集，需要安装：`pnpm add @iconify-json/<icon-set-name>`
:::

### 默认配置

```ts
links: [
  {
    name: "qq群",
    icon: "fa7-brands:qq",
    url: "https://qm.qq.com/q/wrmF4FI9pu",
    showName: false,
  },
  {
    name: "B站",
    icon: "fa7-brands:bilibili",
    url: "https://space.bilibili.com/2017273493",
    showName: false,
  },
  {
    name: "GitHub",
    icon: "fa7-brands:github",
    url: "https://github.com/fqzlr",
    showName: false,
  },
  {
    name: "Email",
    icon: "fa7-solid:envelope",
    url: "mailto:fqzlr@outlook.com",
    showName: false,
  },
  {
    name: "RSS",
    icon: "fa7-solid:rss",
    url: "/rss/",
    showName: false,
  },
  {
    name: "my home",
    url: "https://home.fqzlr.com",
    icon: "fa7-solid:house-chimney",
    showName: false,
  },
],
```

### 常用图标示例

| 平台 | 图标 |
|------|------|
| QQ | `fa7-brands:qq` |
| Bilibili | `fa7-brands:bilibili` |
| GitHub | `fa7-brands:github` |
| 邮箱 | `fa7-solid:envelope` |
| RSS | `fa7-solid:rss` |
| 网站/主页 | `fa7-solid:house-chimney` |
| 微信 | `fa7-brands:weixin` |
| 微博 | `fa7-brands:weibo` |
| 知乎 | `simple-icons:zhihu` |
| Twitter/X | `fa7-brands:x-twitter` |
| Discord | `fa7-brands:discord` |
| Telegram | `fa7-brands:telegram` |

### 完整配置示例

```ts
export const profileConfig: ProfileConfig = {
  avatar: "assets/images/avatar.webp",
  avatarOffWork: "assets/images/avatar-offwork.gif",
  name: "张三",
  displayName: "张三的博客",
  nameBadge: "全栈开发工程师",
  occupation: "[Web开发者 / 开源爱好者]",
  bio: [
    "记录技术，分享生活。",
    "代码改变世界。",
  ],
  links: [
    {
      name: "GitHub",
      icon: "fa7-brands:github",
      url: "https://github.com/yourname",
      showName: false,
    },
    {
      name: "Email",
      icon: "fa7-solid:envelope",
      url: "mailto:your@email.com",
      showName: false,
    },
    {
      name: "RSS",
      icon: "fa7-solid:rss",
      url: "/rss/",
      showName: false,
    },
  ],
};
```

::: warning
如果不需要下班头像切换功能，将 `avatarOffWork` 设置为空字符串即可。
:::

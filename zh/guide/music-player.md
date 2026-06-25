# 音乐播放器使用指南

Firefly 博客内置了功能完善的音乐播放器，支持 Meting API 在线歌单和本地音乐文件两种模式，具备歌词显示、悬浮歌词、播放列表等功能。本文档将详细介绍如何配置和使用音乐播放器。

## 功能介绍

- ✅ 双模式支持：Meting API 在线歌单 / 本地音乐列表
- ✅ 多平台支持：网易云音乐、QQ音乐、酷狗音乐、虾米音乐、百度音乐
- ✅ 歌词显示：内置歌词抽屉 + 悬浮歌词
- ✅ 播放模式：列表循环、单曲循环、随机播放
- ✅ 歌单列表：可展开查看和切换歌曲
- ✅ 音量控制：滑块调节 + 静音切换
- ✅ 进度控制：点击进度条跳转
- ✅ 封面旋转：播放时专辑封面旋转动画
- ✅ 状态持久化：音量设置保存到 localStorage
- ✅ 3D 可视化：配合 Three.js 实现 3D 音乐可视化效果

相关文件：
- 配置文件：[musicConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/musicConfig.ts)
- 音乐管理器：[MusicManager.astro](file:///e:/AItool/zzwork/my-blog/src/components/features/MusicManager.astro)
- 播放器组件：[MusicPlayer.astro](file:///e:/AItool/zzwork/my-blog/src/components/features/MusicPlayer.astro)
- 侧边栏小部件：[Music.astro](file:///e:/AItool/zzwork/my-blog/src/components/widget/Music.astro)

## 基础配置

音乐播放器的配置文件位于 [src/config/musicConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/musicConfig.ts)。

### 显示/隐藏播放器

播放器默认在两个位置显示：

1. **侧边栏小部件**：在 [sidebarConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/sidebarConfig.ts) 中配置
2. **导航栏入口**：在 musicConfig.ts 中通过 `showInNavbar` 控制

```typescript
// src/config/musicConfig.ts
export const musicPlayerConfig: MusicPlayerConfig = {
  // 是否在导航栏显示音乐播放器入口
  showInNavbar: true,
  // ...
};
```

::: tip 完全禁用播放器
如果不需要音乐播放器：
1. 在 `sidebarConfig.ts` 中将音乐组件的 `enable` 设为 `false`
2. 在 `musicConfig.ts` 中将 `showInNavbar` 设为 `false`
:::

### 基础参数配置

```typescript
export const musicPlayerConfig: MusicPlayerConfig = {
  // 使用方式："meting" 使用 Meting API，"local" 使用本地音乐列表
  mode: "meting",

  // 默认音量 (0-1)
  volume: 0.6,

  // 播放模式：'list'=列表循环, 'one'=单曲循环, 'random'=随机播放
  playMode: "list",

  // 是否启用歌词
  showLyrics: true,
  // ...
};
```

## 模式一：使用 Meting API（推荐）

Meting 是一个开源的音乐 API 聚合项目，可以从各大音乐平台获取歌单、歌曲信息和歌词。

### 配置 Meting API

```typescript
// src/config/musicConfig.ts
export const musicPlayerConfig: MusicPlayerConfig = {
  mode: "meting",  // 设置为 meting 模式

  meting: {
    // Meting API 地址
    // 默认使用官方 API，也可以使用自定义 API
    api: "https://api.i-meto.com/meting/api?server=:server&type=:type&id=:id&r=:r",
    
    // 音乐平台：netease=网易云音乐, tencent=QQ音乐, kugou=酷狗音乐, xiami=虾米音乐, baidu=百度音乐
    server: "netease",
    
    // 类型：song=单曲, playlist=歌单, album=专辑, search=搜索, artist=艺术家
    type: "playlist",
    
    // 歌单/专辑/单曲 ID 或搜索关键词
    id: "17955431099",
    
    // 认证 token（可选，部分平台需要）
    auth: "",
    
    // 备用 API 配置（当主 API 失败时自动切换）
    fallbackApis: [
      "https://api.injahow.cn/meting/?server=:server&type=:type&id=:id",
      "https://api.moeyao.cn/meting/?server=:server&type=:type&id=:id",
    ],
  },
  // ...
};
```

### 如何获取歌单 ID

**网易云音乐示例**：
1. 打开网易云音乐网页版或客户端
2. 找到你想要的歌单
3. 歌单链接格式：`https://music.163.com/#/playlist?id=17955431099`
4. `id=` 后面的数字就是歌单 ID

::: warning 注意事项
- Meting API 是第三方服务，稳定性依赖于 API 提供者
- 建议配置多个 `fallbackApis` 作为备用
- 部分歌曲可能因版权问题无法播放
- 本地开发时如果遇到跨域问题，需要确保 API 支持 CORS
:::

### 常用平台参数对照表

| 平台 | server 参数 | 歌单链接示例 |
|------|-------------|-------------|
| 网易云音乐 | `netease` | `https://music.163.com/#/playlist?id=歌单ID` |
| QQ音乐 | `tencent` | `https://y.qq.com/n/ryqq/playlist/歌单ID` |
| 酷狗音乐 | `kugou` | - |
| 百度音乐 | `baidu` | - |

## 模式二：使用本地音乐列表

如果你希望完全掌控音乐文件，可以使用本地音乐模式。

### 1. 添加音乐文件

将音乐文件（.mp3、.m4a 等格式）放置在 `public/assets/music/` 目录下：

```
public/
└── assets/
    └── music/
        ├── 《远航星的告别》翻唱 希望你能轻松快乐地活着.m4a
        ├── 使一颗心免于哀伤-哼唱.mp3
        └── cover/
            └── 109951169585655912.webp  # 封面图片
```

### 2. 歌词文件准备（可选）

歌词支持两种方式：

**方式一：单独的 .lrc 文件**

将歌词文件放在 `public/assets/music/lrc/` 目录下：
```
public/assets/music/lrc/
└── 使一颗心免于哀伤-哼唱.lrc
```

**方式二：直接在配置中填写歌词字符串**

```typescript
lrc: "[00:00.00]歌词第一行\n[00:05.00]歌词第二行",
```

### 3. 配置本地歌单

```typescript
// src/config/musicConfig.ts
export const musicPlayerConfig: MusicPlayerConfig = {
  mode: "local",  // 设置为 local 模式

  local: {
    playlist: [
      {
        name: "歌曲名称",
        artist: "艺术家名称",
        url: "/assets/music/歌曲文件.mp3",
        cover: "/assets/music/cover/封面图片.webp",
        lrc: "/assets/music/lrc/歌词文件.lrc",  // 可选：歌词文件路径
        // 或者直接填写歌词内容：
        // lrc: "[00:00.00]歌词内容...",
      },
      {
        name: "使一颗心免于哀伤",
        artist: "知更鸟 / HOYO-MiX / Chevy",
        url: "/assets/music/使一颗心免于哀伤-哼唱.mp3",
        cover: "/assets/music/cover/109951169585655912.webp",
        lrc: "",
      },
    ],
  },
  // ...
};
```

::: tip 外链音乐
`url`、`cover`、`lrc` 字段也支持完整的 HTTP/HTTPS 外链地址，会自动识别。
:::

## 侧边栏小部件配置

音乐播放器在侧边栏的显示可以通过 [sidebarConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/sidebarConfig.ts) 控制。

```typescript
// 示例配置（请参考实际 sidebarConfig.ts）
export const sidebarConfig = {
  widgets: [
    // ...其他小部件
    {
      name: "music",
      enable: true,  // 设置为 false 可在侧边栏隐藏音乐播放器
    },
    // ...
  ],
};
```

## 播放器功能使用说明

### 界面布局

播放器界面分为以下几个区域：

1. **顶部区域**：专辑封面 + 歌曲信息 + 音量控制
2. **进度条区域**：播放进度显示和控制 + 时间显示
3. **控制按钮区域**：播放模式、上一首、播放/暂停、下一首、歌单
4. **歌词抽屉**：点击歌词按钮展开
5. **播放列表抽屉**：点击列表按钮展开

### 控制按钮说明

| 按钮 | 功能 |
|------|------|
| 🔊 / 🔇 | 音量调节 / 静音切换 |
| 📝 | 展开/收起歌词抽屉 |
| 🎤 | 切换悬浮歌词显示 |
| 🔁 / 🔂 / 🔀 | 循环模式切换（列表循环/单曲循环/随机播放） |
| ⏮️ | 上一首 |
| ▶️ / ⏸️ | 播放 / 暂停 |
| ⏭️ | 下一首 |
| 📋 | 展开/收起播放列表 |

### 悬浮歌词

点击播放器上的"悬浮歌词"按钮（麦克风图标）可以开启/关闭悬浮歌词功能。悬浮歌词会在页面上以浮动层的形式显示当前播放的歌词行。

相关组件：[FloatingLyrics.astro](file:///e:/AItool/zzwork/my-blog/src/components/features/FloatingLyrics.astro)

### 播放列表

点击播放列表按钮可以展开当前播放列表，点击列表中的歌曲可以直接切换播放。长列表支持分批渲染，滚动到底部自动加载更多。

## 3D 音乐可视化

Firefly 博客内置了基于 Three.js 的 3D 音乐可视化效果，可以在专属页面体验沉浸式音乐可视化。

- 可视化组件目录：[music-visualizer/](file:///e:/AItool/zzwork/my-blog/src/components/features/music-visualizer/)
- 音乐页面路由：`/music/`
- 页面组件：[music/index.astro](file:///e:/AItool/zzwork/my-blog/src/pages/music/index.astro)

::: tip 详细文档
3D 音乐可视化功能的详细说明请参考 [music-visualizer.md](./music-visualizer.md) 文档。
:::

## 页面开关配置

音乐页面的显示可以在 [siteConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/siteConfig.ts) 中控制：

```typescript
export const siteConfig: SiteConfig = {
  pages: {
    // 音乐页面开关
    musicPage: true,  // 设置为 false 可隐藏音乐页面
    // ...
  },
  // ...
};
```

## 常见问题

### 1. Meting API 无法加载歌单

**可能原因**：
- API 地址失效
- 歌单 ID 错误
- 跨域问题
- 网络问题

**解决方案**：
1. 检查歌单 ID 是否正确
2. 尝试更换 `api` 地址或添加备用 API
3. 使用本地音乐模式作为替代方案

### 2. 歌词不显示

**检查项**：
- 确保 `showLyrics` 设置为 `true`
- Meting 模式下：API 返回的歌词数据是否正常
- 本地模式下：检查 `lrc` 路径是否正确
- 部分纯音乐可能没有歌词

### 3. 音乐无法播放

**可能原因**：
- 音乐文件路径错误（本地模式）
- 版权限制导致歌曲无法播放（Meting 模式）
- 音频格式不支持（推荐使用 .mp3 格式）
- 浏览器自动播放策略限制

::: warning 浏览器自动播放策略
现代浏览器限制自动播放带声音的媒体。播放器会在用户首次与页面交互后（如点击）才开始播放，这是正常行为。
:::

### 4. 封面图片不显示

**检查项**：
- 本地模式下：检查 `cover` 路径是否正确
- Meting 模式下：封面 URL 由 API 返回，部分歌曲可能没有封面
- 网络图片可能存在防盗链问题

### 5. 悬浮歌词位置不对

悬浮歌词的样式定义在 [floating-lyrics.css](file:///e:/AItool/zzwork/my-blog/src/styles/components/floating-lyrics.css)，可以根据需要自定义样式。

## 相关文件链接

| 文件 | 说明 |
|------|------|
| [musicConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/musicConfig.ts) | 音乐播放器配置文件 |
| [MusicManager.astro](file:///e:/AItool/zzwork/my-blog/src/components/features/MusicManager.astro) | 音乐状态管理器（核心逻辑） |
| [MusicPlayer.astro](file:///e:/AItool/zzwork/my-blog/src/components/features/MusicPlayer.astro) | 播放器 UI 组件 |
| [Music.astro](file:///e:/AItool/zzwork/my-blog/src/components/widget/Music.astro) | 侧边栏音乐小部件 |
| [FloatingLyrics.astro](file:///e:/AItool/zzwork/my-blog/src/components/features/FloatingLyrics.astro) | 悬浮歌词组件 |
| [music-player.css](file:///e:/AItool/zzwork/my-blog/src/styles/components/music-player.css) | 播放器样式 |
| [music-visualizer/](file:///e:/AItool/zzwork/my-blog/src/components/features/music-visualizer/) | 3D 可视化组件目录 |

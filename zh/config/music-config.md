# 音乐播放器配置详解

音乐播放器配置文件用于设置博客音乐播放器的播放模式、音乐来源、音量、歌词等功能。

配置文件路径：`musicConfig.ts`

组件文档：[音乐播放器组件](../components/music-player.md)

## 基础配置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `showInNavbar` | `boolean` | `true` | 是否在导航栏显示音乐播放器入口 |
| `mode` | `"meting" \| "local"` | `"meting"` | 音乐来源模式：`meting` 使用 Meting API，`local` 使用本地音乐列表 |
| `volume` | `number` | `0.6` | 默认音量，范围 0-1 |
| `playMode` | `"list" \| "one" \| "random"` | `"list"` | 播放模式：`list` 列表循环，`one` 单曲循环，`random` 随机播放 |
| `showLyrics` | `boolean` | `true` | 是否启用歌词显示 |

::: tip
禁用音乐播放器的方法：
1. 侧边栏：在 [sidebarConfig.ts](./sidebar-config.md) 中将音乐组件 `enable` 设为 `false`
2. 导航栏：将本配置文件的 `showInNavbar` 设为 `false`
:::

### 示例

```ts
export const musicPlayerConfig: MusicPlayerConfig = {
  showInNavbar: true,
  mode: "meting",
  volume: 0.7,
  playMode: "list",
  showLyrics: true,
  // ...
};
```

## Meting API 配置

使用 Meting 模式时（`mode: "meting"`），通过 `meting` 对象配置，可以对接网易云音乐、QQ音乐等平台。

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `meting.api` | `string` | `"https://api.i-meto.com/meting/api?server=:server&type=:type&id=:id&r=:r"` | Meting API 地址，支持自定义 API |
| `meting.server` | `"netease" \| "tencent" \| "kugou" \| "xiami" \| "baidu"` | `"netease"` | 音乐平台 |
| `meting.type` | `"song" \| "playlist" \| "album" \| "search" \| "artist"` | `"playlist"` | 类型：单曲、歌单、专辑、搜索、艺术家 |
| `meting.id` | `string` | `"17955431099"` | 歌单/专辑/单曲 ID 或搜索关键词 |
| `meting.auth` | `string` | `""` | 认证 token（可选） |
| `meting.fallbackApis` | `string[]` | 见源文件 | 备用 API 地址数组，主 API 失败时依次尝试 |

### 音乐平台说明

| 平台 | server 值 | 说明 |
|------|-----------|------|
| 网易云音乐 | `netease` | 推荐，资源丰富 |
| QQ音乐 | `tencent` | - |
| 酷狗音乐 | `kugou` | - |
| 虾米音乐 | `xiami` | 已停运 |
| 百度音乐 | `baidu` | - |

### 类型说明

| 类型 | type 值 | 说明 | id 示例 |
|------|---------|------|---------|
| 单曲 | `song` | 单个歌曲 | `"123456"` |
| 歌单 | `playlist` | 歌单列表 | `"17955431099"` |
| 专辑 | `album` | 专辑 | `"12345"` |
| 搜索 | `search` | 关键词搜索 | `"周杰伦"` |
| 艺术家 | `artist` | 歌手 | `"6452"` |

### 备用 API

默认配置了两个备用 API：

```ts
fallbackApis: [
  "https://api.injahow.cn/meting/?server=:server&type=:type&id=:id",
  "https://api.moeyao.cn/meting/?server=:server&type=:type&id=:id",
],
```

### Meting 模式示例

使用网易云音乐歌单：

```ts
mode: "meting",
meting: {
  api: "https://api.i-meto.com/meting/api?server=:server&type=:type&id=:id&r=:r",
  server: "netease",
  type: "playlist",
  id: "你的歌单ID",
  auth: "",
  fallbackApis: [
    "https://api.injahow.cn/meting/?server=:server&type=:type&id=:id",
  ],
},
```

使用 QQ音乐单曲：

```ts
mode: "meting",
meting: {
  api: "https://api.i-meto.com/meting/api?server=:server&type=:type&id=:id&r=:r",
  server: "tencent",
  type: "song",
  id: "歌曲ID",
  auth: "",
  fallbackApis: [],
},
```

::: warning
注意：Meting API 为第三方服务，稳定性无法保证。建议配置多个备用 API。
:::

## 本地音乐配置

使用本地音乐模式时（`mode: "local"`），通过 `local.playlist` 数组配置本地音乐列表。

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `local.playlist` | `Array` | 见源文件 | 本地音乐列表数组 |

每首歌曲支持以下属性：

| 属性 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `name` | `string` | 是 | 歌曲名称 |
| `artist` | `string` | 是 | 艺术家/歌手 |
| `url` | `string` | 是 | 音乐文件路径（支持本地路径或远程 URL） |
| `cover` | `string` | 否 | 封面图片路径 |
| `lrc` | `string` | 否 | 歌词，支持歌词文件路径或 LRC 格式字符串 |

### 歌词配置方式

歌词支持两种配置方式：

1. **歌词文件路径**：
```ts
lrc: "/assets/music/lrc/song-name.lrc",
```

2. **直接填入 LRC 字符串**：
```ts
lrc: "[00:00.00]歌词第一行\n[00:05.00]歌词第二行",
```

### 默认配置

```ts
local: {
  playlist: [
    {
      name: "使一颗心免于哀伤",
      artist: "知更鸟 / HOYO-MiX / Chevy",
      url: "/assets/music/使一颗心免于哀伤-哼唱.mp3",
      cover: "/assets/music/cover/109951169585655912.webp",
      lrc: "",
    },
  ],
},
```

### 本地音乐示例

```ts
mode: "local",
local: {
  playlist: [
    {
      name: "歌曲1",
      artist: "歌手1",
      url: "/assets/music/song1.mp3",
      cover: "/assets/music/cover1.webp",
      lrc: "/assets/music/lrc/song1.lrc",
    },
    {
      name: "歌曲2",
      artist: "歌手2",
      url: "/assets/music/song2.mp3",
      cover: "/assets/music/cover2.webp",
      lrc: "[00:00.00]歌词内容...",
    },
  ],
},
```

::: tip
- 本地音乐文件建议放置在 `public/assets/music/` 目录下
- 封面图片建议放置在 `public/assets/music/cover/` 目录下
- 歌词文件建议放置在 `public/assets/music/lrc/` 目录下
- 本地路径以 `/` 开头表示 public 目录
:::

::: warning
使用本地音乐模式时，音乐文件不会被构建优化，请确保文件体积合适以免影响加载速度。
:::

## 播放模式说明

| 模式 | playMode 值 | 说明 |
|------|-------------|------|
| 列表循环 | `"list"` | 按顺序播放列表，播完最后一首回到第一首 |
| 单曲循环 | `"one"` | 重复播放当前歌曲 |
| 随机播放 | `"random"` | 随机播放列表中的歌曲 |

## 完整配置示例

### Meting 模式（推荐）

```ts
export const musicPlayerConfig: MusicPlayerConfig = {
  showInNavbar: true,
  mode: "meting",
  volume: 0.6,
  playMode: "list",
  showLyrics: true,
  meting: {
    api: "https://api.i-meto.com/meting/api?server=:server&type=:type&id=:id&r=:r",
    server: "netease",
    type: "playlist",
    id: "17955431099",
    auth: "",
    fallbackApis: [
      "https://api.injahow.cn/meting/?server=:server&type=:type&id=:id",
      "https://api.moeyao.cn/meting/?server=:server&type=:type&id=:id",
    ],
  },
  local: {
    playlist: [],
  },
};
```

### 本地音乐模式

```ts
export const musicPlayerConfig: MusicPlayerConfig = {
  showInNavbar: true,
  mode: "local",
  volume: 0.5,
  playMode: "random",
  showLyrics: true,
  meting: {
    api: "",
    server: "netease",
    type: "playlist",
    id: "",
    auth: "",
    fallbackApis: [],
  },
  local: {
    playlist: [
      {
        name: "我的歌曲",
        artist: "歌手名",
        url: "/assets/music/my-song.mp3",
        cover: "/assets/music/my-cover.webp",
        lrc: "/assets/music/lrc/my-song.lrc",
      },
    ],
  },
};
```

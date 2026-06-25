# 音乐播放器组件

音乐播放器组件提供了完整的音乐播放功能，支持 Meting API 和本地音乐两种模式，包含歌词显示、播放列表、播放控制等功能。

组件文件：
- [MusicManager.astro](file:///e:/AItool/zzwork/my-blog/src/components/features/MusicManager.astro) - 音乐管理器（核心逻辑）
- [MusicPlayer.astro](file:///e:/AItool/zzwork/my-blog/src/components/features/MusicPlayer.astro) - 播放器 UI 组件

配置文档：[音乐播放器配置详解](../config/music-config.md)

## 组件概述

音乐播放器采用全局单例设计，`MusicManager` 负责音频播放、状态管理、数据获取等核心逻辑，`MusicPlayer` 负责 UI 渲染和用户交互。

### 特性

- 支持 Meting API（网易云音乐、QQ音乐等平台）
- 支持本地音乐播放
- 歌词同步显示（支持 LRC 格式）
- 三种播放模式：列表循环、单曲循环、随机播放
- 音量控制和静音
- 播放列表抽屉
- 浮动歌词功能
- 封面旋转动画
- 进度条拖拽
- 跨页面状态保持（Swup/SPA 导航）
- 本地存储音量设置

## 使用方式

### 在侧边栏显示

在 [sidebarConfig.ts](../config/sidebar-config.md) 中启用音乐组件：

```ts
leftComponents: [
  // ...其他组件
  {
    type: "music",
    enable: true,
    position: "sticky",
    showOnPostPage: true,
  },
],
```

### 在导航栏显示

在 [musicConfig.ts](../config/music-config.md) 中启用：

```ts
showInNavbar: true,
```

### 组件 Props

`MusicPlayer.astro` 组件支持以下 Props：

| 属性 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `class` | `string` | - | 自定义 CSS 类名 |
| `style` | `string` | - | 自定义内联样式 |
| `id` | `string` | 自动生成 | 组件 ID |
| `lazy` | `boolean` | `false` | 延迟初始化，不自动触发 `mgr.init()`，由外部触发 |

### 基础用法

```astro
---
import MusicPlayer from "@components/features/MusicPlayer.astro";
---

<MusicPlayer />
```

### 延迟初始化

```astro
---
import MusicPlayer from "@components/features/MusicPlayer.astro";
---

<MusicPlayer lazy={true} />
<button id="start-music">点击播放音乐</button>

<script>
  document.getElementById("start-music")?.addEventListener("click", () => {
    window.__fireflyMusic?.init();
  });
</script>
```

## MusicManager API

`window.__fireflyMusic` 提供了全局 API，可以通过 JavaScript 控制播放器。

### 方法

| 方法 | 说明 | 参数 |
|------|------|------|
| `init()` | 初始化播放器，加载音乐列表 | - |
| `getState()` | 获取当前播放状态 | - |
| `togglePlay()` | 切换播放/暂停 | - |
| `playNext()` | 播放下一首 | - |
| `playPrev()` | 播放上一首 | - |
| `cyclePlayMode()` | 循环切换播放模式 | - |
| `setVolume(val)` | 设置音量 | `val`: 0-1 |
| `toggleMute()` | 切换静音 | - |
| `seek(percent)` | 跳转到指定百分比位置 | `percent`: 0-1 |
| `seekToTime(time)` | 跳转到指定时间（秒） | `time`: 秒数 |
| `playTrackByIndex(index)` | 播放指定索引的歌曲 | `index`: 歌曲索引 |
| `loadTrack(index, autoPlay)` | 加载指定索引的歌曲 | `index`: 索引，`autoPlay`: 是否自动播放 |
| `resync()` | 重新同步所有播放器 UI（页面切换后调用） | - |

### 状态对象

`getState()` 返回以下状态：

| 属性 | 类型 | 说明 |
|------|------|------|
| `playlist` | `Array` | 播放列表 |
| `currentIndex` | `number` | 当前播放歌曲索引 |
| `track` | `object` | 当前歌曲信息 |
| `isPlaying` | `boolean` | 是否正在播放 |
| `playMode` | `number` | 播放模式：0=列表循环，1=单曲循环，2=随机 |
| `volume` | `number` | 当前音量（0-1） |
| `isMuted` | `boolean` | 是否静音 |
| `currentTime` | `number` | 当前播放时间（秒） |
| `duration` | `number` | 歌曲总时长（秒） |
| `progress` | `number` | 播放进度百分比（0-100） |
| `currentTimeStr` | `string` | 格式化的当前时间（如 "3:45"） |
| `durationStr` | `string` | 格式化的总时长 |
| `lyrics` | `Array` | 解析后的歌词数组 |
| `currentLrcIndex` | `number` | 当前歌词索引 |
| `initialized` | `boolean` | 是否已初始化 |
| `error` | `string \| null` | 错误信息 |

### 使用示例

```javascript
// 播放/暂停
window.__fireflyMusic.togglePlay();

// 下一首
window.__fireflyMusic.playNext();

// 设置音量为 50%
window.__fireflyMusic.setVolume(0.5);

// 跳转到 50% 位置
window.__fireflyMusic.seek(0.5);

// 获取当前状态
const state = window.__fireflyMusic.getState();
console.log("当前播放：", state.track?.name);
```

## 事件系统

播放器通过 `CustomEvent` 在 `window` 上派发事件，可用于监听播放器状态变化。

| 事件名 | 说明 | detail 属性 |
|--------|------|-------------|
| `fm:init` | 播放器初始化完成 | `playlist`, `playMode`, `volume`, `isMuted` |
| `fm:track` | 歌曲切换 | `index`, `track`, `autoPlay` |
| `fm:play-state` | 播放状态变化 | `isPlaying` |
| `fm:time` | 播放进度更新 | `currentTime`, `duration`, `progress`, `currentTimeStr`, `durationStr` |
| `fm:volume` | 音量变化 | `volume`, `isMuted` |
| `fm:mode` | 播放模式变化 | `playMode` |
| `fm:lyrics` | 歌词加载状态 | `lyrics`, `status`（`loading`/`loaded`/`failed`/`none`） |
| `fm:lrc-index` | 当前歌词索引变化 | `index` |
| `fm:error` | 发生错误 | `message` |
| `fm:toggle-floating-lyrics` | 切换浮动歌词显示 | - |

### 事件监听示例

```javascript
// 监听歌曲切换
window.addEventListener("fm:track", (e) => {
  console.log("正在播放：", e.detail.track.name);
});

// 监听播放状态
window.addEventListener("fm:play-state", (e) => {
  console.log(e.detail.isPlaying ? "开始播放" : "已暂停");
});

// 监听错误
window.addEventListener("fm:error", (e) => {
  console.error("播放器错误：", e.detail.message);
});
```

## UI 功能

### 控制面板

播放器 UI 包含以下控件：

1. **封面**：圆形旋转封面，播放时旋转，暂停时停止
2. **歌曲信息**：歌名、艺术家
3. **音量控制**：音量按钮和滑动条
4. **歌词按钮**：展开/收起歌词抽屉
5. **浮动歌词按钮**：切换浮动歌词显示
6. **进度条**：显示播放进度，支持点击跳转
7. **时间显示**：当前时间/总时长
8. **播放模式**：切换列表循环/单曲循环/随机播放
9. **上一首/下一首**：切换歌曲
10. **播放/暂停**：控制播放
11. **播放列表按钮**：展开/收起播放列表抽屉

### 歌词功能

- 支持 LRC 格式歌词
- 自动高亮当前歌词行
- 点击歌词可跳转到对应位置
- 用户滚动时暂停自动滚动，3秒后恢复
- 支持歌词文件路径和内联歌词字符串

### 播放列表

- 显示所有歌曲，当前播放歌曲高亮
- 点击歌曲直接播放
- 支持分批渲染（歌单较大时性能优化）
- 滚动到底部自动加载更多

## 键盘快捷键

（注：当前版本未内置键盘快捷键，可通过事件 API 自行实现）

```javascript
document.addEventListener("keydown", (e) => {
  if (e.target.tagName === "INPUT" || e.target.tagName === "TEXTAREA") return;
  
  switch (e.code) {
    case "Space":
      e.preventDefault();
      window.__fireflyMusic?.togglePlay();
      break;
    case "ArrowRight":
      e.preventDefault();
      window.__fireflyMusic?.playNext();
      break;
    case "ArrowLeft":
      e.preventDefault();
      window.__fireflyMusic?.playPrev();
      break;
  }
});
```

## 样式自定义

播放器样式位于 `src/styles/components/music-player.css`，可通过 CSS 变量自定义主题色：

```css
.music-player-widget {
  --primary: var(--theme-primary, #10b981);
}
```

::: tip
组件会自动适应暗色/亮色模式，跟随网站主题切换。
:::

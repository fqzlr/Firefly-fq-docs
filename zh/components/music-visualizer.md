# 3D音乐可视化组件

3D 音乐可视化组件基于 Three.js 实现，提供沉浸式的音乐可视化效果，支持音频频谱分析、3D 场景渲染、歌词叠加显示等功能。

组件文件：
- `MusicVisualizer.svelte` - 主组件
- `VisualizerControls.svelte` - 控制面板
- 相关组件：ThreeScene.svelte、LyricsOverlay.svelte、AudioAnalyzer.ts

配置文档：[音乐播放器配置详解](../config/music-config.md)

## 组件概述

3D 音乐可视化组件是一个全屏沉浸式的音乐可视化页面，使用 Web Audio API 分析音频频谱，通过 Three.js 渲染 3D 视觉效果，并与音乐播放器无缝集成。

### 特性

- 基于 Web Audio API 的实时音频频谱分析
- Three.js 3D 场景渲染
- 响应明暗主题
- 完整的播放控制面板
- 播放列表切换
- 歌词叠加显示
- 音量控制和进度条
- 自动连接全局音乐管理器

## 使用方式

该组件用于音乐可视化页面（`/music` 路由），无需手动引入，页面会自动加载。

如果你需要在其他地方使用：

```astro
---
// 注意：这是 Svelte 组件，需要在 Astro 中通过 client 指令加载
---

<!-- 在需要的页面引入 -->
<div id="visualizer-container"></div>
```

::: tip
组件会自动连接到全局音乐管理器 `window.__fireflyMusic`，无需额外配置。
:::

## 组件结构

### MusicVisualizer.svelte

主组件，负责：
- 初始化音频分析器
- 连接全局音乐播放器
- 监听主题变化
- 管理 3D 场景和控制面板的渲染

### VisualizerControls.svelte

控制面板组件，提供：
- 当前歌曲信息显示（封面、歌名、艺术家）
- 进度条和时间显示
- 播放控制按钮（上一首、播放/暂停、下一首）
- 播放模式切换
- 音量控制和静音
- 播放列表面板

### ThreeScene.svelte

Three.js 3D 场景组件，负责：
- 初始化 Three.js 渲染器、场景、相机
- 渲染音频可视化效果
- 响应音频频谱数据

### LyricsOverlay.svelte

歌词叠加层组件，负责：
- 显示同步歌词
- 跟随播放进度高亮

### AudioAnalyzer.ts

音频分析器类，负责：
- 连接 HTMLAudioElement
- 创建 AudioContext 和 AnalyserNode
- 提供频谱数据获取接口

## 控制面板功能

### 歌曲信息区

| 元素 | 说明 |
|------|------|
| 封面 | 当前歌曲封面图片，无封面时显示音乐图标 |
| 歌名 | 当前播放歌曲名称 |
| 艺术家 | 歌手/艺术家名称 |

### 进度控制

| 元素 | 说明 |
|------|------|
| 当前时间 | 已播放时间（如 "1:23"） |
| 进度条 | 可点击跳转到指定位置 |
| 总时长 | 歌曲总时长 |

### 播放控制

| 按钮 | 功能 |
|------|------|
| 歌单按钮 | 打开/关闭播放列表面板 |
| 模式按钮 | 循环切换播放模式（列表→单曲→随机） |
| 上一首 | 播放上一首歌曲 |
| 播放/暂停 | 切换播放状态 |
| 下一首 | 播放下一首歌曲 |
| 静音按钮 | 切换静音 |
| 音量条 | 点击调整音量 |

### 播放列表面板

- 显示当前歌单所有歌曲
- 当前播放歌曲高亮并显示均衡器动画
- 点击歌曲直接切换播放
- 自动滚动到当前播放歌曲
- 显示歌曲总数

## 主题适配

组件会自动监听页面主题变化：

```javascript
function syncPageTheme() {
  useLightBackground = !document.documentElement.classList.contains("dark");
}

const themeObserver = new MutationObserver(syncPageTheme);
themeObserver.observe(document.documentElement, {
  attributes: true,
  attributeFilter: ["class"],
});
```

- 暗色模式：深色背景
- 亮色模式：浅色背景

## 音频上下文

由于浏览器自动播放策略限制，音频上下文需要用户交互后才能恢复：

```javascript
const handleFirstClick = () => {
  audioAnalyzer.resume();
  document.removeEventListener("click", handleFirstClick);
};
document.addEventListener("click", handleFirstClick);
```

::: tip
用户首次点击页面后，音频分析器会自动恢复，可视化效果开始工作。
:::

## 与音乐播放器集成

组件通过 `window.__fireflyMusic` 全局 API 与音乐播放器通信：

### 监听的事件

| 事件 | 响应 |
|------|------|
| `fm:init` | 初始化完成，同步状态 |
| `fm:track` | 歌曲切换，更新显示 |
| `fm:play-state` | 播放状态变化，更新按钮 |
| `fm:time` | 进度更新，更新进度条 |
| `fm:volume` | 音量变化，更新音量条 |
| `fm:mode` | 播放模式变化，更新图标 |

### 调用的方法

| 方法 | 用途 |
|------|------|
| `mgr.init()` | 初始化音乐管理器 |
| `mgr.togglePlay()` | 切换播放/暂停 |
| `mgr.playNext()` | 下一首 |
| `mgr.playPrev()` | 上一首 |
| `mgr.cyclePlayMode()` | 切换播放模式 |
| `mgr.toggleMute()` | 切换静音 |
| `mgr.setVolume(val)` | 设置音量 |
| `mgr.seek(percent)` | 跳转进度 |
| `mgr.playTrackByIndex(index)` | 播放指定歌曲 |
| `mgr.getState()` | 获取当前状态 |

## 样式

组件样式位于 `src/styles/pages/music-visualizer.css`，采用全屏沉浸设计：

- 控制面板固定在屏幕顶部
- 播放列表从侧边滑出
- 深色/亮色主题自动适配
- 响应式设计，支持移动端

::: tip
可视化效果会填满整个视口，建议作为独立全屏页面使用。
:::

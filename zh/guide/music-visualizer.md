# 3D 音乐可视化功能说明

Firefly 博客内置了基于 Three.js 的 3D 音乐可视化功能，可以将音乐转化为沉浸式的视觉效果。通过 Web Audio API 实时分析音频频谱，驱动 3D 场景中的粒子、波纹、光影等视觉元素随音乐律动。

## 功能介绍

- 🎵 **实时音频分析**：使用 Web Audio API 分析音频频谱数据
- 🌊 **波形可视化**：3D 地形/波浪效果随音乐起伏
- ✨ **粒子特效**：音乐节拍触发粒子爆发和光效
- 💫 **波纹涟漪**：鼓点/重低音触发地面涟漪效果
- 🌠 **流星特效**：中高频触发流星划过效果
- 🎤 **歌词叠加**：支持在 3D 场景上显示歌词
- 🎛️ **完整播放控制**：播放/暂停、切歌、音量、进度、播放模式
- 📋 **歌单面板**：侧边歌单快速切换歌曲
- 🌓 **明暗主题适配**：自动跟随网站主题切换
- 🖱️ **交互支持**：点击可触发手动涟漪效果

## 页面访问

音乐可视化页面的访问路径为 `/music/`，前提是在 `siteConfig.ts` 中开启了音乐页面：

```typescript
export const siteConfig: SiteConfig = {
  pages: {
    musicPage: true,  // 设置为 true 启用音乐页面
  },
  // ...
};
```

页面入口组件：`music/index.astro`

::: warning 性能提示
3D 可视化基于 WebGL 渲染，对设备性能有一定要求。如果设备性能较低，可能出现帧率下降、发热等情况。建议使用支持 WebGL 2.0 的现代浏览器访问。
:::

## 组件架构

音乐可视化功能位于 `src/components/features/music-visualizer/` 目录下：

```
music-visualizer/
├── MusicVisualizer.svelte   # 主组件入口
├── AudioAnalyzer.ts         # 音频分析引擎
├── ThreeScene.svelte        # Three.js 3D 场景
├── LyricsOverlay.svelte     # 歌词叠加层
└── VisualizerControls.svelte # 播放控制面板
```

各组件职责：

| 文件 | 职责 |
|------|------|
| `MusicVisualizer.svelte` | 主容器组件，负责连接音频分析器和 3D 场景，管理主题同步 |
| `AudioAnalyzer.ts` | Web Audio API 封装，实时音频频谱分析、节拍检测 |
| `ThreeScene.svelte` | Three.js 3D 场景渲染，地形波浪、粒子、光照等视觉效果 |
| `LyricsOverlay.svelte` | 歌词叠加显示组件 |
| `VisualizerControls.svelte` | 音乐播放控制面板 |
| `music-visualizer.css` | 页面样式文件 |

## 音频分析引擎

`AudioAnalyzer.ts` 是可视化的核心，负责从音频元素中提取频谱数据。

### 频谱频段划分

音频被分析并划分为多个频段，分别驱动不同的视觉效果：

| 频段 | 频率范围 | 驱动效果 |
|------|---------|---------|
| subBass (超低音) | 20-60 Hz | 大波纹、整体震动 |
| bass (低音) | 60-250 Hz | 地形起伏、节拍检测 |
| lowMid (中低音) | 250-500 Hz | 波纹扩散 |
| mid (中音) | 500-2000 Hz | 粒子运动 |
| highMid (中高音) | 2-6 kHz | 粒子亮度 |
| presence (临场感) | 6-12 kHz | 光芒强度 |
| brilliance ( Brilliance ) | 12-20 kHz | 高频闪烁 |
| air (空气感) | 20+ kHz | 环境光效果 |

### 音频特征数据

分析器输出以下音频特征供 3D 场景使用：

```typescript
interface AudioData {
  bass: number;           // 低音强度
  mid: number;            // 中音强度
  treble: number;         // 高音强度
  energy: number;         // 整体能量
  subBass: number;        // 超低音
  lowMid: number;         // 中低音
  highMid: number;        // 中高音
  presence: number;       // 临场感
  brilliance: number;     // 明亮度
  air: number;            // 空气感
  warmth: number;         // 温暖度（低频占比）
  brightness: number;     // 明亮度（高频占比）
  sharpness: number;      // 锐度
  smoothness: number;     // 平滑度
  density: number;        // 频段密度
  spectralCentroid: number; // 频谱质心
}
```

### 节拍检测

音频分析器内置了节拍检测算法，基于历史能量波动和标准差来识别节拍：

```typescript
// 节拍检测核心逻辑
private detectBeats(fluxPulse: number, fluxMeteor: number) {
  // 维护历史能量值
  this.beatHistory[this.beatHistoryIndex] = smoothedFlux;
  // 计算平均值和标准差
  const threshold = Math.max(0.05, avgFlux + fluxStdDev * 1.5);
  
  if (smoothedFlux > threshold) {
    // 触发节拍事件
    this.events.onRipple?.(rx, rz, strength, false);
    this.events.onBeat?.(strength);
  }
  
  // 流星特效触发
  if (fluxMeteor > 0.08) {
    this.events.onMeteor?.(strength);
  }
}
```

检测到节拍后会触发以下事件回调：
- `onRipple(x, z, strength, isWhite)`: 触发涟漪效果
- `onBeat(strength)`: 节拍事件
- `onMeteor(strength)`: 流星事件

## 使用方式

### 基本使用

1. 确保音乐播放器已正确配置（参考 [music-player.md](./music-player.md)）
2. 访问 `/music/` 页面
3. 点击播放按钮开始音乐，3D 场景会自动加载并与音乐同步
4. 首次点击页面后音频上下文才会激活（浏览器自动播放策略）

### 播放控制

控制面板提供完整的音乐播放功能：

| 控件 | 功能 |
|------|------|
| ▶️ / ⏸️ | 播放/暂停 |
| ⏮️ / ⏭️ | 上一首/下一首 |
| 🔁 / 🔂 / 🔀 | 播放模式切换（列表循环/单曲循环/随机） |
| 🔊 | 静音切换 |
| 进度条 | 点击跳转到指定位置 |
| 音量条 | 点击调节音量 |
| 📋 | 打开/关闭歌单面板 |

### 歌单面板

点击歌单按钮可打开侧边歌单面板：
- 显示当前播放列表的所有歌曲
- 当前播放歌曲高亮显示并有 EQ 动画指示
- 点击歌曲可直接切换播放
- 自动滚动到当前播放位置

### 歌词显示

如果当前歌曲有歌词，会在 3D 场景上方显示歌词：
- 歌词会随播放进度自动高亮当前行
- 支持 LRC 格式的时间轴歌词

### 交互操作

- **点击 3D 场景**：在点击位置触发手动涟漪效果
- **鼠标移动**：可影响相机/视角（取决于具体实现）

## 主题适配

3D 可视化场景会自动跟随网站明暗主题切换：

- 监听 `<html>` 元素的 `class` 属性变化（`dark` 类）
- 亮色主题：明亮的地面、清新的配色
- 暗色主题：深色背景、霓虹光效

主题同步逻辑在 `MusicVisualizer.svelte` 中实现：

```svelte
<script lang="ts">
function syncPageTheme() {
  useLightBackground = !document.documentElement.classList.contains("dark");
}

onMount(() => {
  syncPageTheme();
  const themeObserver = new MutationObserver(syncPageTheme);
  themeObserver.observe(document.documentElement, {
    attributes: true,
    attributeFilter: ["class"],
  });
});
</script>
```

## 与音乐播放器集成

3D 可视化与全站音乐播放器共享同一个音乐管理器 (`window.__fireflyMusic`)，两者状态完全同步：

```typescript
// 音乐管理器全局实例
interface FireflyMusicManager {
  getState: () => MusicState;
  init: () => void;
  togglePlay: () => void;
  playNext: () => void;
  playPrev: () => void;
  cyclePlayMode: () => void;
  toggleMute: () => void;
  setVolume: (value: number) => void;
  seek: (percent: number) => void;
  playTrackByIndex: (index: number) => void;
}
```

事件通信通过 CustomEvent 实现：

| 事件名 | 触发时机 | 数据 |
|--------|---------|------|
| `fm:init` | 音乐管理器初始化完成 | `{ playlist, playMode, volume }` |
| `fm:track` | 切换歌曲 | `{ track, index }` |
| `fm:play-state` | 播放/暂停状态变化 | `{ isPlaying }` |
| `fm:time` | 播放进度更新 | `{ progress, currentTimeStr, durationStr }` |
| `fm:volume` | 音量变化 | `{ volume, isMuted }` |
| `fm:mode` | 播放模式变化 | `{ playMode }` |
| `fm:lyrics` | 歌词加载完成 | `{ lyrics, status }` |
| `fm:lrc-index` | 当前歌词行变化 | `{ index }` |
| `fm:toggle-floating-lyrics` | 切换悬浮歌词 | - |

## 浏览器兼容性

| 浏览器 | 最低版本 | 说明 |
|--------|---------|------|
| Chrome | 90+ | ✅ 完全支持 |
| Firefox | 89+ | ✅ 完全支持 |
| Safari | 15+ | ✅ 支持（部分效果可能有差异） |
| Edge | 90+ | ✅ 完全支持 |

### 必要 API 支持

- **WebGL 2.0**: 3D 渲染必需
- **Web Audio API**: 音频分析必需
- **AudioContext**: 音频处理
- **requestAnimationFrame**: 动画循环
- **ResizeObserver**: 画布自适应

::: warning iOS Safari 注意事项
iOS Safari 对 Web Audio API 有严格限制：
1. 必须在用户交互（如点击）后才能创建/恢复 AudioContext
2. 页面隐藏时音频可能被暂停
3. 这些限制已在代码中做了兼容处理
:::

## 自定义和扩展

### 修改视觉效果参数

视觉效果参数主要在 `ThreeScene.svelte` 中定义，可以调整以下参数：

- 地形网格密度
- 波浪高度/幅度
- 粒子数量
- 颜色配色方案
- 光照强度
- 相机位置/视角

### 添加新的视觉效果

1. 在 `AudioAnalyzer.ts` 中添加新的音频特征检测
2. 在事件回调中暴露新事件
3. 在 `ThreeScene.svelte` 中监听新事件并添加对应的 3D 效果

### 自定义颜色主题

可以修改 Three.js 场景中的材质颜色，使其与网站主题色相匹配。主题色相可从 `siteConfig.themeColor.hue` 获取。

## 性能优化建议

如果遇到性能问题，可以尝试以下优化：

1. **降低粒子数量**：减少 Three.js 场景中的粒子数
2. **降低几何体复杂度**：使用更简单的地形网格
3. **关闭阴影**：阴影计算开销较大
4. **降低像素比**：限制渲染器的 pixel ratio
5. **使用性能更好的设备**：独立显卡设备性能更佳

```typescript
// 示例：限制像素比以提升性能
renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
```

## 常见问题

### 1. 进入页面后没有声音

**原因**：浏览器自动播放策略限制，需要用户交互后才能播放音频

**解决方案**：点击播放按钮或页面任意位置，音频会正常播放

### 2. 3D 场景黑屏/不显示

**可能原因**：
- 浏览器不支持 WebGL 2.0
- 设备 GPU 被禁用或驱动问题
- WebGL 上下文丢失

**解决方案**：
1. 确认浏览器支持 WebGL：访问 `https://get.webgl.org/` 检测
2. 更新显卡驱动
3. 尝试使用 Chrome/Edge 浏览器
4. 关闭硬件加速后重新开启

### 3. 页面卡顿/帧率低

**可能原因**：
- 设备性能不足
- 同时运行多个占用 GPU 的程序
- 浏览器标签页过多

**解决方案**：
1. 关闭其他占用资源的程序和标签页
2. 使用性能更好的设备
3. 可以考虑在 ThreeScene 中降低渲染质量

### 4. 歌词不显示

**检查项**：
- 确认 `musicConfig.ts` 中 `showLyrics` 为 `true`
- 确认当前歌曲有歌词数据
- Meting 模式下部分歌曲可能没有歌词

### 5. 切换页面后音乐停止

这是正常现象，因为 3D 可视化页面的控制面板是独立实例。使用侧边栏小部件或导航栏的播放器可以在页面切换时保持播放。

::: tip 保持播放
全站音乐播放器由 `MusicManager.astro` 管理，会在页面加载时初始化。3D 可视化页面连接到同一个全局管理器实例。
:::

## 相关文件链接

| 文件 | 说明 |
|------|------|
| `MusicVisualizer.svelte` | 可视化主组件 |
| `AudioAnalyzer.ts` | 音频分析引擎 |
| `ThreeScene.svelte` | Three.js 3D 场景 |
| `LyricsOverlay.svelte` | 歌词叠加层 |
| `VisualizerControls.svelte` | 播放控制面板 |
| `music/index.astro` | 音乐页面路由 |
| `music-visualizer.css` | 页面样式 |
| `MusicManager.astro` | 全局音乐管理器 |
| [three](https://threejs.org/) | Three.js 官方文档 |

# Live2D/Spine 看板娘组件

看板娘组件支持 Live2D 和 Spine 两种模型格式，提供可交互的虚拟角色，支持点击触发动作、消息气泡、拖拽移动等功能。

组件文件：
- `Live2DWidget.astro` - Live2D 看板娘组件
- `SpineModel.astro` - Spine 看板娘组件

配置文件：`pioConfig.ts`

## 组件概述

主题同时支持两种看板娘技术：
- **Live2D**：使用 Cubism SDK，支持 Cubism 2/3/4 模型，表现力更丰富
- **Spine**：使用 Spine Web Player，支持 Spine 2D 骨骼动画

两种组件共享相似的配置结构和交互逻辑，可以任选其一启用。

### 特性

- 支持 Live2D（Cubism 2/3/4）和 Spine 两种模型
- 点击触发随机动作和消息
- 可拖拽移动位置
- 工具栏：移动、切换动画组、作者信息、关闭
- 空闲自动待机动画
- 页面切换时状态保持
- 本地存储位置和可见性
- 响应式设计，支持移动端
- 消息气泡提示
- 显示/隐藏切换按钮
- 模型资源懒加载

## 配置文件

看板娘配置位于 `src/config/pioConfig.ts`，包含 `spineModelConfig` 和 `live2dModelConfig` 两个配置对象。

### 通用配置项

Live2D 和 Spine 共享以下配置结构：

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `enable` | `boolean` | Live2D: `true`, Spine: `false` | 是否启用该看板娘 |
| `model.path` | `string` | - | 模型文件路径 |
| `position.corner` | `"bottom-left" \| "bottom-right" \| "top-left" \| "top-right"` | `"bottom-left"` | 显示位置 |
| `position.offsetX` | `number` | `0` | 水平偏移（px） |
| `position.offsetY` | `number` | `0` | 垂直偏移（px） |
| `size.width` | `number` | Live2D: `255`, Spine: `135` | 容器宽度（px） |
| `size.height` | `number` | Live2D: `285`, Spine: `165` | 容器高度（px） |
| `interactive.enabled` | `boolean` | `true` | 是否启用交互功能 |
| `interactive.clickMessages` | `string[]` | 见源文件 | 点击时随机显示的消息 |
| `interactive.messageDisplayTime` | `number` | `3000` | 消息显示时间（毫秒） |
| `responsive.hideOnMobile` | `boolean` | Live2D: `false`, Spine: `true` | 是否在移动端隐藏 |
| `responsive.mobileBreakpoint` | `number` | `768` | 移动端断点（px） |

::: warning
注意：`bottom-right` 位置可能会挡住返回顶部按钮。
:::

### Live2D 特有配置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `defaultVisible` | `boolean` | `false` | 首次访问是否默认显示（false 则点击入口后加载） |
| `resolution` | `number` | `3` | 渲染分辨率倍率，高 DPI 屏幕可设为 2-3，值越大越耗性能 |
| `author.name` | `string` | `"木果阿木果"` | 作者名称 |
| `author.url` | `string` | 见源文件 | 作者链接 |

### Spine 特有配置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `model.scale` | `number` | `1.0` | 模型缩放比例 |
| `model.x` | `number` | `0` | X 轴偏移 |
| `model.y` | `number` | `0` | Y 轴偏移 |
| `interactive.clickAnimations` | `string[]` | 见源文件 | 点击时随机播放的动画列表 |
| `interactive.idleAnimations` | `string[]` | 见源文件 | 待机动画列表 |
| `interactive.idleInterval` | `number` | `8000` | 待机动画切换间隔（毫秒） |
| `zIndex` | `number` | `1000` | CSS 层级 |
| `opacity` | `number` | `1.0` | 透明度 |

## Live2D 配置示例

```ts
export const live2dModelConfig: Live2DModelConfig = {
  enable: true,
  defaultVisible: false,
  model: {
    path: "/pio/models/live2d/model-name/model.model3.json",
  },
  position: {
    corner: "bottom-left",
    offsetX: 20,
    offsetY: 20,
  },
  size: {
    width: 280,
    height: 250,
  },
  resolution: 2,
  interactive: {
    enabled: true,
    clickMessages: [
      "你好呀！",
      "欢迎来到我的博客~",
      "今天也要开心哦！",
    ],
    messageDisplayTime: 3000,
  },
  author: {
    name: "模型作者",
    url: "https://example.com",
  },
  responsive: {
    hideOnMobile: false,
    mobileBreakpoint: 768,
  },
};
```

## Spine 配置示例

```ts
export const spineModelConfig: SpineModelConfig = {
  enable: true,
  model: {
    path: "/pio/models/spine/model-name/model.json",
    scale: 1.0,
    x: 0,
    y: 0,
  },
  position: {
    corner: "bottom-left",
    offsetX: 0,
    offsetY: 0,
  },
  size: {
    width: 150,
    height: 180,
  },
  interactive: {
    enabled: true,
    clickAnimations: ["tap", "happy"],
    clickMessages: [
      "你好！",
      "点击我做什么呀~",
    ],
    messageDisplayTime: 3000,
    idleAnimations: ["idle", "blink"],
    idleInterval: 8000,
  },
  responsive: {
    hideOnMobile: true,
    mobileBreakpoint: 768,
  },
  zIndex: 1000,
  opacity: 1.0,
};
```

## 使用方式

组件通常在布局文件中全局引入，会在所有页面显示。

### 启用 Live2D

在 `pioConfig.ts` 中设置：

```ts
export const live2dModelConfig: Live2DModelConfig = {
  enable: true,
  // ...其他配置
};

export const spineModelConfig: SpineModelConfig = {
  enable: false, // 禁用 Spine
  // ...
};
```

### 启用 Spine

```ts
export const live2dModelConfig: Live2DModelConfig = {
  enable: false, // 禁用 Live2D
  // ...
};

export const spineModelConfig: SpineModelConfig = {
  enable: true,
  // ...其他配置
};
```

::: tip
不建议同时启用 Live2D 和 Spine，两者功能重叠，会增加页面资源加载。
:::

## 工具栏功能

Live2D 模型显示时，左上角有工具栏按钮：

| 按钮 | 图标 | 功能 |
|------|------|------|
| 移动 | 拖拽图标 | 用于拖拽移动看板娘（按住可拖动） |
| 切换动画组 | 切换图标 | 循环切换点击触发的动画组（表情/短动画/长动画/其他） |
| 作者信息 | 信息图标 | 显示/隐藏模型作者信息 |
| 关闭 | 关闭图标 | 关闭看板娘，显示入口按钮 |

### 动画组说明

切换按钮可循环切换以下动画组：

| 组名 | 说明 |
|------|------|
| Expression | 只触发表情变化 |
| TapShort | 触发短点击动画 |
| TapLong | 触发长点击动画 |
| Other | 触发其他动画 |

::: tip
动画组和动作名称从模型的 JSON 文件中自动读取，不同模型可用的动画组可能不同。
:::

## 交互功能

### 点击交互

- 点击模型：播放随机动作，显示随机消息气泡
- 移动端点击非工具栏区域：显示/隐藏工具栏
- 点击页面其他区域（移动端）：隐藏工具栏

### 拖拽

- 按住拖拽按钮或模型主体可拖动看板娘
- 拖拽位置自动限制在视口范围内
- 位置保存在 localStorage，刷新页面后保持位置
- 支持鼠标拖拽和触屏拖拽

### 空闲状态

- 15-30 秒无操作后自动播放待机动画
- 页面切换到后台时暂停渲染节省资源
- 页面重新可见时恢复渲染

## 显示/隐藏

### 关闭看板娘

点击工具栏的关闭按钮：
1. 播放下落退出动画
2. 保存位置和可见状态到 localStorage
3. 暂停 PIXI 渲染
4. 显示入口按钮

### 显示看板娘

点击入口按钮：
1. 加载/恢复 PIXI 渲染
2. 播放进入动画
3. 恢复保存的位置

### 首次访问

`defaultVisible: false` 时，首次访问页面不显示看板娘，只显示入口按钮，用户点击后才加载模型资源，提升首屏加载速度。

## 状态持久化

以下状态保存在 localStorage 中：

| Key | 说明 |
|-----|------|
| `live2d-visibility-cache` | 可见性状态（`visible`/`hidden`） |
| `live2d-position-cache` | 位置（left/top） |

## 资源加载

### Live2D 依赖

组件动态加载以下外部库（带失败重试）：

1. Cubism Core：`https://cubism.live2d.com/sdk-web/cubismcore/live2dcubismcore.min.js`
2. PixiJS：`https://cdn.jsdelivr.net/npm/pixi.js@6.5.10/dist/browser/pixi.min.js`
3. pixi-live2d-display：`https://cdn.jsdelivr.net/npm/pixi-live2d-display/dist/cubism4.min.js`

### Spine 依赖

Spine 组件优先从 CDN 加载，失败后回退到本地文件：

1. CSS：`https://unpkg.com/@esotericsoftware/spine-player@4.2.*/dist/spine-player.min.css` → 回退到 `/pio/static/spine-player.min.css`
2. JS：`https://unpkg.com/@esotericsoftware/spine-player@4.2.*/dist/iife/spine-player.min.js` → 回退到 `/pio/static/spine-player.min.js`

::: tip
本地 Spine 运行时文件需要放置在 `public/pio/static/` 目录下。
:::

## 模型放置

### Live2D 模型

将 Live2D 模型文件夹放置在 `public/pio/models/live2d/` 目录下：

```
public/
  pio/
    models/
      live2d/
        your-model/
          your-model.model3.json
          textures/
          ...
```

配置路径：

```ts
model: {
  path: "/pio/models/live2d/your-model/your-model.model3.json",
}
```

支持的模型格式：
- Cubism 2：`.model.json`
- Cubism 3+：`.model3.json`

### Spine 模型

将 Spine 模型文件放置在 `public/pio/models/spine/` 目录下：

```
public/
  pio/
    models/
      spine/
        your-model/
          your-model.json
          your-model.atlas
          your-model.png
          ...
```

配置路径指向 `.json` 文件，组件会自动加载同名 `.atlas` 文件：

```ts
model: {
  path: "/pio/models/spine/your-model/your-model.json",
}
```

## JavaScript API

### 全局方法

| 方法 | 说明 |
|------|------|
| `window._showLive2DWidget()` | 显示 Live2D 看板娘 |
| `window._closeLive2DWidget()` | 关闭 Live2D 看板娘 |

### 事件

| 事件名 | 说明 | detail |
|--------|------|--------|
| `live2d-visibility-change` | 可见性变化 | `{ visible: boolean }` |

### 使用示例

```javascript
// 显示看板娘
window._showLive2DWidget?.();

// 关闭看板娘
window._closeLive2DWidget?.();

// 监听可见性变化
window.addEventListener("live2d-visibility-change", (e) => {
  console.log("看板娘可见性：", e.detail.visible);
});
```

## 消息气泡

消息气泡功能由公共组件 `PioMessageBox.astro` 提供，支持：

- 自动在模型上方显示
- 显示指定时间后自动消失
- 箭头指向模型
- 支持文本内容

使用全局方法显示消息：

```javascript
window.showModelMessage?.("你好！", {
  containerId: "live2d-widget",
  displayTime: 3000,
});
```

::: tip
点击模型时自动从 `clickMessages` 数组随机选择消息显示。
:::

## 性能建议

1. **首次延迟加载**：建议设置 `defaultVisible: false`，用户点击后再加载模型
2. **移动端隐藏**：性能较差的设备建议设置 `responsive.hideOnMobile: true`
3. **分辨率控制**：`resolution` 建议不超过 2，高分辨率会显著增加 GPU 负担
4. **模型大小**：选择文件体积适中的模型，纹理建议压缩
5. **避免同时启用**：Live2D 和 Spine 二选一即可

::: warning
看板娘会使用 WebGL 渲染，在性能较低的设备上可能影响页面流畅度。
:::

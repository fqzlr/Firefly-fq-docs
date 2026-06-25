# 侧边栏配置

侧边栏配置文件用于设置博客侧边栏的布局、位置和组件显示。

配置文件路径：`sidebarConfig.ts`

## 侧边栏布局配置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `enable` | `boolean` | `true` | 是否启用侧边栏功能 |
| `position` | `"left" \| "right" \| "both"` | `"both"` | 侧边栏位置：`left` 仅左侧，`right` 仅右侧，`both` 双侧栏 |
| `tabletSidebar` | `"left" \| "right"` | `"left"` | 平板端（769-1279px）显示哪侧侧边栏，仅 `position` 为 `both` 时生效 |
| `showBothSidebarsOnPostPage` | `boolean` | `true` | 使用单侧栏时，是否在文章详情页显示双侧边栏 |

### 位置说明

- `position: "left"`：仅显示左侧边栏
- `position: "right"`：仅显示右侧边栏
- `position: "both"`：1280px 以上同时显示左右，769-1279px 根据 `tabletSidebar` 配置显示其中一侧

::: tip
`showBothSidebarsOnPostPage` 适用于只想用单侧栏，但在文章详情页想用对侧栏目录等组件的场景。
:::

### 示例

```ts
export const sidebarLayoutConfig: SidebarLayoutConfig = {
  enable: true,
  position: "both",
  tabletSidebar: "left",
  showBothSidebarsOnPostPage: true,
  // ...
};
```

## 组件通用配置属性

每个侧边栏组件支持以下配置属性：

| 属性 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `type` | `string` | - | 组件类型 |
| `enable` | `boolean` | - | 是否启用该组件 |
| `position` | `"top" \| "sticky"` | - | 组件位置：`top` 固定顶部，`sticky` 粘性定位（跟随滚动） |
| `showOnPostPage` | `boolean` | `true` | 是否在文章详情页显示 |
| `showOnNonPostPage` | `boolean` | `true` | 是否在非文章详情页显示 |
| `configId` | `string` | - | 组件配置 ID（目前仅广告组件使用） |
| `responsive` | `object` | - | 响应式配置（部分组件可用） |

::: tip
组件的渲染顺序完全取决于它们在配置数组中出现的顺序，但 `top` 位置的组件会优先于 `sticky` 位置的组件渲染。
:::

## 左侧边栏组件

左侧边栏组件通过 `leftComponents` 数组配置。

### 可用组件类型

| 类型 | 说明 |
|------|------|
| `"profile"` | 用户资料组件 |
| `"announcement"` | 公告组件 |
| `"music"` | 音乐播放器组件 |
| `"categories"` | 分类组件 |
| `"tags"` | 标签组件（已移至浮动坞弹窗） |
| `"advertisement"` | 广告栏组件 |

### 默认配置

```ts
leftComponents: [
  {
    type: "profile",
    enable: true,
    position: "top",
    showOnPostPage: true,
  },
  {
    type: "announcement",
    enable: true,
    position: "top",
    showOnPostPage: true,
  },
  {
    type: "music",
    enable: true,
    position: "sticky",
    showOnPostPage: true,
  },
  {
    type: "categories",
    enable: true,
    position: "sticky",
    showOnPostPage: true,
    responsive: {
      collapseThreshold: 5,
    },
  },
  {
    type: "tags",
    enable: false,
    position: "sticky",
    showOnPostPage: true,
    responsive: {
      collapseThreshold: 10,
    },
  },
  {
    type: "advertisement",
    enable: false,
    position: "sticky",
    showOnPostPage: true,
    configId: "ad1",
  },
],
```

### 分类/标签组件响应式配置

| 属性 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `responsive.collapseThreshold` | `number` | `5`（分类）/ `10`（标签） | 折叠阈值：当数量超过此值时自动折叠 |

## 右侧边栏组件

右侧边栏组件通过 `rightComponents` 数组配置。

### 可用组件类型

| 类型 | 说明 |
|------|------|
| `"stats"` | 站点统计组件 |
| `"calendar"` | 日历组件 |
| `"sidebarToc"` | 侧边栏目录组件（只在文章详情页显示） |
| `"advertisement"` | 广告栏组件 |

### 默认配置

```ts
rightComponents: [
  {
    type: "stats",
    enable: true,
    position: "top",
    showOnPostPage: true,
  },
  {
    type: "calendar",
    enable: true,
    position: "sticky",
    showOnPostPage: false,
  },
  {
    type: "sidebarToc",
    enable: true,
    position: "sticky",
    showOnPostPage: true,
    showOnNonPostPage: false,
  },
  {
    type: "advertisement",
    enable: false,
    position: "sticky",
    showOnPostPage: true,
    configId: "ad2",
  },
],
```

## 移动端底部组件

移动端（<768px）底部组件通过 `mobileBottomComponents` 数组配置，这些组件独立于左右侧边栏配置。

### 默认配置

```ts
mobileBottomComponents: [
  {
    type: "profile",
    enable: true,
    showOnPostPage: true,
  },
  {
    type: "announcement",
    enable: true,
    showOnPostPage: true,
  },
  {
    type: "music",
    enable: true,
    showOnPostPage: true,
  },
  {
    type: "categories",
    enable: true,
    showOnPostPage: true,
    responsive: {
      collapseThreshold: 5,
    },
  },
  {
    type: "tags",
    enable: false,
    showOnPostPage: true,
    responsive: {
      collapseThreshold: 20,
    },
  },
  {
    type: "stats",
    enable: false,
    showOnPostPage: true,
  },
],
```

::: warning
移动端组件不支持 `position` 属性，因为它们都显示在页面底部。
:::

## 配置示例

### 仅使用左侧边栏

```ts
export const sidebarLayoutConfig: SidebarLayoutConfig = {
  enable: true,
  position: "left",
  showBothSidebarsOnPostPage: true, // 文章页显示双侧栏以显示目录
  leftComponents: [
    // ...左侧组件配置
  ],
  rightComponents: [
    {
      type: "sidebarToc",
      enable: true,
      position: "sticky",
      showOnPostPage: true,
      showOnNonPostPage: false,
    },
  ],
  // ...
};
```

### 禁用某个组件

```ts
{
  type: "music",
  enable: false, // 禁用音乐播放器
  position: "sticky",
  showOnPostPage: true,
},
```

### 仅在文章页显示目录

```ts
{
  type: "sidebarToc",
  enable: true,
  position: "sticky",
  showOnPostPage: true,
  showOnNonPostPage: false, // 非文章页不显示
},
```

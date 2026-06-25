# 字体配置

字体配置文件用于设置博客使用的字体，支持自定义字体、CDN 字体和本地字体。

配置文件路径：[fontConfig.ts](file:///e:/AItool/zzwork/my-blog/src/config/fontConfig.ts)

## 基础配置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `enable` | `boolean` | `true` | 是否启用自定义字体功能 |
| `preload` | `boolean` | `true` | 是否预加载字体文件 |
| `selected` | `string[]` | `["aazongyiyuan"]` | 当前选择的字体，支持多个字体组合 |
| `fallback` | `string[]` | 见源文件 | 全局字体回退列表 |

::: warning
- 推荐使用可靠的 CDN 服务商提供的字体链接，天然做了按需分片加载，性能较好
- 使用本地字体文件需自行进行字体子集化处理，否则字体文件庞大可能导致页面加载缓慢
- 字体子集化处理会导致动态内容（如评论、Bangumi 等）无法正确显示字体，因此不推荐使用本地字体文件
:::

## 字体配置

通过 `fonts` 对象配置可用字体列表，每个字体支持以下属性：

| 属性 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `id` | `string` | 是 | 字体唯一标识 |
| `name` | `string` | 是 | 字体显示名称 |
| `src` | `string` | 是 | 字体源地址（CSS 链接或本地字体文件路径），系统字体留空 |
| `family` | `string` | 是 | CSS font-family 值 |
| `format` | `string` | 否 | 字体格式（仅本地字体需要），如 `"woff2"` |
| `weight` | `number` | 否 | 字重，如 `400`、`500`、`600` |
| `display` | `"auto" \| "block" \| "swap" \| "fallback" \| "optional"` | 否 | font-display 属性，推荐 `"swap"` |

## 预设字体

主题内置了以下预设字体：

### 系统字体

| 配置项 | 值 |
|--------|-----|
| `id` | `"system"` |
| `name` | `"系统字体"` |
| `src` | `""`（系统字体无需源） |
| `family` | `"system-ui, -apple-system, BlinkMacSystemFont, Segoe UI, Roboto, sans-serif"` |

### Inter（Google Fonts）

| 配置项 | 值 |
|--------|-----|
| `id` | `"inter"` |
| `name` | `"Inter"` |
| `src` | `"https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap"` |
| `family` | `"Inter"` |
| `display` | `"swap"` |

### Aa综艺圆（本地字体）

| 配置项 | 值 |
|--------|-----|
| `id` | `"aazongyiyuan"` |
| `name` | `"Aa综艺圆"` |
| `src` | `"/fonts/AaZongYiYuan/AaZongYiYuan-2.woff2"` |
| `family` | `"AaZongYiYuan"` |
| `format` | `"woff2"` |
| `display` | `"swap"` |

### MiSans Normal（小米字体）

| 配置项 | 值 |
|--------|-----|
| `id` | `"misans-normal"` |
| `name` | `"MiSans Normal"` |
| `src` | `"https://unpkg.com/misans@4.1.0/lib/Normal/MiSans-Normal.min.css"` |
| `family` | `"MiSans"` |
| `weight` | `400` |
| `display` | `"swap"` |

### MiSans Regular（小米字体）

| 配置项 | 值 |
|--------|-----|
| `id` | `"misans-regular"` |
| `name` | `"MiSans Regular"` |
| `src` | `"https://unpkg.com/misans@4.1.0/lib/Normal/MiSans-Regular.min.css"` |
| `family` | `"MiSans"` |
| `weight` | `500` |
| `display` | `"swap"` |

### MiSans Semibold（小米字体）

| 配置项 | 值 |
|--------|-----|
| `id` | `"misans-semibold"` |
| `name` | `"MiSans Semibold"` |
| `src` | `"https://unpkg.com/misans@4.1.0/lib/Normal/MiSans-Semibold.min.css"` |
| `family` | `"MiSans"` |
| `weight` | `600` |
| `display` | `"swap"` |

## 添加自定义字体

### 添加 CDN 字体（推荐）

可以添加 Google Fonts、字由（iconfont）等 CDN 提供的字体：

```ts
fonts: {
  // ...现有字体
  "noto-sans-sc": {
    id: "noto-sans-sc",
    name: "思源黑体",
    src: "https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@400;500;700&display=swap",
    family: "Noto Sans SC",
    display: "swap",
  },
},
```

### 添加本地字体

::: warning
不推荐使用本地字体，除非已进行子集化处理。
:::

```ts
fonts: {
  // ...现有字体
  "my-font": {
    id: "my-font",
    name: "我的字体",
    src: "/fonts/MyFont/MyFont.woff2",
    family: "MyFont",
    format: "woff2",
    display: "swap",
  },
},
```

### 字体组合

`selected` 支持多个字体组合，会按顺序加载：

```ts
selected: ["misans-regular", "misans-semibold", "aazongyiyuan"],
```

## 默认回退字体

```ts
fallback: [
  "system-ui",
  "-apple-system",
  "BlinkMacSystemFont",
  "Segoe UI",
  "Roboto",
  "sans-serif",
],
```

这些字体将在自定义字体加载失败或不支持时作为回退显示。

## 完整配置示例

```ts
export const fontConfig = {
  enable: true,
  preload: true,
  selected: ["misans-normal"] as string[],
  fonts: {
    system: {
      id: "system",
      name: "系统字体",
      src: "",
      family: "system-ui, -apple-system, BlinkMacSystemFont, Segoe UI, Roboto, sans-serif",
    },
    inter: {
      id: "inter",
      name: "Inter",
      src: "https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap",
      family: "Inter",
      display: "swap" as const,
    },
    "noto-sans-sc": {
      id: "noto-sans-sc",
      name: "思源黑体",
      src: "https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@400;500;700&display=swap",
      family: "Noto Sans SC",
      display: "swap" as const,
    },
    "misans-normal": {
      id: "misans-normal",
      name: "MiSans Normal",
      src: "https://unpkg.com/misans@4.1.0/lib/Normal/MiSans-Normal.min.css",
      family: "MiSans",
      weight: 400,
      display: "swap" as const,
    },
  },
  fallback: [
    "system-ui",
    "-apple-system",
    "BlinkMacSystemFont",
    "Segoe UI",
    "Roboto",
    "sans-serif",
  ] as string[],
};
```

::: tip
- 用户可以通过博客主题切换器选择不同字体
- 使用 `display: "swap"` 可以避免字体加载期间的不可见文本问题（FOIT）
:::

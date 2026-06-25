# 封面图片配置

封面图片配置文件用于设置文章封面图的显示和随机封面图功能。

配置文件路径：`coverImageConfig.ts`

## 基础配置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `enableInPost` | `boolean` | `true` | 是否在文章详情页显示封面图 |

### 示例

```ts
export const coverImageConfig: CoverImageConfig = {
  enableInPost: true,
  // ...
};
```

::: tip
将 `enableInPost` 设为 `false` 可以在文章详情页隐藏封面图，只在文章列表中显示。
:::

## 随机封面图配置

随机封面图功能通过 `randomCoverImage` 对象配置。启用后，在文章 Frontmatter 中设置 `image: "api"` 即可使用随机封面。

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `randomCoverImage.enable` | `boolean` | `false` | 随机封面图功能开关 |
| `randomCoverImage.apis` | `string[]` | 见源文件 | 封面图 API 列表 |
| `randomCoverImage.fallback` | `string` | `"assets/images/cover.avif"` | API 失败时的回退图片路径 |
| `randomCoverImage.showLoading` | `boolean` | `false` | 是否显示加载动画 |

### 默认 API 列表

```ts
apis: [
  "https://t.alcy.cc/pc",
  "https://www.dmoe.cc/random.php",
  "https://uapis.cn/api/v1/random/image?category=acg&type=pc",
],
```

### 使用方法

在文章的 Frontmatter 中添加 `image: "api"` 即可启用随机封面图：

```markdown
---
title: 文章标题
date: 2024-01-01
image: "api"
---

文章内容...
```

::: tip
系统会依次尝试配置的所有 API，全部失败后使用 `fallback` 指定的备用图片。
:::

### 图片路径格式

回退图片路径支持两种格式：

| 格式 | 说明 | 示例 |
|------|------|------|
| src 目录 | 不以 `/` 开头，自动优化（推荐） | `"assets/images/cover.avif"` |
| public 目录 | 以 `/` 开头，不优化 | `"/assets/images/cover.avif"` |

## 完整配置示例

### 启用随机封面图

```ts
export const coverImageConfig: CoverImageConfig = {
  enableInPost: true,
  randomCoverImage: {
    enable: true,
    apis: [
      "https://t.alcy.cc/pc",
      "https://www.dmoe.cc/random.php",
    ],
    fallback: "assets/images/cover.avif",
    showLoading: true,
  },
};
```

### 禁用随机封面图

```ts
export const coverImageConfig: CoverImageConfig = {
  enableInPost: true,
  randomCoverImage: {
    enable: false,
    apis: [],
    fallback: "assets/images/cover.avif",
    showLoading: false,
  },
};
```

### 文章详情页不显示封面

```ts
export const coverImageConfig: CoverImageConfig = {
  enableInPost: false,
  randomCoverImage: {
    enable: true,
    apis: [
      "https://t.alcy.cc/pc",
    ],
    fallback: "assets/images/cover.avif",
    showLoading: false,
  },
};
```

::: warning
- 随机封面图 API 为第三方服务，稳定性无法保证，建议配置多个 API 和本地回退图片
- 如果文章 Frontmatter 中已指定 `image` 字段（非 `"api"`），则使用指定图片，不会使用随机封面
:::

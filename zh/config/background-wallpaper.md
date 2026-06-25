# 背景壁纸配置

背景壁纸配置文件用于设置博客的背景图片、显示模式、横幅效果等。

配置文件路径：[backgroundWallpaper.ts](file:///e:/AItool/zzwork/my-blog/src/config/backgroundWallpaper.ts)

## 基础配置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `mode` | `"banner" \| "overlay" \| "none"` | `"banner"` | 壁纸模式：`banner` 横幅壁纸，`overlay` 全屏透明，`none` 纯色背景 |
| `switchable` | `boolean` | `true` | 是否允许用户通过导航栏切换壁纸模式，设为 `false` 可提升性能 |

### 模式说明

- **banner**：横幅模式，顶部显示横幅壁纸，下方为纯色背景
- **overlay**：全屏透明覆盖模式，壁纸作为半透明背景覆盖整个页面
- **none**：纯色背景，无壁纸

::: tip
将 `switchable` 设为 `false` 可以提升性能，因为只渲染当前模式。
:::

## 背景图片配置

背景图片通过 `src` 对象配置，支持桌面端和移动端分别设置。

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `src.desktop` | `string \| string[]` | 见源文件 | 桌面端背景图片 |
| `src.mobile` | `string \| string[]` | 见源文件 | 移动端背景图片 |

### 图片路径格式

图片路径支持三种格式：

| 格式 | 说明 | 示例 |
|------|------|------|
| public 目录 | 以 `/` 开头，不优化 | `"/assets/images/banner.avif"` |
| src 目录 | 不以 `/` 开头，自动优化（推荐） | `"assets/images/banner.avif"` |
| 远程 URL | 网络图片或随机图 API | `"https://t.alcy.cc/pc"` |

::: warning
远程 URL 和 public 目录的图片不会被优化，请确保图片体积足够小以免影响加载速度。
:::

::: tip
建议不要替换 d1-d6、m1-m6 这些默认示例图片，因为以后可能会更换示例图片导致自定义图片被覆盖。使用自己的图片时请命名为其他名称。
:::

### 单张图片配置

```ts
src: {
  desktop: "assets/images/DesktopWallpaper/my-wallpaper.avif",
  mobile: "assets/images/MobileWallpaper/my-mobile-wallpaper.avif",
},
```

### 随机图 API 配置

```ts
src: {
  desktop: "https://t.alcy.cc/pc",
  mobile: "https://t.alcy.cc/mp",
},
```

### 多张图片随机显示

配置数组时，每次刷新页面会随机显示一张：

```ts
src: {
  desktop: [
    "assets/images/DesktopWallpaper/d1.avif",
    "assets/images/DesktopWallpaper/d2.avif",
    "assets/images/DesktopWallpaper/d3.avif",
  ],
  mobile: [
    "assets/images/MobileWallpaper/m1.avif",
    "assets/images/MobileWallpaper/m2.avif",
  ],
},
```

## Banner 模式配置

Banner 模式特有配置在 `banner` 对象中。

### 图片位置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `banner.position` | `string` | `"0% 20%"` | 图片位置，支持 CSS `object-position` 值 |

常用位置值：
- `'center'`：居中
- `'top'`：顶部居中
- `'bottom'`：底部居中
- `'left'`：左侧居中
- `'right'`：右侧居中
- `'25% 75%'`：自定义百分比位置
- `'10px 20px'`：自定义像素位置

### 主页横幅文字

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `banner.homeText.enable` | `boolean` | `false` | 是否启用主页横幅文字 |
| `banner.homeText.switchable` | `boolean` | `true` | 是否允许用户切换横幅标题显示 |
| `banner.homeText.title` | `string` | `"Lovely firefly!"` | 主标题 |
| `banner.homeText.titleSize` | `string` | `"3.8rem"` | 主标题字体大小 |
| `banner.homeText.subtitle` | `string[]` | 见源文件 | 副标题数组 |
| `banner.homeText.subtitleSize` | `string` | `"1.5rem"` | 副标题字体大小 |

### 打字机效果

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `banner.homeText.typewriter.enable` | `boolean` | `true` | 是否启用打字机效果 |
| `banner.homeText.typewriter.speed` | `number` | `100` | 打字速度（毫秒） |
| `banner.homeText.typewriter.deleteSpeed` | `number` | `50` | 删除速度（毫秒） |
| `banner.homeText.typewriter.pauseTime` | `number` | `2000` | 完全显示后的暂停时间（毫秒） |

::: tip
- 打字机开启：循环显示所有副标题
- 打字机关闭：每次刷新随机显示一条副标题
:::

### 图片来源

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `banner.credit.enable.desktop` | `boolean` | `true` | 桌面端是否显示图片来源 |
| `banner.credit.enable.mobile` | `boolean` | `true` | 移动端是否显示图片来源 |
| `banner.credit.text.desktop` | `string` | `"Pixiv - 晚晚喵"` | 桌面端来源文本 |
| `banner.credit.text.mobile` | `string` | `"Pixiv - KiraraShss"` | 移动端来源文本 |
| `banner.credit.url.desktop` | `string` | 见源文件 | 桌面端来源链接 |
| `banner.credit.url.mobile` | `string` | 见源文件 | 移动端来源链接 |

### 横幅导航栏

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `banner.navbar.transparentMode` | `"semi" \| "full" \| "semifull"` | `"semi"` | 透明模式：`semi` 半透明，`full` 完全透明，`semifull` 动态透明 |
| `banner.navbar.enableBlur` | `boolean` | `true` | 是否开启毛玻璃模糊效果，开启可能影响性能 |
| `banner.navbar.blur` | `number` | `5` | 毛玻璃模糊度 |

### 图片轮播

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `banner.carousel.enable` | `boolean` | `false` | 是否启用横幅图片轮播 |
| `banner.carousel.interval` | `number` | `5000` | 轮播切换间隔（毫秒） |
| `banner.carousel.switchable` | `boolean` | `false` | 是否允许用户切换轮播 |

::: warning
开启轮播可能会有点奇怪，为了让图片之间的切换自然，图片会在下一张加载完成后当前图片才会消失，所以会导致过渡有重影，可能影响观感。
:::

### 水波纹动画

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `banner.waves.enable.desktop` | `boolean` | `true` | 桌面端是否启用水波纹 |
| `banner.waves.enable.mobile` | `boolean` | `true` | 移动端是否启用水波纹 |
| `banner.waves.switchable` | `boolean` | `true` | 是否允许用户切换水波纹 |

::: warning
水波纹动画会影响页面性能，请根据设备性能选择是否开启。
:::

## Overlay 模式配置

全屏透明覆盖模式特有配置在 `overlay` 对象中。

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `overlay.switchable.opacity` | `boolean` | `true` | 是否允许用户调整透明度 |
| `overlay.switchable.blur` | `boolean` | `true` | 是否允许用户调整模糊度 |
| `overlay.switchable.cardOpacity` | `boolean` | `true` | 是否允许用户调整卡片透明度 |
| `overlay.zIndex` | `number` | `-1` | 壁纸层级，确保在背景层 |
| `overlay.opacity` | `number` | `0.8` | 壁纸透明度，0-1 之间 |
| `overlay.blur` | `number` | `10` | 背景模糊度 |
| `overlay.cardOpacity` | `number` | `0.5` | 卡片透明度，0-1 之间，值越小越透明 |

## 完整配置示例

```ts
export const backgroundWallpaper: BackgroundWallpaperConfig = {
  mode: "banner",
  switchable: true,
  src: {
    desktop: [
      "assets/images/DesktopWallpaper/d1.avif",
      "assets/images/DesktopWallpaper/d2.avif",
    ],
    mobile: "https://t.alcy.cc/mp",
  },
  banner: {
    position: "center",
    homeText: {
      enable: true,
      switchable: true,
      title: "我的博客",
      titleSize: "3.5rem",
      subtitle: ["欢迎来到我的博客", "分享技术，记录生活"],
      subtitleSize: "1.2rem",
      typewriter: {
        enable: true,
        speed: 80,
        deleteSpeed: 40,
        pauseTime: 2000,
      },
    },
    credit: {
      enable: { desktop: true, mobile: true },
      text: { desktop: "网络", mobile: "网络" },
      url: { desktop: "", mobile: "" },
    },
    navbar: {
      transparentMode: "semi",
      enableBlur: true,
      blur: 8,
    },
    carousel: {
      enable: false,
      interval: 5000,
      switchable: false,
    },
    waves: {
      enable: { desktop: true, mobile: false },
      switchable: true,
    },
  },
  overlay: {
    switchable: { opacity: true, blur: true, cardOpacity: true },
    zIndex: -1,
    opacity: 0.7,
    blur: 8,
    cardOpacity: 0.6,
  },
};
```

# 评论系统配置指南

Firefly 博客内置了多种评论系统支持，包括 Twikoo、Waline、Giscus、Artalk、Disqus 等。本文档将详细介绍如何配置和使用这些评论系统。

## 功能介绍

- 💬 **多评论系统支持**：Twikoo、Waline、Giscus、Artalk、Disqus
- 🌓 **明暗主题自适应**：自动跟随网站主题切换
- 🔄 **Swup 页面切换兼容**：无刷新页面切换时评论区自动重载
- 📊 **访问量统计**：支持文章阅读量统计（部分评论系统）
- 🎨 **样式深度整合**：评论系统样式与主题风格统一
- 🔒 **单篇文章控制**：可在 Frontmatter 中单独关闭某篇文章的评论

相关文件：
- 配置文件：`commentConfig.ts`
- 评论入口组件：`comment/index.astro`
- 各评论系统组件位于 `src/components/comment/` 目录

## 基础配置

评论系统配置文件位于 `src/config/commentConfig.ts`。

### 选择评论系统

通过 `type` 字段选择要使用的评论系统：

```typescript
import type { CommentConfig } from "../types/config";

export const commentConfig: CommentConfig = {
  // 评论系统类型: none, twikoo, waline, giscus, disqus, artalk
  // 默认为 none，即不启用评论系统
  type: "twikoo",
  
  // 各评论系统的具体配置...
};
```

::: tip 只启用一个
同一时间只启用一个评论系统，选择你部署好的那个即可。设置为 `"none"` 则完全关闭评论功能。
:::

### 文章级别的评论开关

可以在文章 Frontmatter 中单独控制是否显示评论：

```yaml
---
title: "文章标题"
comment: false  # 设为 false 则本文不显示评论区
---
```

`comment` 默认为 `true`，不设置则默认显示评论。

### 留言板页面

除了文章页面，评论组件也用于留言板（Guestbook）页面。留言板功能需要先配置好评论系统才能正常使用。

留言板页面开关在 `siteConfig.ts` 中：

```typescript
pages: {
  guestbook: true,  // 开启留言板页面
}
```

## Twikoo 配置

Twikoo 是一个简洁、安全、免费的静态网站评论系统，基于腾讯云开发/CloudBase/ Vercel 等部署。

### 配置项

```typescript
// src/config/commentConfig.ts
export const commentConfig: CommentConfig = {
  type: "twikoo",
  
  twikoo: {
    // Twikoo 云函数/服务地址
    envId: "https://your-twikoo-domain.com",
    // 语言设置
    lang: "zh-CN",
    // 是否启用文章访问量统计
    visitorCount: true,
  },
};
```

### 部署 Twikoo

Twikoo 支持多种部署方式，请参考 [Twikoo 官方文档](https://twikoo.js.org/)：

1. **Vercel 部署（推荐）**：一键部署到 Vercel
2. **腾讯云开发**：使用 CloudBase 云函数
3. **私有部署**：使用 Docker 自托管

获取到 `envId`（云环境 ID 或服务地址）后填入配置即可。

### 特性

- ✅ 支持匿名评论
- ✅ 支持 Markdown 格式
- ✅ 支持表情包
- ✅ 支持评论管理后台
- ✅ 支持反垃圾评论
- ✅ 支持邮件通知
- ✅ 文章访问量统计
- ✅ 暗色模式自适应

相关组件：`Twikoo.astro`

::: tip 静态资源说明
项目使用了定制的 Twikoo JS 文件来修复一些交互问题，位于 `public/assets/js/twikoo.nocss.js`，样式文件位于 `public/assets/css/twikoo.css` 和 `public/assets/css/twikoo-custom.css`。
:::

## Waline 配置

Waline 是一个简单、安全的评论系统，后端基于 Valine 衍生，支持多种数据库和部署方式。

### 配置项

```typescript
// src/config/commentConfig.ts
export const commentConfig: CommentConfig = {
  type: "waline",
  
  waline: {
    // Waline 后端服务地址
    serverURL: "https://your-waline-domain.vercel.app",
    // 语言设置
    lang: "zh-CN",
    // 表情包配置
    emoji: [
      "https://unpkg.com/@waline/emojis@1.4.0/weibo",
      "https://unpkg.com/@waline/emojis@1.4.0/bilibili",
      "https://unpkg.com/@waline/emojis@1.4.0/bmoji",
    ],
    // 登录模式
    login: "enable",
    // 是否启用文章访问量统计
    visitorCount: true,
  },
};
```

### login 模式说明

| 值 | 说明 |
|----|------|
| `"enable"` | 默认，允许访客匿名评论和用第三方 OAuth 登录评论，兼容性最佳 |
| `"force"` | 强制必须登录后才能评论，适合严格社区，关闭匿名评论 |
| `"disable"` | 禁止所有登录和 OAuth，仅允许匿名评论（填写昵称/邮箱） |

### 部署 Waline

Waline 支持多种部署方式，请参考 [Waline 官方文档](https://waline.js.org/)：

1. **Vercel 部署（推荐）**：一键部署
2. **Cloudflare Pages**：免费部署
3. **Docker 部署**：自托管
4. **Netlify**：一键部署

部署后获得的服务地址填入 `serverURL`。

### 特性

- ✅ 支持匿名评论和社交登录
- ✅ 支持 Markdown 格式
- ✅ 丰富的表情包支持
- ✅ 评论管理后台
- ✅ 反垃圾评论（Akismet）
- ✅ 邮件/微信通知
- ✅ 文章访问量统计
- ✅ 图片上传
- ✅ 暗色模式自适应

相关组件：`Waline.astro`

## Giscus 配置

Giscus 是基于 GitHub Discussions 的评论系统，完全免费，数据存储在 GitHub 仓库中。

### 配置项

```typescript
// src/config/commentConfig.ts
export const commentConfig: CommentConfig = {
  type: "giscus",
  
  giscus: {
    // GitHub 仓库（格式：用户名/仓库名）
    repo: "username/my-blog",
    // 仓库 ID（从 Giscus 官网获取）
    repoId: "R_kgDOxxxxxxx",
    // Discussions 分类
    category: "General",
    // 分类 ID
    categoryId: "DIC_kwDOxxxxxxx",
    // 评论映射方式
    mapping: "pathname",
    // 严格模式
    strict: "0",
    // 是否启用反应功能
    reactionsEnabled: "1",
    // 是否输出元数据
    emitMetadata: "0",
    // 输入框位置："top" 或 "bottom"
    inputPosition: "top",
    // 语言
    lang: "zh-CN",
    // 加载方式："lazy" 懒加载
    loading: "lazy",
  },
};
```

### 获取配置参数

1. 确保你的仓库是**公开**的
2. 确保你已在仓库中**安装了 Giscus App**
3. 访问 [Giscus 配置页面](https://giscus.app/zh-CN)
4. 按页面提示填入仓库信息
5. 在"启用 giscus"部分获取所有配置参数

### mapping 映射方式

| 值 | 说明 |
|----|------|
| `"pathname"` | 使用页面路径映射（推荐） |
| `"url"` | 使用完整 URL 映射 |
| `"title"` | 使用页面标题映射 |
| `"og:title"` | 使用 OG 标题映射 |
| `"specific"` | 使用特定字符串映射 |
| `"number"` | 使用特定 Discussion 编号 |

### 特性

- ✅ 完全免费，基于 GitHub
- ✅ 数据存储在自己的 GitHub 仓库
- ✅ 支持 Markdown
- ✅ 支持 GitHub 表情反应
- ✅ 暗色模式自动切换
- ✅ 无需额外部署后端
- ✅ 用户使用 GitHub 账号登录

::: warning 注意事项
- 仓库必须是公开的（Public）
- 用户需要有 GitHub 账号才能评论
- Giscus App 需要安装到对应仓库
- Discussion 功能需要在仓库设置中开启
:::

相关组件：`Giscus.astro`

## Artalk 配置

Artalk 是一款轻量、安全、易部署的自托管评论系统，使用 Go 语言编写后端。

### 配置项

```typescript
// src/config/commentConfig.ts
export const commentConfig: CommentConfig = {
  type: "artalk",
  
  artalk: {
    // Artalk 后端 API 地址
    server: "https://artalk.example.com/",
    // 语言设置
    locale: "zh-CN",
    // 是否启用文章访问量统计
    visitorCount: true,
  },
};
```

### 部署 Artalk

Artalk 需要自托管后端，支持 Docker 部署：

```bash
docker run -d \
  --name artalk \
  -p 8080:23366 \
  -v $(pwd)/data:/data \
  artalk/artalk-go
```

详细部署请参考 [Artalk 官方文档](https://artalk.js.org/)。

### 特性

- ✅ 自托管，数据完全可控
- ✅ Go 后端，性能优秀
- ✅ 支持匿名评论
- ✅ 多层级评论嵌套
- ✅ 表情包、图片上传
- ✅ 反垃圾、验证码
- ✅ 邮件、Telegram 等通知
- ✅ 多站点管理
- ✅ 暗色模式自适应
- ✅ 文章访问量统计

相关组件：`Artalk.astro`

## Disqus 配置

Disqus 是一个老牌的第三方评论系统，全球广泛使用。

### 配置项

```typescript
// src/config/commentConfig.ts
export const commentConfig: CommentConfig = {
  type: "disqus",
  
  disqus: {
    // Disqus 站点 shortname
    shortname: "your-disqus-shortname",
  },
};
```

### 获取 shortname

1. 注册 [Disqus](https://disqus.com/) 账号
2. 添加一个新站点
3. 获得的站点唯一标识就是 `shortname`

### 特性

- ✅ 全球知名，用户基数大
- ✅ 支持社交登录
- ✅ 评论管理后台
- ✅ 反垃圾系统（Akismet）
- ✅ 邮件通知

::: warning 注意事项
- Disqus 在中国大陆访问可能不稳定
- 免费版有广告
- 加载第三方 JS 可能影响页面速度
- 数据存储在 Disqus 服务器
:::

相关组件：`Disqus.astro`

## 评论系统对比

| 特性 | Twikoo | Waline | Giscus | Artalk | Disqus |
|------|--------|--------|--------|--------|--------|
| 部署难度 | 中等 | 简单 | 极简 | 中等 | 极简 |
| 需要后端 | ✅ | ✅ | ❌ | ✅ | ❌（第三方） |
| 数据存储 | 云开发/数据库 | 数据库 | GitHub | 自建数据库 | Disqus 服务器 |
| 匿名评论 | ✅ | ✅ | ❌ | ✅ | ✅ |
| 国内访问 | ✅ 优秀 | ✅ 优秀 | ⚠️ 取决于GitHub | ✅ 优秀 | ❌ 不稳定 |
| 访问量统计 | ✅ | ✅ | ❌ | ✅ | ❌ |
| 暗色模式 | ✅ | ✅ | ✅ | ✅ | ⚠️ |
| 管理后台 | ✅ | ✅ | GitHub | ✅ | ✅ |
| 表情包 | ✅ | ✅ | GitHub表情 | ✅ | ✅ |
| 图片上传 | ✅ | ✅ | ❌ | ✅ | ✅ |
| 邮件通知 | ✅ | ✅ | ❌ | ✅ | ✅ |

### 推荐选择

- **国内用户**：推荐 Twikoo 或 Waline，访问速度快
- **开发者/GitHub 用户**：推荐 Giscus，无需部署后端
- **注重数据可控**：推荐 Artalk 自托管
- **面向国际用户**：Disqus 是传统选择

## 主题集成说明

所有评论系统组件都已与 Firefly 主题深度整合：

### 主题色适配

评论系统的主题色通过 CSS 变量自动适配，与网站主题色保持一致：

```css
--primary       /* 主题色 */
--card-bg       /* 卡片背景色 */
--line-divider  /* 分割线颜色 */
```

### 暗色模式切换

评论系统会自动监听 `html.dark` 类名变化，在明暗主题切换时自动更新评论区主题。

### Swup 页面兼容

评论组件已处理 Swup 无刷新页面切换场景：
- 页面切换时评论区自动重新初始化
- 防止重复加载脚本和绑定事件
- 正确传递页面路径参数

相关组件入口：`comment/index.astro`

## 访问量统计

Twikoo、Waline、Artalk 都支持文章访问量统计功能：

```typescript
// 在对应评论配置中启用
visitorCount: true
```

访问量显示组件：`VisitorCount.astro`

::: tip 说明
- 访问量数据由评论系统后端存储和管理
- 如果评论系统未启用或不支持，访问量统计不会显示
- 部分主题位置（如文章卡片）的访问量展示依赖此功能
:::

## 常见问题

### 1. 评论区不显示

**检查清单**：
1. 确认 `commentConfig.type` 不是 `"none"`
2. 确认对应评论系统的配置参数正确
3. 检查浏览器控制台是否有报错
4. 确认评论系统后端服务正常运行（Twikoo/Waline/Artalk）
5. 确认文章 Frontmatter 中 `comment` 不是 `false`

### 2. Giscus 加载失败

**可能原因和解决方案**：
1. 仓库不是公开的 → 将仓库设为 Public
2. Giscus App 未安装 → 在仓库设置中安装 Giscus
3. Discussion 功能未开启 → 在仓库 Settings → General 中开启 Discussions
4. repoId/categoryId 错误 → 重新在 [giscus.app](https://giscus.app) 获取
5. GitHub 访问问题 → 检查网络连接

### 3. 评论区加载慢

**可能原因**：
- 评论系统服务器在海外（如 Disqus）
- 评论系统后端响应慢
- 网络问题

**解决方案**：
- 选择国内访问良好的服务（Twikoo/Waline）
- 使用 `loading: "lazy"` 懒加载评论
- 考虑自托管评论系统

### 4. 切换页面后评论区不更新

这是 Swup 无刷新页面切换的问题，主题已做兼容处理。如果仍有问题：
1. 检查是否有自定义脚本干扰
2. 确保使用的是主题内置的评论组件
3. 查看浏览器控制台是否有错误

### 5. 暗色模式下评论区样式异常

主题已为 Giscus、Twikoo、Waline、Artalk 配置了暗色模式同步。如果自定义了评论系统样式，需要确保暗色模式样式正确。

### 6. 如何自定义评论区样式

评论区样式可以通过自定义 CSS 覆盖：

- Twikoo: 编辑 `public/assets/css/twikoo-custom.css`
- Waline/Artalk/Disqus/Giscus: 在全局 CSS 中添加自定义样式

::: warning 注意
不建议直接修改 `node_modules` 中的样式文件，更新后会丢失。优先使用自定义 CSS 文件覆盖。
:::

## 相关文件链接

| 文件 | 说明 |
|------|------|
| `commentConfig.ts` | 评论系统配置文件 |
| `comment/index.astro` | 评论组件入口（根据配置加载对应系统） |
| `Twikoo.astro` | Twikoo 评论组件 |
| `Waline.astro` | Waline 评论组件 |
| `Giscus.astro` | Giscus 评论组件 |
| `Artalk.astro` | Artalk 评论组件 |
| `Disqus.astro` | Disqus 评论组件 |
| `VisitorCount.astro` | 访问量统计组件 |
| `twikoo-custom.css` | Twikoo 自定义样式 |
| `twikoo.css` | Twikoo 基础样式 |

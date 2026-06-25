# 部署指南

Firefly 博客是纯静态站点，可以部署到任何支持静态文件托管的平台。本文档将详细介绍各种部署方式的配置方法。

::: tip 前置准备
在部署之前，请确保：
1. 已正确配置 `siteConfig.ts` 中的 `site_url` 为你的实际域名
2. 已安装项目依赖：`pnpm install`
3. 本地构建测试通过：`pnpm build`
:::

## 构建命令

项目使用 pnpm 作为包管理器，构建命令如下：

```bash
# 安装依赖
pnpm install

# 开发模式（本地预览）
pnpm dev

# 构建生产版本
pnpm build

# 预览构建结果
pnpm preview
```

构建完成后，所有静态文件将输出到 `dist/` 目录。

::: warning 构建说明
`pnpm build` 命令会执行以下操作：
1. 生成图标资源 (`node scripts/generate-icons.js`)
2. Astro 构建静态站点
3. Pagefind 生成全文搜索索引

构建时间取决于文章数量和图片数量，首次构建可能需要几分钟。
:::

## Vercel 部署（推荐）

Vercel 是 Astro 官方推荐的部署平台，提供零配置部署、全球 CDN 和自动 HTTPS。

### 方式一：通过 Vercel 控制台部署

1. 将代码推送到 GitHub/GitLab/Bitbucket 仓库
2. 登录 [Vercel](https://vercel.com/)，点击 "New Project"
3. 导入你的博客仓库
4. 配置项目：
   - **Framework Preset**: 选择 `Astro`
   - **Build Command**: `pnpm build`（Vercel 会自动识别）
   - **Output Directory**: `dist`（Vercel 会自动识别）
   - **Install Command**: `pnpm install`
5. 点击 "Deploy"

### 方式二：使用 vercel.json 配置

在项目根目录创建 `vercel.json` 文件：

```json
{
  "$schema": "https://openapi.vercel.sh/vercel.json",
  "buildCommand": "pnpm build",
  "outputDirectory": "dist",
  "framework": "astro",
  "installCommand": "pnpm install",
  "headers": [
    {
      "source": "/_astro/(.*)",
      "headers": [
        {
          "key": "Cache-Control",
          "value": "public, max-age=31536000, immutable"
        }
      ]
    },
    {
      "source": "/assets/(.*)",
      "headers": [
        {
          "key": "Cache-Control",
          "value": "public, max-age=31536000, immutable"
        }
      ]
    },
    {
      "source": "/(.*).html",
      "headers": [
        {
          "key": "Cache-Control",
          "value": "public, max-age=0, must-revalidate"
        }
      ]
    }
  ]
}
```

### 环境变量配置

在 Vercel 项目设置的 "Environment Variables" 中添加：

| 变量名 | 说明 | 示例值 |
|--------|------|--------|
| `NODE_VERSION` | Node.js 版本 | `20` |

::: tip Vercel 优势
- 自动识别 Astro 框架，零配置
- 每次 Git 推送自动部署
- 预览部署（Pull Request 时自动生成预览链接）
- 全球 CDN 加速
- 自动 HTTPS
:::

## Netlify 部署

Netlify 也是一个流行的静态托管平台，提供类似 Vercel 的功能。

### 部署步骤

1. 将代码推送到 GitHub/GitLab/Bitbucket
2. 登录 [Netlify](https://www.netlify.com/)，点击 "New site from Git"
3. 选择你的仓库
4. 配置构建选项：
   - **Build command**: `pnpm build`
   - **Publish directory**: `dist`
5. 点击 "Deploy site"

### 使用 netlify.toml 配置

在项目根目录创建 `netlify.toml`：

```toml
[build]
  command = "pnpm build"
  publish = "dist"

[build.environment]
  NODE_VERSION = "20"

[[headers]]
  for = "/_astro/*"
  [headers.values]
    Cache-Control = "public, max-age=31536000, immutable"

[[headers]]
  for = "/assets/*"
  [headers.values]
    Cache-Control = "public, max-age=31536000, immutable"

[[headers]]
  for = "/*.html"
  [headers.values]
    Cache-Control = "public, max-age=0, must-revalidate"
```

### Node.js 版本配置

在 `package.json` 中添加：

```json
{
  "engines": {
    "node": ">=20.0.0"
  }
}
```

## Cloudflare Pages 部署

Cloudflare Pages 提供免费的全球 CDN 和无服务器函数支持。

### 部署步骤

1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. 进入 "Pages"，点击 "Create a project"
3. 连接你的 Git 仓库
4. 配置构建设置：
   - **Framework preset**: `Astro`
   - **Build command**: `pnpm build`
   - **Build output directory**: `dist`
   - **Root directory**: `/`（保持默认）
5. 在 "Environment variables" 中添加：
   - `NODE_VERSION`: `20`
6. 点击 "Save and Deploy"

### 使用 _headers 文件配置缓存

在 `public/` 目录创建 `_headers` 文件：

```
/_astro/*
  Cache-Control: public, max-age=31536000, immutable

/assets/*
  Cache-Control: public, max-age=31536000, immutable

/*.html
  Cache-Control: public, max-age=0, must-revalidate
```

::: tip Cloudflare Pages 注意事项
- Cloudflare Pages 默认使用较旧的 Node.js 版本，务必设置 `NODE_VERSION=20`
- 如果构建遇到内存不足问题，可以设置 `NODE_OPTIONS=--max_old_space_size=4096`
:::

## GitHub Pages 部署

GitHub Pages 是 GitHub 提供的免费静态托管服务。

### 方式一：使用 GitHub Actions 自动部署

在项目根目录创建 `.github/workflows/deploy.yml`：

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup pnpm
        uses: pnpm/action-setup@v4
        with:
          version: 9

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: pnpm

      - name: Install dependencies
        run: pnpm install

      - name: Build
        run: pnpm build

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./dist

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

### 配置 siteConfig.ts

如果部署到 `https://username.github.io/repo-name/`，需要修改配置：

`siteConfig.ts`:
```typescript
export const siteConfig: SiteConfig = {
  site_url: "https://username.github.io/repo-name/",
  // ...
};
```

`astro.config.mjs`:
```javascript
export default defineConfig({
  site: "https://username.github.io/repo-name/",
  base: "/repo-name/",  // 如果是用户/组织主页（username.github.io），则设置为 "/"
  // ...
});
```

### 启用 GitHub Pages

1. 推送到 main 分支
2. 进入仓库 Settings → Pages
3. 在 "Build and deployment" → "Source" 选择 "GitHub Actions"
4. 等待 Action 执行完成

::: warning GitHub Pages 注意事项
- 如果部署到仓库子路径（`/repo-name/`），必须设置 `base` 配置
- Jekyll 默认会忽略以 `_` 开头的文件和文件夹，Astro 构建的 `_astro` 目录需要特别处理。**但使用 GitHub Actions 部署不会有这个问题**，因为是直接上传 dist 目录
:::

### 方式二：使用 .nojekyll 文件

为了防止 Jekyll 处理，确保在 `public/` 目录创建 `.nojekyll` 文件：

```bash
# public/.nojekyll - 空文件即可
```

这个文件会被复制到 dist 目录，告诉 GitHub Pages 不要使用 Jekyll 处理。

## 传统服务器部署（Nginx 等）

如果你有自己的服务器，可以使用 Nginx、Caddy 或 Apache 托管静态文件。

### 构建并上传

```bash
# 本地构建
pnpm build

# 将 dist 目录内容上传到服务器
# 例如使用 rsync:
rsync -avz dist/ user@your-server:/var/www/blog/
```

### Nginx 配置示例

```nginx
server {
    listen 80;
    listen 443 ssl http2;
    server_name your-domain.com;

    # SSL 配置（建议使用 Let's Encrypt）
    ssl_certificate /etc/letsencrypt/live/your-domain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/your-domain.com/privkey.pem;

    root /var/www/blog;
    index index.html;

    # 静态资源缓存
    location ~* /_astro/ {
        expires 1y;
        add_header Cache-Control "public, immutable";
        try_files $uri =404;
    }

    location ~* /assets/ {
        expires 1y;
        add_header Cache-Control "public, immutable";
        try_files $uri =404;
    }

    # HTML 文件不缓存
    location ~* \.html$ {
        add_header Cache-Control "public, max-age=0, must-revalidate";
        try_files $uri $uri/ $uri/index.html =404;
    }

    # SPA 路由回退
    location / {
        try_files $uri $uri/ $uri/index.html /index.html;
    }

    # 启用 gzip 压缩
    gzip on;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript image/svg+xml;
}
```

### Caddy 配置示例

Caddy 是一个现代化的 Web 服务器，自动 HTTPS：

```Caddyfile
your-domain.com {
    root * /var/www/blog
    file_server
    encode gzip

    @cached path /_astro/* /assets/*
    header @cached Cache-Control "public, max-age=31536000, immutable"

    @html path *.html
    header @html Cache-Control "public, max-age=0, must-revalidate"

    try_files {path} {path}/ {path}/index.html /index.html
}
```

## Docker 部署

你也可以使用 Docker 容器化部署。

### 创建 Dockerfile

在项目根目录创建 `Dockerfile`：

```dockerfile
# 构建阶段
FROM node:20-alpine AS builder

RUN npm install -g pnpm@9

WORKDIR /app

COPY package.json pnpm-lock.yaml ./
RUN pnpm install --frozen-lockfile

COPY . .
RUN pnpm build

# 运行阶段
FROM nginx:alpine

COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

### 创建 .dockerignore

```
node_modules
dist
.git
.github
docs
.env
.env.*
```

### 构建和运行

```bash
# 构建镜像
docker build -t firefly-blog .

# 运行容器
docker run -d -p 80:80 --name firefly-blog firefly-blog
```

## 环境变量

可以在项目根目录创建 `.env` 文件来配置环境变量（参考 `.env.example`）：

```env
# 示例环境变量
# 注意：以 PUBLIC_ 开头的变量会暴露到客户端
```

::: warning 安全提示
- 不要将 `.env` 文件提交到 Git 仓库（已在 `.gitignore` 中忽略）
- 私密信息（API Key 等）不要以 `PUBLIC_` 开头
:::

## 缓存策略说明

为了获得最佳性能，建议配置以下缓存策略（在 `astro.config.mjs` 中有注释说明）：

| 路径 | Cache-Control | 说明 |
|------|---------------|------|
| `/_astro/*` | `public, max-age=31536000, immutable` | 构建产物带哈希指纹，可长期缓存 |
| `/assets/*` | `public, max-age=31536000, immutable` | public 目录下的静态资源 |
| `/*.html` | `public, max-age=0, must-revalidate` | HTML 文件始终验证更新 |

## 部署检查清单

在正式部署前，请检查以下项目：

::: tip 部署前检查
- [ ] `siteConfig.site_url` 已设置为正确的生产域名
- [ ] 如果部署到子路径，`astro.config.mjs` 中的 `base` 已正确配置
- [ ] 评论系统（Giscus/Twikoo/Waline 等）已配置正确的域名
- [ ] 统计分析（Google Analytics/Umami 等）已配置
- [ ] `pnpm build` 本地构建无错误
- [ ] 所有图片资源可正常访问
- [ ] 音乐播放器（如使用）API 地址可访问
- [ ] RSS 订阅链接正确
- [ ] sitemap.xml 可正常访问
:::

## 常见问题

### 1. 构建时内存不足

**症状**: 构建过程中出现 JavaScript heap out of memory

**解决方案**:
```bash
# 增加 Node.js 内存限制
NODE_OPTIONS=--max_old_space_size=4096 pnpm build
```

### 2. 页面刷新后 404

**症状**: 在非首页刷新时出现 404 错误

**原因**: 静态托管平台需要配置 SPA 回退规则

**解决方案**:
- Vercel/Netlify/Cloudflare Pages: 自动处理
- Nginx: 配置 `try_files`（参考上面的 Nginx 配置）
- GitHub Pages: 目前不支持，需要使用其他方案或 Hash 路由（不推荐）

### 3. 图片加载 403 错误

**症状**: 部分外链图片显示 403 防盗链错误

**解决方案**: 在 `siteConfig.ts` 的 `imageOptimization.noReferrerDomains` 中添加相关域名：

```typescript
imageOptimization: {
  noReferrerDomains: ["i0.hdslb.com", "*.bilibili.com"],
  // ...
}
```

### 4. 部署后样式丢失

**症状**: 部署后页面样式异常

**可能原因**:
1. `base` 路径配置错误
2. CDN 缓存了旧版本
3. 构建不完整

**解决方案**:
1. 检查 `site_url` 和 `base` 配置
2. 清除 CDN 缓存
3. 重新构建部署

### 5. Pagefind 搜索不工作

**症状**: 搜索功能无法使用或返回空结果

**解决方案**:
- 确保 `pnpm build` 时 Pagefind 索引成功生成
- 检查 `dist/pagefind/` 目录是否存在
- 确认部署时包含了 pagefind 目录的所有文件

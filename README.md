# AI 音乐前端项目

## 项目介绍

音乐创作展示工作流 - 前端展示，基于 React + Vite 构建的古风音乐作品展示平台。

## 快速开始

```bash
# 安装依赖
npm install

# 开发模式
npm run dev

# 构建生产版本
npm run build

# 预览生产版本
npm run preview
```

## 项目架构

```
frontend/
├── src/
│   ├── components/    # UI 组件
│   ├── pages/         # 页面组件
│   ├── App.jsx       # 主应用
│   ├── App.css       # 主样式
│   ├── index.css     # 全局样式
│   └── catalog.json  # 音乐目录数据
├── public/           # 静态资源
├── vite.config.js   # Vite 配置
└── package.json
```

## 部署到 PinMe 网络

### 方式一：CLI 部署

```bash
# 安装 PinMe CLI
npm install -g pinme-cli

# 登录（首次使用）
pinme login

# 构建并部署
npm run build
pinme deploy dist
```

### 方式二：GitHub Actions 自动部署

1. 在 GitHub 仓库设置 `PINME_TOKEN` Secret
2. 创建 `.github/workflows/deploy.yml`:

```yaml
name: Deploy to PinMe

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Build
        run: npm run build
      
      - name: Deploy to PinMe
        run: npx pinme-cli deploy dist
        env:
          PINME_TOKEN: ${{ secrets.PINME_TOKEN }}
```

### 方式三：手动上传

1. 运行 `npm run build`
2. 访问 [pinme.eth.limo](https://pinme.eth.limo)
3. 将 `dist` 文件夹拖拽上传

## 数据源配置

前端通过 Vite 中间件代理读取 `/composition` 目录的音乐文件：

- **开发环境**：代理到项目根目录的 `composition/` 文件夹
- **生产环境**：需要将 `composition/` 文件夹部署到可访问的 CDN

### 数据源仓库

> [独立作品仓库 composition](https://github.com/xplan2026/ai-music/composition/)

该仓库包含所有音乐作品文件，前端通过相对路径 `/composition/xxx.mp3` 访问。

## 自定义域名（可选）

1. 在 PinMe 控制台添加自定义域名
2. 配置 DNS CNAME 记录指向 PinMe 提供的地址
3. 在 `.pinme` 配置文件中指定域名

## 环境变量

```env
# .env.pinme
DOMAIN=your-domain.com
```

## 相关链接

- [PinMe 官方文档](https://pinme.eth.limo/#/docs?id=getting-started)
- [PinMe GitHub Actions 部署指南](https://pinme.eth.limo/#/docs?id=github-actions-deployment)
- [PinMe 项目工作流](https://pinme.eth.limo/#/docs?id=project-workflow)


# CyberOracle / 赛博算卦

[中文](#中文) | [English](#english)

Live site / 部署地址: https://corookie.github.io/Cyber-Oracle/

## 中文

CyberOracle 是一个赛博玄学风格的单页算卦网页。用户输入问题或直接启动卦卜后，页面会播放六爻生成与卦轮同步动画，并随机得到一条易懂的周易风格卦辞。

### 功能

- 赛博朋克仪表盘式界面
- 六爻生成仪式动画
- 本地随机起卦逻辑
- 周易风格白话卦辞
- 卦象等级：大吉、吉、平、小凶、凶兆、大凶
- 大吉与凶兆专属结果页面
- 桌面端与移动端响应式适配

### 技术栈

- Vite 4
- Vue 3
- CSS 动画
- GitHub Pages

### 本地开发

```bash
npm install
npm run dev
```

### 构建

```bash
npm run build
```

### 部署

项目通过 GitHub Actions 部署到 GitHub Pages。推送到 `main` 分支后，workflow 会自动构建并发布 `dist`。

## English

CyberOracle is a cyber-divination single-page web app. Users can enter a question or start a reading directly, watch a six-line hexagram ritual animation, and receive an easy-to-understand I Ching-inspired oracle result.

### Features

- Cyberpunk dashboard interface
- Six-line casting ritual animation
- Local randomized oracle logic
- Plain-language I Ching-style readings
- Result levels: Great Fortune, Fortune, Neutral, Minor Omen, Omen, Major Omen
- Dedicated favorable and caution result screens
- Responsive desktop and mobile layout

### Tech Stack

- Vite 4
- Vue 3
- CSS animations
- GitHub Pages

### Local Development

```bash
npm install
npm run dev
```

### Build

```bash
npm run build
```

### Deployment

The project is deployed to GitHub Pages through GitHub Actions. Every push to `main` builds and publishes the `dist` directory.

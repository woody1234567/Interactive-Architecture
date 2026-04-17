# 網頁互動式建築模型 (Interactive Architecture)

An interactive 3D architectural visualization web application built with Nuxt 4, Vue 3, and Three.js. Explore detailed building models with multiple interaction modes to understand architectural design before and after construction.

---

## 功能特性 / Features

### 🏗️ 多個建築場景 / Multiple Architectural Scenarios
- **增建前/後 (Before/After)** — Compare building models before and after new construction phases
- **環境文脈 (Environmental Context)** — View buildings with surrounding environment for spatial understanding
- **詳細模型 (Detailed Models)** — High-quality GLTF models with proper lighting and shadows

### 🖱️ 靈活的互動方式 / Flexible Interaction Modes

#### 滑鼠控制 (Mouse Controls)
- **左鍵拖曳** — Rotate the model
- **右鍵拖曳** — Pan around
- **滾輪** — Zoom in/out
- Smooth damping and constraints to prevent disorientation

#### 鍵盤控制 (Keyboard Controls)
- **WASD** — Move forward/backward/left/right
- **R/F** — Move up/down
- **Q/E** — Rotate left/right
- First-person perspective for immersive exploration

### 🎨 現代化界面 / Modern UI
- Built with **@nuxt/ui** components and **Tailwind CSS 4**
- Responsive design for desktop and mobile
- Loading progress indicator for model downloads
- Control overlay with visual guides

### ⚡ 高性能渲染 / High-Performance Rendering
- Three.js with WebGL rendering
- Optimized shadow mapping and lighting
- Ground plane grid for spatial reference
- Proper resource cleanup to prevent memory leaks

---

## 快速開始 / Quick Start

### 安裝 / Installation

```bash
# Using pnpm
pnpm install
```

### 開發服務器 / Development Server

Start the development server at `http://localhost:3000`:

```bash
pnpm dev
```

### 生產構建 / Production Build

```bash
# Build for production
pnpm run build

# Preview the production build locally
pnpm run preview

# Generate static site
pnpm run generate
```

---

## 使用場景 / Use Cases

- **建築師展示** — Architects showcasing design iterations to clients
- **不動產營銷** — Real estate developers presenting pre/post-construction visuals
- **教育工具** — Students learning about architectural design and spatial relationships
- **工程規劃** — Engineers visualizing construction phases and environmental impact

---

## 技術棧 / Tech Stack

- **Framework**: [Nuxt 4](https://nuxt.com)
- **UI**: [Vue 3](https://vuejs.org) with Composition API + `<script setup>`
- **3D Graphics**: [Three.js](https://threejs.org)
- **Styling**: [Tailwind CSS 4](https://tailwindcss.com) + [@nuxt/ui](https://ui.nuxt.com)
- **Language**: TypeScript
- **Build**: Vite (via Nuxt)

---

## 項目結構 / Project Structure

```
app/
├── pages/           # 3D 場景和著陸頁面 (3D scenes and landing page)
├── components/      # 可複用組件 (Reusable Vue components)
├── layouts/         # 頁面佈局 (Layout wrappers)
└── assets/          # 圖片和樣式 (Images and styles)

public/
└── models/          # GLTF 3D 模型文件 (3D model files)
```

詳細見 [CLAUDE.md](./CLAUDE.md)。

---

## 頁面導航 / Navigation

| 頁面 | 功能 |
|------|------|
| `/` | 首頁 - 展示所有可用的場景 |
| `/arch_before_mouse` | 增建前 - 滑鼠控制 |
| `/arch_before_keyboard` | 增建前 - 鍵盤控制 |
| `/arch_before_with_env` | 增建前 - 含環境文脈 |
| `/arch_after_mouse` | 增建後 - 滑鼠控制 |
| `/arch_after_keyboard` | 增建後 - 鍵盤控制 |

---

## 開發指南 / Development Guide

### 添加新的 3D 場景
1. 在 `app/pages/` 中創建新的 `.vue` 文件
2. 複製現有的 3D 頁面結構（mouse 或 keyboard 版本）
3. 更新 GLTF 模型路徑
4. 調整模型位置和相機視角
5. 在 `index.vue` 的卡片數組中添加導航項

### 修改交互行為
- **滑鼠控制**: 編輯 `OrbitControls` 參數（damping、距離限制等）
- **鍵盤控制**: 修改 `FlyControls` 速度和自定義鍵盤事件處理

### 更新 UI
- 使用 @nuxt/ui 組件進行一致的設計
- Tailwind CSS 用於自定義樣式
- CSS 變量 `--ui-*` 用於主題顏色

---

## 性能注意事項 / Performance Notes

- 應用設置為 CSR（客戶端渲染）以最大化 Three.js 性能
- 使用 WebGL 渲染時監控 GPU 使用
- GLTF 模型在首次加載時進行優化（幾何體和材質）
- 在組件卸載時正確清理資源以防止內存洩漏

---

## 許可證 / License

MIT

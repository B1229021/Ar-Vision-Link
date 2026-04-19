# AR Vision Link

![WebAR](https://img.shields.io/badge/WebAR-MediaPipe-blue)
![FaceAPI](https://img.shields.io/badge/AI-face--api.js-orange)
![FastAPI](https://img.shields.io/badge/Backend-FastAPI-009688)
![Status](https://img.shields.io/badge/Status-Active_Development-success)

## 專案簡介 (Project Overview)
**AR Vision Link** 是一個「重前端、輕後端」的雙模組 Web 應用程式。本專案將擴增實境 (AR)、邊緣 AI 視覺運算與即時多人連線技術結合，創造出「刷臉&答題同樂」的沉浸式互動體驗。

專案由兩大核心模組組成：
1. **Vision 模組 (已實裝)**：基於瀏覽器端的高效能 AR 臉部追蹤與身分辨識系統。
2. **Link 模組 (開發中)**：基於 WebSocket 的高併發多人即時答題系統。

---

## 核心功能 (Key Features)

### Vision 模組：零延遲 WebAR 追蹤
* **邊緣 AI 運算**：使用 `MediaPipe FaceMesh` 實現高頻率 468 點 3D 臉部空間網格追蹤。
* **本地端特徵辨識**：整合 `face-api.js` 與 `WebAssembly (WASM)`，於客戶端提取 128 維臉部特徵矩陣 (Face Embeddings) 並進行歐幾里得距離比對，大幅降低伺服器負擔。
* **AR 遊戲化疊加**：動態定位並渲染虛擬寶箱與名牌，支援平滑跟隨 (Smooth Tracking) 與 CSS 動畫互動。

### Link 模組：即時答題
* **狀態機房間管理**：伺服器精準控管 LOBBY（大廳）、ACTIVE（答題）、結算等遊戲進程。

---

## 技術堆疊 (Tech Stack)

### 前端與 AI 視覺層 (Frontend & Edge AI)
* **Core**: HTML5, CSS3, Vanilla JavaScript (ES6 Modules)
* **Computer Vision**: [MediaPipe FaceMesh](https://google.github.io/mediapipe/solutions/face_mesh)
* **Face Recognition**: [face-api.js](https://github.com/vladmandic/face-api) (TensorFlow.js w/ WASM backend)
* **Cloud Database (Current)**: [Supabase](https://supabase.com/) (BaaS)

### 自建後端服務 (Standby Backend API)
* **Framework**: Python FastAPI
* **ORM**: SQLAlchemy
* **Database**: MySQL (`pymysql`)
* **Server**: Uvicorn

### 即時通訊層 (Real-time - Planned)
* **Protocol**: WebSocket / Socket.io

---

## 專案結構 (Project Structure)

```text
AR-VISION-LINK/                
├── README.md                
├── Ignore                     
└── webar-main/                # Vision 模組主程式 (AR 辨識系統)
    ├── index.html             # 專案入口大廳
    ├── README.md              # Vision 模組開發文件
    │
    ├── webar-frontend/        # 前端與 AI 運算層
    │   ├── camera.html        # AR 體驗與辨識主頁
    │   ├── register.html      # 使用者註冊頁面
    │   ├── script.js          # Vision 核心主控邏輯 (雙引擎協作)
    │   ├── register.js        # 註冊邏輯與特徵提取
    │   ├── camera_utils.js    # 鏡頭與 Frame Loop 控制
    │   ├── drawing_utils.js   # MediaPipe 繪圖工具
    │   ├── face_mesh.js       # MediaPipe 核心庫
    │   ├── face-api.min.js    # 人臉辨識引擎
    │   ├── style.css          # 全域樣式與 AR 動畫
    │   ├── chest.png          
    │   └── tfjs-backend-wasm* # TensorFlow.js WASM 加速檔案 (多執行緒/SIMD)
    │
    └── webar-backend/         # 自建 API 服務 (開發中)
        ├── app.py             # FastAPI 主程式
        ├── requirements.txt   # Python 依賴清單
        └── 目前沒用到後端     

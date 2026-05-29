# AR Vision Link 
**webar網頁版臉部辨識 + 多人線上即時制答題系統**

![WebAR](https://img.shields.io/badge/WebAR-MediaPipe-blue)
![FaceAPI](https://img.shields.io/badge/AI-face--api.js-orange)
![FastAPI](https://img.shields.io/badge/Backend-FastAPI-009688)
![WebSocket](https://img.shields.io/badge/RealTime-WebSocket-red)
![Status](https://img.shields.io/badge/Status-Active_Development-success)

## 專題簡介 (Project Overview)
**AR Vision Link** 是一個結合邊緣AI視覺運算與即時多人連線技術的雙模組 Web 應用系統。本專案將擴增實境 (AR)、邊緣 AI 視覺運算與即時多人連線技術結合，創造出「刷臉&答題同樂」的沉浸式互動體驗平台。

專題由兩大核心模組組成：
1. **Vision 模組 (已實裝)**：基於瀏覽器端的高效能 AR 臉部追蹤與身分辨識系統。
2. **Link 模組 (開發中)**：基於 WebSocket 的高併發即時多人答題與房間管理系統。

---

## 專題架構 (System Architecture)

本系統採用「重前端 (Heavy-Client)、微服務 (Microservices) 擴充」的架構設計，確保 AI 運算效能與連線的穩定性。

* **前端運算層 (Edge Computing)**：
    * 利用 WebAssembly (WASM) 將繁重的機器視覺任務（臉部 3D 網格建立、128 維特徵矩陣提取）完全卸載至客戶端瀏覽器執行，達成零延遲的 AR 追蹤體驗。
* **雲端/資料層 (Data Persistence)**：
    * 目前使用 **Supabase (BaaS)** 進行快速的特徵向量 (Face Embeddings) 讀寫與快取。
    * 並行開發 **FastAPI + MySQL** 自建後端，為未來的複雜關聯資料（如題庫、歷史作答紀錄）做準備。
* **即時通訊層 (Real-time Sync)**：
    * Link 模組將透過獨立的 **WebSocket 伺服器** 負責全雙工通訊，處理房間建立、題目廣播、以及各客戶端的作答狀態同步。

---

## 核心模組功能 (Core Features)

### Vision 模組：零延遲 WebAR 追蹤
* **邊緣 AI 運算**：使用 `MediaPipe FaceMesh` 實現高頻率 468 點 3D 臉部空間網格追蹤。
* **本地端特徵辨識**：整合 `face-api.js`，於客戶端即時計算特徵矩陣並進行歐幾里得距離比對，辨識使用者身分。

### Link 模組：多人即時答題系統 (WIP)
* **無縫入房 (Face-Login Integration)**：整合 Vision 模組，學生透過 AR 鏡頭辨識身分後，系統自動讀取資料並將其加入對應的測驗房間。
* **狀態機房間管理**：伺服器精準控管房間的生命週期，包含 LOBBY（等待大廳）、QUESTION_ACTIVE（作答倒數）、RESULT（單題結算）等狀態。
* **即時作答同步**：支援高併發連線，當老師派發題目時，所有連線客戶端毫秒級同步顯示題目；學生作答後，系統即時更新排行榜與正確率。

---

## 技術堆疊 (Tech Stack)

### 前端與 AI 視覺層 (Frontend & Edge AI)
* **Core**: HTML5, CSS3, Vanilla JavaScript (ES6 Modules)
* **Computer Vision**: [MediaPipe FaceMesh](https://google.github.io/mediapipe/solutions/face_mesh)
* **Face Recognition**: [face-api.js](https://github.com/vladmandic/face-api) (TensorFlow.js w/ WASM backend)

### 資料庫與後端 API (Database & REST API)
* **Cloud Database (Current)**: [Supabase](https://supabase.com/) (BaaS)
* **Custom Backend (Standby)**: Python FastAPI, SQLAlchemy, MySQL (`pymysql`)

### 即時通訊層 (Real-time Communication)
* **Protocol**: WebSocket (預計導入 Socket.io)

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
    │   ├── register.html      # 使用者註冊與臉部建檔頁面
    │   ├── script.js          # Vision 核心主控邏輯 
    │   ├── register.js        # 註冊邏輯 & 臉部特徵提取 
    │   ├── camera_utils.js    # WebRTC 鏡頭與 Frame Loop 控制
    │   ├── drawing_utils.js   # MediaPipe Canvas 繪製工具
    │   ├── face_mesh.js       # MediaPipe 核心庫
    │   ├── face-api.min.js    # 人臉辨識引擎
    │   ├── style.css          # 全域樣式 & AR疊加層動畫
    │   ├── chest.png          
    │   └── tfjs-backend-wasm* # TensorFlow.js WASM 加速檔案
    │
    └── webar-backend/         # 自建 API 服務 (準備階段)
        ├── app.py             # FastAPI 主程式與路由
        ├── requirements.txt   
        └── 目前沒用到後端     

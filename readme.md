# 🐾 PocketPet 手繪療癒記帳小窩

> 一款專為「懶得記帳、容易忘記」設計的手繪插畫風、0 秒極速記帳 Web App，支援手機主畫面 PWA 體驗、同物種世代繁衍房產、每月固定扣繳動態家具，以及 Supabase 雲端雙向即時同步。

---

## 📱 核心功能特色

1. **⚡ 1 秒極速記帳**：
   - 預設早餐、午餐、手搖飲等高頻膠囊，點擊 ⚡ 按鈕 1 秒完成今日記錄。
   - 點擊卡片可開啟微調滑桿、快捷步進（+10 / -10 / +50）、備註與自訂補記日期（今天／昨天／前天／自訂過往日期）。
2. **🏡 手繪 3D 房間與動態家具**：
   - 暖棕墨線＋粉彩水彩全手繪 Isometric 3D 立體房間。
   - 9 項每月固定扣繳開銷（美金保險、孝親費、失能險、房租、TPASS、電話費、Netflix、Grok、Gemini）化身房內精緻動態家具。點擊可查看加成，收起開銷家具即時從房間消失。
3. **🐾 6 大手繪萌寵與 5 階世代房產**：
   - 倉鼠丸子、柴犬旺財、橘貓胖橘、恐龍咔滋、青蛙呱呱、河馬嘟嘟。
   - 隨記帳累積經驗值，經歷：溫馨紙箱小窩 ➔ 陽光小套房 ➔ 門牌一房一廳 ➔ 雙層樓中樓成婚 ➔ 三代同堂透天厝繁衍下一代。
4. **📊 週／月全螢幕統計與歷史查詢**：
   - 自動隱藏寵物房間放大統計視野，支援月／週切換、增減比較、各分類佔比進度條、單筆 Top 3 排行榜與自訂區間篩選。
5. **☁️ Supabase 0 秒樂觀更新（Optimistic UI）**：
   - 本地優先，操作瞬間有感反饋，背景自動向 Supabase REST API 雙向同步。

---

## 🚀 VS Code 快速操作與同步到 GitHub

### 步驟 1：在 VS Code 中開啟專案資料夾

1. 在電腦上建立一個新資料夾（例如取名 `pocket-pet`）。
2. 將專案中的 **`index.html`** 與本份 **`README.md`** 放入該資料夾。
3. 開啟 VS Code，點選頂部選單 **檔案 (File)** ➔ **開啟資料夾 (Open Folder...)**，選取剛建立的資料夾。

---

### 步驟 2：同步至 GitHub（二選一）

#### 方式 A：使用 VS Code 內建介面（最直覺，無需記指令）

1. 點擊 VS Code 左側側邊欄的 **原始檔控制 (Source Control)** 圖示（或按快速鍵 `Ctrl + Shift + G` / macOS 為 `Cmd + Shift + G`）。
2. 點擊藍色的 **Publish to GitHub**（發布到 GitHub）按鈕。
3. 依提示選擇：
   - **Publish to GitHub public repository**（公開儲存庫，若要使用免費 GitHub Pages 推薦選此項）。
4. VS Code 會自動完成初始化、Commit 並將程式碼推送到你的 GitHub 帳號！

#### 方式 B：使用 VS Code 終端機（Terminal）指令

1. 在 VS Code 中按下 `Ctrl + ~`（或點選選單 **終端機 ➔ 新增終端機**）。
2. 依序貼上並執行以下指令（請將 `你的GitHub帳號` 與 `你的儲存庫名稱` 替換為實際名稱）：

```bash
# 初始化 Git 儲存庫
git init

# 加入所有檔案
git add .

# 建立初次提交
git commit -m "feat: initial release of PocketPet with Supabase sync"

# 設定主分支為 main
git branch -M main

# 關聯你的 GitHub 遠端儲存庫
git remote add origin https://github.com/你的GitHub帳號/你的儲存庫名稱.git

# 推送至 GitHub
git push -u origin main
```

---

### 步驟 3：開啟 GitHub Pages（免費用手機隨開隨用）

把專案推上 GitHub 後，可以免費開啟靜態網頁託管，手機打開就能記帳：

1. 開啟瀏覽器進入該 GitHub Repository 頁面。
2. 點選上方頁籤 **Settings** ➔ 左側欄選擇 **Pages**。
3. 在 **Build and deployment** 下方的 **Branch**：
   - 分支選擇 **`main`**。
   - 資料夾選擇 **`/ (root)`**。
   - 點擊 **Save**。
4. 等待約 1 分鐘後重新整理頁面，上方會出現專屬網址：
   `https://<你的帳號>.github.io/<你的儲存庫名稱>/`
5. 用 iPhone Safari 點擊「分享 ➔ 加入主畫面」或 Android Chrome 點擊「安裝應用程式 / 新增至主螢幕」，即可擁有 App 級無邊框體驗！

---

## 🗄️ Supabase 雲端資料庫建置指南

本專案使用原生 `fetch()` 透過 PostgREST API 直接與 Supabase 連線，無需安裝任何龐大套件。

### 1. 執行建立資料表 SQL

進入 [Supabase Dashboard](https://www.google.com/search?q=https%3A%2F%2Fsupabase.com%2Fdashboard%2F) 進入你的專案，點選左側選單的 **SQL Editor** ➔ 點擊 **New Query**，複製並執行以下語法：

```sql
-- 1. 建立小窩狀態資料表 (儲存等級、家具擺設、自訂分類)
create table if not exists public.pocket_pet_profile (
  user_key text primary key,
  state_json jsonb not null,
  updated_at timestamptz default now()
);

-- 2. 建立消費明細資料表 (儲存每一筆開銷紀錄)
create table if not exists public.pocket_pet_expenses (
  id bigint primary key,
  user_key text default 'my_ledger',
  expense_date text,
  cat_id text,
  cat_name text,
  note text,
  amount int,
  ts text,
  updated_at timestamptz default now()
);

-- 3. 開放公開金鑰存取 (若未使用 Supabase Auth 登入時必填)
alter table public.pocket_pet_profile disable row level security;
alter table public.pocket_pet_expenses disable row level security;
```

點擊綠色 **Run** 執行完成後，右上角顯示 `Success` 即代表雲端資料表建立完畢！

### 2. 連線金鑰設定

在 `index.html` 中已內建好連線設定：
- **Project URL**: `https://<你的專案ID>.supabase.co`
- **anon key**: 你的 Publishable Key
- **User Key**: `my_ledger`（在不同裝置填寫相同代號即可共享帳本與房間）

亦可於網頁右上角點擊 **☁️ 連後台 / 已連線** 按鈕隨時開啟設定面板檢查連線狀態或進行手動備份／復原。

---

## 📂 專案檔案結構

```text
pocket-pet/
├── index.html        # 單一自包含完整 Web App (包含 UI、手繪 SVG、邏輯與 Supabase 同步)
└── README.md         # 專案說明與部署指南文件
```

---

## 🛠️ 技術規格

- **架構**：Vanilla HTML5 / Modern CSS / Vanilla JavaScript（單一檔案、零依賴、載入 0 毫秒延遲）。
- **視覺**：手繪墨線風向量 SVG、Isometric 3D 等角投影房間視角、Safe-Area 沉浸式流動版面。
- **後端**：Supabase (PostgreSQL + PostgREST REST API)。
- **同步機制**：樂觀更新（Optimistic UI）+ 背景衝突合併（Upsert）。

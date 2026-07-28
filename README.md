# Cathay_Pacific_20210813

> **About（GitHub 儲存庫簡介建議）**  
> Android Kotlin 練習專案：呼叫 GitHub API 列出使用者，並顯示個人檔案詳情。

## 這個專案在做什麼？

這是一個 **Android 原生 App**（Kotlin），應用名稱為 **GitHud Users**。  
專案以 MVC 風格組織程式，透過 [GitHub REST API](https://docs.github.com/en/rest) 取得使用者資料，並以列表與個人頁呈現。

依程式與命名推斷，這份程式多半是 **2021/08/13 前後的國泰航空（Cathay Pacific）相關技術練習／面試作業**。

### 主要功能

1. **使用者列表（`MainActivity`）**
   - 啟動後呼叫 `GET https://api.github.com/users`
   - 以 `ListView` 顯示頭像（圓形）與 `login` 帳號名稱
   - 列表最多顯示約 100 筆；不足 100 筆時會多一列分隔線

2. **使用者詳情（`OneUserActivity`）**
   - 點擊列表項目後開啟詳情頁
   - 呼叫 `GET https://api.github.com/users/{login}`
   - 顯示圓形頭像、名稱（`name`）、帳號（`login`）、地區（`location`）、網站／部落格（`blog`）
   - 左上角關閉圖示可返回上一頁

## 技術架構

```
app/src/main/java/com/example/cathay_pacific_20210813/
├── Controller/
│   ├── Activity/
│   │   ├── MainActivity.kt      # 多人列表頁
│   │   └── OneUserActivity.kt   # 單人詳情頁
│   └── Adapter/
│       └── UsersListAdapter.kt  # ListView Adapter
└── Model/
    ├── Data/
    │   ├── Users.kt             # 列表 API 回應模型
    │   └── User.kt              # 單人 API 回應模型
    └── Uill/
        └── CircleTransform.kt   # Picasso 圓形頭像轉換
```

| 項目 | 說明 |
|------|------|
| 語言 | Kotlin |
| 最低 SDK | 16 |
| 目標／編譯 SDK | 30 |
| 網路 | OkHttp 3 |
| JSON | Gson |
| 圖片 | Picasso + 自訂 `CircleTransform` |
| UI | AndroidX、AppCompat、Material、ListView |

## 畫面與流程

```
啟動 App
   │
   ▼
MainActivity
  └─ GET /users → ListView（頭像 + login）
                    │
                    │ 點擊使用者
                    ▼
              OneUserActivity
                └─ GET /users/{login}
                   顯示頭像、name、login、location、blog
```

## 環境需求

- Android Studio（建議 Arctic Fox 或之後可開啟 Gradle 專案的版本）
- JDK 8
- Android SDK（compileSdk 30）
- 可連線網際網路的裝置或模擬器（需存取 `api.github.com`）

## 建置與執行

1. 用 Android Studio 開啟本專案根目錄
2. 等待 Gradle Sync 完成
3. 選擇模擬器或實體裝置，按下 Run

命令列（需已安裝 Android SDK）：

```bash
./gradlew assembleDebug
```

產生的 APK 位於：

```text
app/build/outputs/apk/debug/app-debug.apk
```

## 權限

`AndroidManifest.xml` 宣告：

- `android.permission.INTERNET` — 呼叫 GitHub API、載入頭像圖片

## 備註

- 應用字串資源中的名稱為 `GitHud Users`（拼字與 GitHub 不同）。
- GitHub API 未認證時有速率限制；頻繁測試可能暫時被拒。
- 專案使用較舊的 Android Gradle Plugin（4.2.1）與依賴，若用新版 Android Studio 開啟，可能需要調整 Gradle／JDK 設定。

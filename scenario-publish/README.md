# Speak AI — 線上預設情境庫

App 會從這個 repo 下載預設情境（含背景圖）。本 repo 的內容就是 App 的「官方情境來源」。

## 檔案結構

```
├── manifest.json      # 版本資訊（App 每天檢查一次這個檔）
├── scenarios.json     # 全部情境的完整內容
├── images/            # 情境背景圖，檔名 = 情境 ID.jpg
└── README.md
```

- `manifest.json`：小檔案，App 只比對 `version` 是否改變，有變才下載 `scenarios.json`。
- `scenarios.json`：情境本體。`imageUrl` 是相對路徑，App 會自行加上下載基底網址。
- 圖片：一律放 `images/{情境ID}.jpg`，解析度與比例不拘（建議直式 9:16、1K）。

## 情境 ID 規則

格式：`scn_{yyyyMMdd}_{seq}`，seq 為三碼流水號，例如 `scn_20260920_001`。

**ID 是版本單位，一旦發布就不可改**：

| 操作 | 做法 |
|------|------|
| 新增情境 | 用新的 ID（當天日期 + 新流水號）加進 `scenarios.json`，圖片放 `images/` |
| 修改情境內容 | **換一個新 ID**（新內容 + 新圖），並把舊 ID 從 JSON 移除。App 端會自動「下載新的、刪掉舊的」 |
| 刪除情境 | 直接把該 ID 從 `scenarios.json` 移除（圖片檔也可一併刪）。App 端會自動刪除本機對應情境與圖片 |

> 原因：App 的同步規則是「本機已有的 ID 不覆蓋」，所以改內容必須靠換 ID 才會推送到使用者。

## 每次更新後必做

1. 修改 `scenarios.json`（或增刪圖片）。
2. 把 `manifest.json` 的 `version` 加 1（兩個檔案的 `version` 保持一致）。
3. Commit + push。

> App 端下載 `scenarios.json` 後會做嚴格解析與欄位驗證，失敗就保留本機舊版，不需要額外的雜湊或簽章欄位。

## 欄位說明（scenarios.json）

| 欄位 | 說明 |
|------|------|
| `id` | `scn_{yyyyMMdd}_{seq}`，唯一、不可變 |
| `title` | 情境標題 |
| `sceneDescription` | 情境描述（可中文，給使用者看） |
| `aiRoleName` / `aiRolePrompt` | AI 扮演的角色名與系統提示詞（英文） |
| `userRoleName` / `userRolePrompt` | 使用者扮演的角色名與提示詞 |
| `characterId` | 虛擬角色：`rita` / `emma` / `zoe` / `chloe` |
| `voiceGender` | `female` / `male` |
| `voiceId` | 指定聲音 ID，通常留 `null` |
| `defaultDifficulty` | `A1` / `A2` / `B1` / `B2` / `C1` |
| `turnLimit` | 對話輪數上限 |
| `learningGoals` | 學習目標陣列，可為空 `[]` |
| `imageUrl` | 圖片相對路徑，如 `images/scn_20260920_001.jpg` |
| `imagePrompt` | 產圖用的 prompt（留作記錄，App 不使用） |
| `isPreset` | 固定 `true`（App 用來區隔使用者自建情境） |

# PSJGAS 上線與 PSJGA_CORE 安裝

## GitHub Pages

本儲存庫由 `main` 分支的根目錄發布。

新版是 `index.html`；舊稽核頁保留為 `legacy.html`，與發布前的 `index.html` 完全相同（Git blob `fa747d5a42ab5f75d2e9b7145bd2141359aac6a7`）。需要回復首頁時，可在 GitHub 還原發布提交。

## 安裝新 Apps Script

1. 用管理總務資料的 Google 帳號到 [Apps Script](https://script.google.com/) 建立**獨立專案**，命名 **PSJGA_CORE**。把 `PSJGA_CORE.gs` 全部貼到預設的 `Code.gs`，儲存。只貼這個檔。選函式 **`setupPSJGA` → 執行**，接受 Google Drive 授權。這一步建立新的私人 `PSJGA_DATA` 資料夾與存取碼；不動原有的 GS、試算表或 Bot。
2. 在左側 **專案設定 → 指令碼屬性**，保留自動建立的 `PSJGA_ACCESS_KEY`。新增 `TELEGRAM_BOT_TOKEN`（填現有 @psjga_bot 的 Token）與 `TELEGRAM_CHAT_ID`（填現有 PSJGA 群組 ID）。Token 留在這裡即可。只向有權讀寫總表及發摘要的人提供存取碼。
3. 選 **部署 → 新增部署 → 網頁應用程式**，設定「執行身分：我」、「誰可以存取：任何人」，部署後複製結尾為 **`/exec`** 的網址。這樣 GitHub Pages 可呼叫 GS；沒有正確存取碼，API 不提供紀錄或寫入。若 Google Workspace 管理規則不允許「任何人」，先請管理員確認部署方式。
4. 到新版首頁 → **雲端設定**，貼入 `/exec` 網址與 `PSJGA_ACCESS_KEY`，按 **驗證並下載**。第一次可能顯示版本 0，表示新總表還沒有紀錄。按 **測試 Bot** 會查名稱與目標群組，**不發任何訊息**。也可以在 GS 編輯器單獨執行 `checkPSJGATelegram`。若日後更新程式碼，在「管理部署」編輯原部署並選新版本，可保留原 `/exec`。

## 啟用 Telegram 群組指令

1. 新版首頁可公開開啟、這一版 GS 已部署後，在 GS 編輯器執行 **`setupPSJGACommand`**。它會核對 Bot、群組、首頁與接收網址，然後註冊 Telegram 的指令接收網址。安裝本身不發訊息。
2. 回到 **PSJGA 群組**輸入 **`/psjga`**，Bot 應回覆 **「開啟總務平台」**按鈕。若群組內有其他 Bot 或 Telegram 未將一般指令交給它，請輸入 **`/psjga@psjga_bot`**。兩種寫法都由同一段程式處理。
3. **`checkPSJGACommand`** 可查看接收網址是否生效，以及最後一次指令回覆狀態。如果顯示 `TG_WEBHOOK_IN_USE`，代表 Bot 已有其他程式接收指令，需將新指令加到原本的程式，保留它現有的設定。若已知其他程式使用 `getUpdates` 接收，也應整合到原程式。若出現 `TG_CALLBACK_UNREACHABLE`，確認使用目前程式碼重新部署網頁應用程式，且匿名 POST 能直接取得回應。

## 日常操作

- 「資料與連線 → 選取 JSON」匯入符合格式的快照，核對後按「上傳」。每份最多 5,000 筆、4 MiB，照片保留 HTTPS 連結；範例資料不可上傳。匯入失敗會保留原有資料。「匯出總表 JSON」可完整備份含刪除標記；報表的「匯出」是所選範圍的 CSV。匯入後若還沒上傳，先匯出備份再關分頁。
- 「下載」核對雲端版本；若其他人先更新，先下載再上傳。同一內容不會重寫。雲端資料在新 GS 所有者的私人 Drive 中；存取碼和目前資料只留在工作台分頁，瀏覽器只記住部署網址。
- 「發摘要」用目前所選期間的雲端總表，可選單一模組。先預覽，再確認發往查到的群組。已確認發過的相同摘要不再發送；送出後回應不明時先到群組查核。各模組的原始照片、明細、待核可按鈕仍由原頁發送。
- SEC、GasCheck、GASExp 原資料庫繼續透過原模組使用。PSJGA_CORE 是新總表；尚未把三個原資料庫的歷史紀錄自動串入，也不會寫回原資料庫。回復舊入口可直接開 `legacy.html`。

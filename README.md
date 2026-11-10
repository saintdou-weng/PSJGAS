# VRT 總務工作台 · PSJGAS

側欄依門禁與安全、環境與巡檢、宿舍與資產、採購與費用分組，單擊即可開啟 **SEC、GasCheck、GASExp 的 23 個業務頁**。每頁上方可在「原系統作業」與「原表查詢」依期間從 SEC、GasCheck、GASExp 直接讀取原系統紀錄，保留原單號、資料分類及原作業入口。貨櫃的車輛出入、貨櫃檢查與放行單分開計算；通勤的汽車、機車、門禁與遲到分列。人員／設備主檔與巡邏原始打點等仍於原系統查看，不重複算為業務事件。

工作台只查詢原表，不另建紀錄。新增、編輯、刪除、上傳、匯入與核可均在原系統操作。已配對裝置沿用原授權；同期間資料在開啟期間重用，手動「更新資料」優先檢查版本。資料讀取不完整會標示來源狀態，部分模組可先查看此裝置的原系統紀錄。這次不改三套原系統及既有 PSJGA_CORE 部署。

`index.html` 是可直接開啟的完整首頁，無前端相依套件、外部字型或圖表 CDN。正式畫面只顯示原系統資料。

部署 `PSJGA_CORE` 並啟用群組指令後，在 PSJGA 群組輸入 `/psjga`，@psjga_bot 會回覆「開啟總務平台」按鈕。如果 Telegram 未把一般指令交給這個 Bot，使用 `/psjga@psjga_bot`。

- [安裝 PSJGA_CORE、連接現有 Bot、更新 GitHub Pages](INSTALL_CN.md)
- [原功能盤點與資料格式](ARCHITECTURE_CN.md)（v1.0 設計紀錄）
- [測試結果](TEST_RESULTS.txt)

本儲存庫保存可直接發布的成品。v1.3 沿用既有 Apps Script 部署與手機授權。


## 修改 index.html 後必做（2026-11-11）

`index.html` 的 `<meta http-equiv="Content-Security-Policy">` 以 sha256 鎖定頁內腳本。
只要改了 `<script type="module">` 裡任何一個字，就必須重新計算雜湊並更新 `script-src`，
否則瀏覽器會擋下整段腳本，頁面變成空白。計算方式：對 `<script type="module">` 與 `</script>` 之間的原文做 SHA-256 後 base64。

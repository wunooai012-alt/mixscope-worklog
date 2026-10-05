# MIXSCOPE 工作日誌

更新日期：2026-10-05（台灣時間）

這裡記錄實際成果、驗收證據、限制與下一步。每次提交可從 History 查看，不把規格或介面當作已驗證功能。

## 下載最新版 1.5.0 測試版

**Code → Download ZIP 現在包含程式。** 解壓 `mixscope-worklog-main.zip` 後，再解壓其中的 `MIXSCOPE-latest.zip`，即可取得 `MIXSCOPE.app` 與使用說明。關閉舊版，再將 App 放進 Applications 並替換。

也可[直接下載最新版 App ZIP](https://github.com/wunooai012-alt/mixscope-worklog/raw/refs/heads/main/MIXSCOPE-latest.zip)，省去外層解壓。適用 macOS 13+、Apple Silicon 與 Intel；[版本、SHA256 與安裝說明](LATEST.txt)。每次驗收交付新版本，都會同步替換這個固定檔名並更新本頁。

**1.5.0 新增**：即時 BPM／Key 實驗估測，8 秒暖機及最近約 30 秒窗口；靜音／訊號中斷會清除舊估測，手動新歌可保留響度與 Reference。修正立體聲假音高、指定高頻混疊及 Demo 節拍誤判。自動啟動／喚醒在未授權時不主動觸發授權。

**仍待驗收**：最終原生 GUI、MacBook Air M4／Intel 實機、真實歌曲準確率、正式 Apple 簽章／公證、更新後保留授權及 App 內自動更新。此包仍是 ad-hoc 測試版；不要將合成測試或免登入下載視為上述項目已完成。自動換歌尚未提供。

工作日誌與下載均已公開，無需登入。[歷史 Releases](https://github.com/wunooai012-alt/mixscope-worklog/releases) 保留先前交付版本。

## 已驗證與限制

|項目|證據|狀態／限制|
|---|---|---|
|LUFS／RMS／LRA|來源重建測試涵蓋44.1／48／96 kHz、mono、靜音、反相、削波、合成LRA|合成測試通過；非完整官方認證|
|True Peak／RTA／聲像|intersample、L/R、反相、52／68頻帶能量與Hold／Memory測試|8x／64-tap TP插值估計，非認證量測|
|生命周期1.3.1|受控延遲重現舊reset回呼覆蓋新session，修正後獨立重建PASS|來源切換、取消與Reference測試通過|
|效能1.3.2|60分鐘音訊加速基準RSS採樣峰值322→29 MiB；pending main 150→1|非一小時實時視窗／系統擷取；樣本數與LUFS／TP一致|
|來源污染修正|有效session後開啟缺檔／拒絕多聲道不再繼承舊DSP；獨立重建PASS|Reference保留，取消取得報告單次約2 ms，非上界承諾|
|UI／Reference|Demo、停止、Freeze狀態、保存Reference；開發者實測離線匯入、JSON匯出／載入与無效schema|開發者已觀察SYSTEM AUDIO時間及讀值變動；成功擷取初步有證據，音效卡／重連／M4實機待完整驗證|
|BPM／Key 1.4.0|MusicalAnalysisTests／MusicalSessionTests 監督獨立重建 PASS；涵蓋3種取樣率、90／120／150 BPM、C／D大調、A小調、反相、靜音／噪音／短檔與分塊一致|合成測試；非真實歌曲準確率。tempo取最後180秒，key彙整全曲；信心是啟發式分數|
|新歌曲 1.4.0|換檔、缺檔、取消、Reference保留、可選響度重置及舊JSON相容測試通過|即時BPM／Key與自動換歌尚未完成|
|封裝|1.4.0 ZIP CRC與9項來源hash吻合；GitHub附件digest相同|本機ad-hoc簽章；未Apple公證；M4／Intel實機待驗|

history超過6,000點會降採樣，peak為累計sample最大值，不能用於完整瞬間超限定位。整段LUFS／LRA／MAX獨立累計。

## 階段日誌

- **1.3**：整合Sparkle更新設定；尚未完成遠端端到端更新。
- **1.3.1**：修正reset／stop／換檔的舊回呼；交付本機測試ZIP。
- **1.3.2**：合併顯示快照降低記憶體與回呼積壓；修正失敗新檔誤用上一首數據；交付本機測試ZIP。
- **1.4.0**：交付離線 BPM／Key、信心／候選及手動新歌曲；合成核心與 session 測試獨立重建通過。
- **1.5.0**：交付即時暖機與30秒窗口、訊號中斷及手動歌曲邊界、拒絕授權後不自動提示，以及簽署工具修正。
- **目前開發**：真實歌曲、最終原生GUI／實機與長時間效能，以及固定更新feed與跨機更新仍待驗證。

## 下一階段與驗收條件

1. **更新發布**：維持既有公鑰；驗feed／ZIP簽章與竄改拒絕；另一台實測檢查、下載、安裝、重啟及版本。repository已公開；免登入更新feed及跨機更新仍待完成。
2. **離線BPM／Key可靠性**：擴充速度邊界、弱節奏／倍半拍、和弦／關係大小調、混疊及立體聲不同訊號；合法已知歌曲須記錄來源、標籤與誤差。比較1.3.2與新增分析的耗時／RSS。
3. **即時換歌**：暖機、保守狀態機與去抖；覆蓋pause、drop及同曲段落誤判；響度是否跟隨重置由明確選項控制。
4. **批次分析與報告**：可取消、逐檔錯誤隔離、CSV／JSON，接續Reference／自訂目標及獨立超限區段定位。

每輪限定可驗收範圍，完成後交付版本ZIP。低風險可逆工作直接執行；權限限制不阻擋其他開發。

## 2026-10-05 13:34 監督追蹤

- 監督從 UpdateSignatureTests.swift 獨立重建測試：正式 1.3.2 ZIP 簽章接受、改動位元與錯誤公鑰拒絕，PASS。這不代表固定 feed 或跨機更新已完成。
- 1.3.2 GUI：開發者紀錄 SYSTEM AUDIO 48kHz stereo，07:30→07:45且讀值變動；未重新啟動／跨機驗收。
- 開發任務閒置，尚無BPM／Key模組；已立即交辦離線估測核心、信心與歧義、換檔重置／Reference保留及回歸測試，發布等待期間繼續實作。
- 圖示需求已確認為MIXSCOPE蒼蠅：改小、呈飛行姿態，排在核心功能之後。


## 2026-10-05 14:04 監督驗收

- 已獨立編譯並執行 MusicalAnalysisTests、MusicalSessionTests，均 PASS；交付1.4.0私人預發布。
- ZIP SHA256：`2f5358b8257ea08b2d4760b6b53b07e7c6346123e93b4f574295325c5401d659`；GitHub附件與本機一致。
- 待驗：真實歌曲、完整抗混疊、每sample選強聲道造成非線性影響、相對調歧義與性能回歸；不得把信心當準確率。
- 1.4.0開發者UI驗過120 BPM／C大調離線fixture及手動新歌曲；本輪系統音訊TCC未完成，前版成功擷取證據不等於本版完整驗收。
- 已交辦上述可靠性／效能修正，再做即時暖機；正在開發時不重複催辦或同檔修改。
- 此ZIP含封裝時的小飛行蒼蠅；之後的一般翅膀修訂留待下一包。


## 2026-10-05 公開查看與下載

依使用者要求，工作日誌與既有測試版 Releases 已改為公開。可直接分享日誌／下載連結，無需 GitHub 帳號。GitHub 公開下載不代表 App 內自動更新已完成。

## 2026-10-05 1.5.0 最新程式納入 Code 下載

- 根目錄固定提供 `MIXSCOPE-latest.zip` 與 `LATEST.txt`，每輪驗收交付同步替換；下載整個 repository 也會包含可執行 App 的完整 ZIP。
- 監督核對最終來源快照 44 項 SHA256、ZIP CRC、解壓後 codesign deep strict、1.5.0/build150 與 arm64／x86_64，均通過。
- 監督由最終來源獨立重建 RealtimeMusicalTests、DemoTempoTests，均 PASS：暖機、靜音窗口退出、訊號中斷與手動邊界；無節奏 Demo 在8／15／30／60秒不產生假BPM。
- 授權模型測試先前獨立通過；跨版本ad-hoc身分不同已確認，正式Apple簽章仍缺憑證。簽章檢查工具的DR字串解析問題已修正，自身正控制通過。
- 30秒model節奏性能為開發者證據：1,444,800 frames、最多1個待發布快照；不含SwiftUI或實際硬體擷取，不能冒充長時間實機驗收。
- 最終原生GUI啟動受自動審核限制，尚未操作；本包以明確未驗事項的測試版交付。

# greekAudio

[English](README.md) | [繁體中文](README.zh-TW.md)

適用於 Termux 的終端機介面（TUI）程式，用來查詢聖經希臘文單字並播放發音。可依希臘文、發音拼音、轉寫或英文釋義搜尋，以鍵盤或觸控選字後，程式會用 yt-dlp 下載音檔、存入本機快取，並以 `termux-media-player` 播放。


## 功能特色

- 收錄 4,599 筆資料（4,598 個希臘文單字，外加希臘字母歌）
- 輸入的內容會同時比對下列欄位：
  - 發音拼音（不分大小寫，忽略長音符號）
  - 希臘文（忽略重音、氣符與字尾 sigma）
  - 希臘文逐字母轉寫（例如 `χ` 轉為 `ch`、`φ` 轉為 `ph`）
  - 希臘文鍵盤對應轉寫（例如 `χ` 對應 `x`、`η` 對應 `h`、`ς` 對應 `w`）
  - 英文釋義
- 輸入多個詞時，每個詞都必須符合；結果依完全相符、字首相符、單字字首相符、部分相符排序
- 符合結果少於 10 筆且輸入至少 3 個字元時，會以模糊配對補上，容忍拼錯
- 不限制結果筆數
- 支援鍵盤與觸控：點擊結果列即可播放，可用滾輪捲動
- 本機快取：已快取的單字以 `*` 標示，再次播放時不需重新下載
- 透過 `termux-media-player` 在背景播放
- 按 `Ctrl+D` 並確認 Y/n 後可清除快取
- 單一 `.py` 檔：單字清單以壓縮形式內嵌，第一次啟動時解壓為 `.greek.csv`
- 只使用 Python 標準函式庫


## 技術堆疊

**語言：** Python 3.7 以上（僅標準函式庫）  
**下載：** yt-dlp  
**播放：** Termux:API（`termux-media-player`）


## 安裝

安裝 Termux 套件：

```bash
pkg install python termux-api
```

另外需安裝 Termux:API 的 Android 應用程式，否則 `termux-media-player` 無法運作。

安裝 yt-dlp：

```bash
pip install -r requirements.txt
```


## 使用方式

啟動程式：

```bash
python greekAudio.py
```

若要使用其他單字清單，可在指令後接 CSV 路徑（必要欄位見附錄）：

```bash
python greekAudio.py words.csv
```

### 操作

| 按鍵或動作 | 效果 |
| --- | --- |
| 輸入文字 | 即時過濾結果 |
| `Enter` 或點擊結果列 | 必要時先下載，然後播放 |
| `Up`、`Down`、`Ctrl+P`、`Ctrl+N` | 移動選取 |
| `PgUp`、`PgDn`、`Home`、`End` | 在清單中跳轉 |
| `Ctrl+U` | 清除輸入 |
| `Ctrl+D` | 清除快取（會詢問 Y/n） |
| `Esc`、`Ctrl+C` | 離開 |

### 搜尋範例

| 輸入 | 比對方式 |
| --- | --- |
| `biblos` | 發音拼音 |
| `βιβλ` | 希臘文 |
| `xaris` | `χάρις` 的希臘文鍵盤對應轉寫 |
| `grace` | 英文釋義 |

### 程式產生的檔案

| 路徑 | 說明 |
| --- | --- |
| `.greek.csv` | 第一次啟動時解壓出的單字清單（刪除後會重新產生） |
| `.cache/` | 已下載的音檔，檔名格式為 `romanization_videoId.ext` |


## 常見問題

#### 如何停止播放？

執行 `termux-media-player stop`。播放另一個單字時，也會自動停止前一個。

#### 下載或播放失敗

請以 `pip install -U yt-dlp` 更新 yt-dlp，並確認已安裝 `termux-api` 套件與 Termux:API 應用程式。

#### 為什麼不轉換音檔格式？

音檔以下載時的格式直接播放（優先取 m4a），因此不需要安裝 ffmpeg。


## 致謝

 - [yt-dlp](https://github.com/yt-dlp/yt-dlp)
 - 發音影片來自 YouTube 上 Logos Bible Software 的頻道。本專案不內附任何音檔，音檔皆於使用時才下載。


## 附錄

單字清單為 UTF-8 編碼的 CSV，含標題列，欄位如下：

| 欄位 | 說明 |
| --- | --- |
| `url` | 發音影片的 YouTube 觀看網址 |
| `romanization` | 單字的發音拼音 |
| `greek` | 希臘文拼寫（希臘字母歌此欄為空） |
| `gloss` | 英文釋義 |


## 下載連結

- Termux:API APK 下載：[https://github.com/termux/termux-api/releases/download/v0.53.0/termux-api-app_v0.53.0+github.debug.apk](https://github.com/termux/termux-api/releases/download/v0.53.0/termux-api-app_v0.53.0+github.debug.apk)

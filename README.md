# 我們的地方

一個只屬於你們這群人的小空間：要邀請碼才能進來，名稱、顏色、群規、每日儀式都由你們自己決定。

這是**模板**。每一群人各自複製一份，資料存在你們自己的資料庫，模板作者看不到、也不經手任何內容。

## 有什麼

- **聊天**：一個共用的聊天室。
- **今天**：每日一個話題，加上一個每日儀式按鈕（例如「點一盞燈」），看得到今天有誰來過。
- **語錄**：聊天裡說得好的話，點一下就能收進語錄。
- **管理**：同意或移除成員、刪除訊息、修改空間設定、更換邀請碼。

## 架設前準備

- 一個 GitHub 帳號、一個 Supabase 帳號（都免費，可用 GitHub 登入 Supabase）。
- 約 20～30 分鐘。用電腦操作最順；手機也可以，但請看每一步的「手機注意」。

---

## 第 1 步：複製這個模板

1. 在這個 repo 頁面按綠色的 **Use this template → Create a new repository**。
2. 名稱自取（例如 `our-space`），選 **Public**，按 **Create repository**。

## 第 2 步：建立資料庫

1. 到 [supabase.com](https://supabase.com) 登入，按 **New project**。
   - 名稱自取；**Database Password** 按 Generate 並另外存起來；
   - **Region** 選 **Northeast Asia (Tokyo)**；方案選 **Free**。
2. 建好後，打開這個網址開一個空白的 SQL 頁面（把 `你的專案代號` 換成網址中 `https://` 後面那串英文）：
   `https://supabase.com/dashboard/project/你的專案代號/sql/new`
3. 在你的 GitHub repo 打開 `schema.sql`，按右上角 **Copy raw file**，貼進 SQL 頁面，按 **Run**。
   - 跳出「Make sure you are not accidentally removing something important」時，按紅色 **Run**。這是正常的，那段是在收回不必要的權限。
   - 成功會顯示 `Success. No rows returned`。
   > **手機注意**：手機版編輯器貼上時可能自動在最後多補一個 `)`。若出現 `syntax error at or near ")"`，捲到最底，把最後一行 `end $$;` 之後多出來的字元刪掉，再 Run 一次。報錯時什麼都不會執行，可以放心重來。
4. 再開一個新的空白 SQL 頁面（同上網址），貼上這一行，**換成你們自己想的兩個碼**，按 Run：
   ```sql
   select setup_space('邀請碼', '管理碼');
   ```
   - **邀請碼**（至少 6 字）：給成員用。可以是好記的中文詞加數字。
   - **管理碼**（至少 8 字）：用它加入的人會成為管理者。請用長而隨機的組合，**只給自己**，不要截圖分享。
   - 不要用任何範例上看到的字串，也不要用 `000000`、`12345678` 這類容易猜的碼。
   - 看到「設定完成」即成功。之後想換碼，再執行一次同一行即可。

## 第 3 步：取得連線資料

在 Supabase 專案首頁，按網址旁的 **Copy**，複製兩樣：
- **Project URL**（`https://xxxx.supabase.co`）
- **Publishable key**（`sb_publishable_` 開頭；舊專案則是 `eyJ` 開頭的 anon key）

> 不要用 **Direct connection string**、`secret` 或 `service_role` key，那些不能放進網頁。

## 第 4 步：填入 config.js

在你的 GitHub repo 打開 `config.js`，按鉛筆編輯，**只改引號裡面**，改完應該長這樣：

```js
window.SPACE_CONFIG = {
  SUPABASE_URL: "https://xxxx.supabase.co",
  SUPABASE_ANON_KEY: "sb_publishable_完整一串"
};
```

- 兩個值都要在雙引號裡；金鑰要**完整一串在同一行**，右引號不能跑到下一行（複製時常帶到換行）。
- 不要在 `};` 後面另外加行。

按 **Commit changes**。

## 第 5 步：開啟網頁

1. repo 的 **Settings → Pages**，Source 選 `Deploy from a branch`，Branch 選 `main`、`/(root)`，按 **Save**。
2. 到 **Actions** 分頁等「pages build and deployment」出現綠色勾勾（約 1 分鐘）。
3. 打開 `https://你的帳號.github.io/repo名稱/`，暱稱隨意，邀請碼欄填**管理碼**，你就是管理者。
4. 到「管理」分頁設定名稱、主題色、入口那句話、群規和儀式，再把網址和**邀請碼**傳給成員。

> **手機注意**：用手機上傳檔案時，檔名可能被改成 `index-1.html` 之類。網站只認 `index.html`，看到被改名就到檔案的編輯畫面把名字改回來。

## 加到手機桌面

成員在「我」分頁按 **加到手機桌面**，桌面就會出現圖示，點開像 App 一樣全螢幕。
- Android（Chrome）會直接跳出安裝視窗；iPhone 要用 Safari，從「分享 → 加入主畫面」。
- 在 LINE 或臉書裡打開的網頁無法加入，要先「用瀏覽器開啟」。
- 想讓桌面圖示顯示你們空間的名字：編輯 `manifest.webmanifest`，把 `name` 和 `short_name` 改成你們的名稱（`short_name` 建議 6 個字以內）。

## 常見問題

| 看到的狀況 | 原因與處理 |
|---|---|
| 「還差一步」 | config.js 沒填好或格式錯（多半是右引號換到下一行）。改好 commit 後重新整理。 |
| 404 | 網站還在建置，等 1～2 分鐘；或 `index.html` 檔名被改。 |
| 「這個空間還沒設定邀請碼」 | 還沒執行第 2 步的 `setup_space`。 |
| 連線失敗 | Supabase 專案被暫停（免費專案一週沒人用會暫停），到 Supabase 後台按 **Resume project**。 |

## 安全設計

- 資料表完全不對外開放；網頁只能呼叫資料庫裡的函式，每個函式都會先核對成員身分。
- 知道網址、拿到金鑰的人，沒有邀請碼也讀不到任何訊息。
- 邀請碼和管理碼只存雜湊值；被移除的成員立刻失去存取權。

想自己確認權限有沒有設好，可在 SQL 頁面執行：

```sql
select proname, has_function_privilege('anon', oid, 'execute') as 對外
from pg_proc where pronamespace = 'public'::regnamespace order by 2, 1;
```

`_hash`、`_me`、`_today`、`setup_space` 必須是 **false**，其餘為 true。

## 限制

- 身分綁在裝置上，換手機或清除瀏覽器資料要用邀請碼重新加入（會成為新成員）。
- 聊天每 3 秒更新一次，沒有推播通知；只有文字，不能傳圖片。
- 免費 Supabase 專案一週沒有使用會被暫停；同一個帳號最多 2 個運作中的免費專案。

## 責任

每個空間的內容與成員，由該空間的架設者（管理者）負責。模板作者不經手、也看不到任何一個空間的資料。

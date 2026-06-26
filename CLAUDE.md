# CLAUDE.md — StudyBase（阿蔓明道國中復習專班）

> **⚠ 給同時拿到 SELA-Starter-Kit 的 Claude：**
> 這是**已對齊 Kit V1.21.0 的成熟專案**，不是新專案（前身有 V1.0→V1.8 版本歷程）。
>
> 衝突仲裁規則：
> 1. **以本專案 CLAUDE.md + DESIGN.md 為主、Kit 為輔**
> 2. 本專案刻意不對齊 Kit 的部分：
>    - 配色用北歐極簡自訂系統（`#F4F2ED` 暖米底 + 三科 accent），**不換 Kit 預設色** — 已多版使用者驗證
>    - UI 顯示名稱保留中文「阿蔓明道國中復習專班」（英文化只管檔名/資料夾）
>    - 歷史版號 V1.0→V1.8 保留，從 V2.0.0 起嚴格三位
>    - DESIGN.md / COURSE-SYSTEM.md 是本站既有真相來源，不為對齊改寫
>    - `theme-color` 用站底色 `#F4F2ED`（坑 #42，不綁 SELA 橘）
> 3. **不要為對齊 Kit 而動既有設計** — 已驗證的就是事實標準
> 4. 版號規則照 Kit（部署版無後綴；純靜態無 build，不需 -source 備份版）
> 5. **下次完成版本時記得評估 SELA-handoff.md**（鐵律 #0）

---

## 〇、當前狀態

- **版本：** V2.0.0
- **狀態：** 對齊 SELA-Kit + 英文化（StudyBase）+ 補 favicon／品牌歸屬的大升級里程碑
- **一句話定位：** 給明道國中學生阿蔓的數理化複習網站，純靜態、零後端、GitHub Pages 託管
- **英文程式名：** StudyBase（中文俗稱「阿蔓明道國中復習專班」，保留給 UI / 文件）
- **技術棧：** 純 HTML + CSS + 原生 JS（無框架、無 build step）
- **入口點：** `index.html`
- **部署目標：** GitHub Pages（Git Pusher 推 main → repo Settings → Pages 啟用，純靜態 branch-serve）

---

## 一、技術棧決策（為什麼這樣選）

| 選擇 | 替代品 | 選這個的理由 |
|------|--------|------------|
| 純 HTML + CSS + 原生 JS | React / Vue | 無 build step、雙擊就跑、Pages 直接 host、Git Pusher branch-serve 不能跑 build |
| 類型 A 用共用 `css/main.css`、類型 B/C 自帶 inline CSS | 全站共用一份 CSS | 教材頁互動重、自帶 CSS 才能各章獨立 accent，不互相污染 |
| CSS 變數（`--c1`~`--c6` 等） | Tailwind / SCSS | 原生支援、不需編譯 |
| 系統字型 fallback chain | webfont | 免下載、跨平台中文支援好 |

> 互動（測驗、計算器、SVG 圖解）都在前端 JS 完成，不需後端。加 framework = 大改版。

---

## 二、業務對映表（單一真相）

> 改一處要連動哪幾處，看這張表。

| 業務概念 | 程式落點 |
|---------|---------|
| 三科 | `chemistry/` `math/` `physics/` 三個資料夾，各一個 `index.html` 主頁（類型 A）|
| 一個章節 | `<科>/chN.html`（類型 B 教材頁，自帶 CSS + 導覽 JS + 測驗系統）|
| 一份段考解析 | `<科>/examN.html`（類型 C）|
| 章節登錄到主頁 | 該科 `index.html` 的 `.card-stack` 加一張 `.item item-link` 卡片 + 更新 `.stats` 數字 |
| 首頁科目狀態 | `index.html` 對應 `.subject-card` 的 `.subject-status` 徽章 |
| **版本號** | **每頁 footer（9 頁）+ `README.md` + `favicon/site.webmanifest` 不在此（無版本欄）** — 升版要全部同步 |
| favicon / 品牌 | `favicon/`（SELA logo 套組；未來換 app logo 時整組替換、SVG link 改指 app logo）|

> 加章節 = 動「新增 chN.html + 該科 index 卡片 + 該科 index stats + 全站 footer 版號」四處。
> 改設計規格 = 改 `DESIGN.md` + 對應頁面 CSS（DESIGN.md 是規格真相）。

---

## 三、關鍵檔案路徑

| 想改什麼 | 動哪些檔 |
|---------|---------|
| 設計規格（顏色/間距/字重） | `DESIGN.md`（真相）+ 各頁 `:root` / `css/main.css` |
| 建課流程 | `COURSE-SYSTEM.md`（7 步 SOP、章節登錄表）|
| 加新章節 | 新 `<科>/chN.html` + `<科>/index.html` 卡片 + stats + 全站 footer 版號 |
| 首頁科目進度 | `index.html` 的 `.subject-status` |
| favicon / 品牌 | `favicon/` 整組 + 各頁 `<head>` favicon 區塊（相對路徑依層級）|
| 升版號 | 9 頁 footer + `README.md`（grep 驗證，見坑 #5）|

---

## 四、踩過的坑（編號累積，永不重排）

> 三段式：症狀 → 原因 → 做法。

1. **字串比對插入章節卡片靜默失敗**
   - 症狀：用 `str.replace` 找註解錨點（如 `<!-- ════ 總覽 ════ -->`）插卡片，沒插進去也不報錯
   - 原因：錨點註解的等號數量/空白跟程式裡寫的不完全一致，比對不到就靜默略過
   - 做法：插入前先 `grep` 看實際格式；Python 改檔用 `assert old in text` 而非 `if old in text`（呼應 Kit 坑 #43）

2. **章節卡片 HTML 沒閉合就插下一張 → 巢狀錯亂**
   - 症狀：主頁卡片列表排版崩掉、卡片互相包進去
   - 原因：插入的 `.item` 區塊少了結尾 `</a>` / `</div>`，下一張就被吃進來
   - 做法：插入的卡片區塊一定完整閉合再接下一張，插完用瀏覽器目視一次

3. **新章節檔返回鏈帶 `href="#"`**
   - 症狀：教材頁左上「← 返回」點了沒反應
   - 原因：作者手寫新章節時返回鏈常先寫 `href="#"` 佔位
   - 做法：整合時一律改成 `href="index.html"`（在 `<科>/` 內即指向該科主頁）

4. **多層目錄純靜態站，favicon/manifest 相對路徑要隨層級調整**
   - 症狀：根頁 favicon 正常、子目錄頁（`math/`、`chemistry/`）分頁 icon 破圖
   - 原因：GitHub Pages project site 在子路徑（`user.github.io/StudyBase/`），絕對路徑 `/favicon/` 會壞；而「同一份相對路徑」貼到不同層級也會錯
   - 做法：root 頁用 `favicon/...`、子目錄頁用 `../favicon/...`（呼應 Kit 坑 #39，但要按層級給前綴）

5. **版本號散落多檔，升版漏改**
   - 症狀：首頁顯示新版、某教材頁 footer 還是舊版
   - 原因：版號出現在 9 個頁面 footer + README，手改容易漏
   - 做法：升版用腳本一次 `replace`，完工 `grep -rn "V<舊版>"` 確認歸零（呼應 Kit 坑 #14）

---

## 五、煙霧測試（可貼上執行）

```bash
# 1. 本機預覽
cd StudyBase && python -m http.server 8000
# 開 http://localhost:8000 → 預期：首頁載入、分頁有 SELA 橘 favicon、無 Console 錯誤

# 2. 設計規格驗證（DESIGN.md 鐵律：無漸層/陰影/大圓角/字重>500）
python3 - <<'PY'
import re, glob
for f in glob.glob('StudyBase/**/*.html', recursive=True):
    c=open(f,encoding='utf-8').read()
    bad=[]
    if 'gradient' in c: bad.append('gradient')
    if 'box-shadow' in c: bad.append('box-shadow')
    if re.search(r'border-radius:\s*(?!var)([89]|[1-9][0-9]+)px', c): bad.append('大圓角')
    if bad: print(f, bad)
print('規格檢查完成（上面沒列出檔案 = 全過）')
PY

# 3. 版本同步
grep -rn "V2.0.0" StudyBase --include=*.html | wc -l   # 預期 9
```

**手動檢查：**
- [ ] 分頁 icon 顯示 SELA 橘 logo（root + 子目錄頁都要對）
- [ ] 手機加到主畫面 icon 正常
- [ ] 每頁 footer 版本都是 V2.0.0

---

## 六、版本歷程（最近數版）

| 版本 | 重點 |
|------|------|
| V1.5 | 化學第一次段考新增「題型攻略」分頁 |
| V1.6 | 化學第一次段考全面改版 |
| V1.7 | 化學第七章 化學反應加入 |
| V1.8 | 數學第一章 相似形整合（首個數學章節）|
| **V2.0.0** | **對齊 SELA-Kit + 英文化 StudyBase + 補 favicon/品牌歸屬 + 建 CLAUDE.md/handoff（大升級里程碑）** |

> 完整歷程在 README.md。

---

## 七、下版候選工作（按優先序）

1. **套用 app logo** — V2.0.0 已產 `SELA-logo-prompt.md`，Sela 拿去生圖後回來，走 logo §10.2 轉檔 + 多尺寸套組，整組替換 `favicon/`、SVG link 改指 app logo、SELA 降為 README footer 歸屬。這是讓分頁/桌面捷徑能辨識的最後一哩。
2. 數學第二章（接續相似形後的課綱章節）
3. 物理首章（架構已就緒，套同一類型 B 流程）
4. 化學第一～四章（學生覺得較簡單，排在後面）
5. 各科段考解析頁（類型 C）

---

## 八、升版必讀

> 等級判斷：加章節/改教材 = 版本歷程帶過（b 或 c+1）；技術棧切換/架構重組才開本章。

### ⚠ V2.0.0 大升級必讀

V2.0.0 是「對齊 Kit + 英文化」里程碑，動到的是**外殼**不是教材內容：
- 資料夾名 `site-v1.x` → `StudyBase`；zip 改 `StudyBase V<a>.<b>.<c>.zip`（空格、三位、英文）
- 全站補 favicon（目前是 SELA logo 佔位，app logo 生成後替換）
- 版號從這版起嚴格三位（歷史 V1.x 不回頭改）
- 之後加章節/改教材都是 b+1 或 c+1，不再開本章

---

## 九、一句話總結

V2.0.0 把這個跑了 8 個版本的複習站正式對齊 SELA-Kit：英文化成 StudyBase、補上 favicon 與品牌歸屬、建立給下個 Claude 的 CLAUDE.md，配色與教材內容一律保留（已驗證的事實標準）。下版第一優先是把 Sela 生成的 app logo 套進來，讓分頁與桌面捷徑能一眼認出這個站。

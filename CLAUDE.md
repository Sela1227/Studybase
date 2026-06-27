# CLAUDE.md — StudyBase（阿蔓明道國中復習專班 / Dora Study）

> **⚠ 給同時拿到 SELA-Starter-Kit 的 Claude：**
> 這是**已對齊 Kit V1.21.0 的成熟專案**，不是新專案（前身 V1.0→V1.8，V2.0.0 對齊 Kit）。
>
> 衝突仲裁規則：
> 1. **以本專案 CLAUDE.md + DESIGN.md 為主、Kit 為輔**
> 2. 本專案刻意不對齊 Kit 的部分：
>    - **配色：V2.1.0 起為薰衣草紫系**（SELA 主動要求，因應 Dora Study app logo、國中女生向）；DESIGN.md 已同步。不是 Kit 北歐霧藍、也不是 SELA 橘。
>    - UI 顯示名稱保留中文「阿蔓明道國中復習專班」；品牌 wordmark = Dora Study（英文化只管檔名/資料夾＝StudyBase）
>    - 歷史版號 V1.0→V1.8 保留，V2.0.0 起嚴格三位
>    - DESIGN.md / COURSE-SYSTEM.md 是既有真相來源
>    - `theme-color` 用品牌紫 `#634A83`（坑 #42，PWA 啟動色＝品牌色）
> 3. **不要為對齊 Kit 而動既有設計** — 已驗證的就是事實標準
> 4. 版號照 Kit（部署版無後綴；純靜態無 build，不需 -source）
> 5. **下次完成版本時記得評估 SELA-handoff.md**（鐵律 #0）

---

## 〇、當前狀態

- **版本：** V2.1.3
- **狀態：** 程式內雙 logo 入站（hero + footer 署名）
- **一句話定位：** 給明道國中學生阿蔓的數理化複習網站（品牌 Dora Study），純靜態、PWA 可安裝、GitHub Pages 託管
- **英文程式名：** StudyBase（檔名/資料夾）；品牌 wordmark：Dora Study；中文俗稱：阿蔓明道國中復習專班
- **技術棧：** 純 HTML + CSS + 原生 JS + Service Worker（無框架、無 build step）
- **入口點：** `index.html`；PWA：`favicon/site.webmanifest` + `sw.js`
- **部署目標：** GitHub Pages（Git Pusher 推 main → Settings → Pages，branch-serve）

---

## 一、技術棧決策（為什麼這樣選）

| 選擇 | 替代品 | 理由 |
|------|--------|------|
| 純 HTML + CSS + 原生 JS | React / Vue | 無 build、雙擊就跑、Pages branch-serve 不能跑 build |
| 類型 A 共用 `css/main.css`；類型 B/C 自帶 inline CSS | 全站一份 CSS | 教材頁互動重，自帶 CSS 各章獨立 accent 不互污 |
| CSS 變數 | Tailwind/SCSS | 原生、不需編譯 |
| Service Worker（cache-first） | 無 PWA | 離線可用 + 加入主畫面，純靜態最輕做法 |

---

## 二、業務對映表（單一真相）

| 業務概念 | 程式落點 |
|---------|---------|
| 三科 | `chemistry/` `math/` `physics/`，各一 `index.html`（類型 A）|
| 章節 | `<科>/chN.html`（類型 B，自帶 CSS + 導覽 JS + 測驗）|
| 段考解析 | `<科>/examN.html`（類型 C）|
| 章節登錄 | 該科 `index.html` 的 `.card-stack` 加卡片 + `.stats` 數字 |
| **版本號** | **9 頁 footer + `README.md` + `sw.js` 快取名 `studybase-vX.Y.Z`**（升版全部同步、grep 驗證，坑 #5）|
| 配色 | `DESIGN.md`（真相）+ 各頁 `:root` / `css/main.css`（薰衣草系）|
| 品牌 | `favicon/`（Dora Study app logo 套組 + manifest）；SELA logo 退 README footer |
| PWA | `favicon/site.webmanifest`（`start_url`/`scope` 用 `../` 指回站根）+ 站根 `sw.js` + 各頁註冊 |

> 加章節 = 動「chN.html + 該科 index 卡片 + stats + 全站 footer + sw.js 版號」。
> 升版 = 9 footer + README + sw.js 快取名，三處對齊。

---

## 三、關鍵檔案路徑

| 想改什麼 | 動哪些檔 |
|---------|---------|
| 配色 | `DESIGN.md` + 各頁 `:root` / `css/main.css` |
| 加章節 | 新 `chN.html` + 該科 `index.html` 卡片/stats + 全站版號 |
| app logo | `favicon/app-*.png` + `favicon.ico` + `apple-touch` + `android-chrome` + manifest icons |
| PWA 快取策略 | `sw.js`（改了要 bump 快取名才會更新）|
| 升版 | 9 頁 footer + README + `sw.js` 快取名（坑 #5）|

---

## 四、踩過的坑（編號累積，永不重排）

1. **字串比對插入章節卡片靜默失敗** — 錨點註解格式不一致就插不進、不報錯。插入前 grep 實際格式；Python 改檔用 `assert old in text`（呼應 Kit #43）。
2. **章節卡片 HTML 沒閉合就插下一張 → 巢狀錯亂** — 插入區塊完整閉合再接下一張，插完目視一次。
3. **新章節檔返回鏈帶 `href="#"`** — 整合時一律改 `href="index.html"`（在 `<科>/` 內即該科主頁）。
4. **多層目錄純靜態站 favicon 相對路徑要隨層級** — root 頁 `favicon/...`、子目錄頁 `../favicon/...`（呼應 Kit #39，按層級給前綴）。
5. **版本號散落多檔升版漏改** — 9 頁 footer + README + sw.js 快取名。升版腳本一次 replace，完工 `grep -rn "V<舊版>"` 歸零（呼應 Kit #14）。
6. **多層目錄純靜態站做 PWA 的路徑陷阱**
   - 症狀：PWA 從 `favicon/` 開、或子目錄頁 SW 註冊失敗
   - 原因：manifest 放在 `favicon/` 子目錄，`start_url`/`scope` 預設相對 manifest → 指到 `favicon/`；SW 各頁註冊若用同一相對路徑、子目錄頁會找錯
   - 做法：manifest 的 `start_url`/`scope` 用 `../` 指回站根；`sw.js` 放站根、各頁註冊用層級前綴（root: `sw.js` / 子目錄: `../sw.js`）；SW 必須真實同源檔（呼應 Kit #39 #60）

---

## 五、煙霧測試（可貼上執行）

```bash
cd StudyBase && python -m http.server 8000   # 開 http://localhost:8000
# 預期：薰衣草霧紫底、分頁 Dora Study 紫 logo、無 Console 錯誤、Application→Manifest 可安裝

# 設計鐵律（無漸層/陰影/大圓角）
python3 - <<'PY'
import re,glob
for f in glob.glob('StudyBase/**/*.html',recursive=True):
    c=open(f,encoding='utf-8').read()
    b=[x for x in('gradient','box-shadow')if x in c]
    if re.search(r'border-radius:\s*(?!var)([89]|[1-9][0-9]+)px',c): b.append('大圓角')
    if b: print(f,b)
PY

# 版本同步（9）
grep -rn "V2.1.0" StudyBase --include=*.html | wc -l
```

---

## 六、版本歷程（最近數版）

| 版本 | 重點 |
|------|------|
| V1.7 | 化學第七章加入 |
| V1.8 | 數學第一章 相似形整合 |
| V2.0.0 | 對齊 SELA-Kit + 英文化 StudyBase + favicon/品牌（大升級里程碑）|
| V2.1.0 | Dora Study app logo + 全站 PWA + 薰衣草紫配色 |
| V2.1.1 | 程式內雙 logo + 補 rgba 換色漏網 |
| **V2.1.3** | **配色精修：物理去綠改矢車菊藍、內頁 tan/棕→藕紫、綠→薄荷、標頭和諧** |

---

## 七、下版候選工作（按優先序）

1. **數學第二章** — 相似形之後的課綱章節；數學線已有 ch1 類型 B 範式可直接套，是目前最該補的內容缺口。
2. 物理首章（架構已就緒）
3. 化學第一～四章（較簡單，排後面）
4. ch5 設計刷新 — 既有 hover box-shadow + 8~12px 圓角違反 DESIGN.md，未對齊薰衣草前一併收斂
5. 各科段考解析頁（類型 C）

---

## 八、升版必讀

> 加章節/改教材 = 版本歷程帶過（b 或 c+1）；技術棧切換才開本章。

V2.1.0 後續加章節/改教材都是 b+1 或 c+1。app logo 已套（Dora Study）、PWA 已上、配色已轉薰衣草——這三件不需再做。改 `sw.js` 記得 bump 快取名否則使用者吃舊版（坑 #5/#6）。

---

## 九、一句話總結

V2.1.0 把站變成 Dora Study：抽出 Gemini 生的 app logo 做成多尺寸 favicon、全站轉成可安裝 PWA、主色調整個換成薰衣草紫系（國中女生向，綠=對/玫瑰=警告語意保留）。下版第一優先是補數學第二章，把已就緒的類型 B 範式繼續往課綱推。

# StudyBase — 阿蔓明道國中復習專班（Dora Study）

給一位 2026 年 8 月升國三學生的個人化復習網站：數學／化學章節教材＋技能卡學習系統。純靜態 HTML/CSS/JS，無框架，PWA 可離線。

- 線上：https://sela1227.github.io/Studybase/
- 慣例框架：SELA-Starter-Kit V1.21.0（專案內 CLAUDE.md／DESIGN.md 優先於 Kit 預設）

## 目錄結構
```
index.html            三科入口
offline.html          離線提示頁（sw 導向）
css/main.css          首頁樣式
css/lesson.css        v4 章節模板共用樣式（新頁引用；既有頁內嵌）
js/lesson-ui.js       v4 模板共用互動（新頁引用）
sw.js                 Service Worker（cache-first）
favicon/              多解析度圖示＋site.webmanifest
math/     index、ch1–ch5（二上）、2b-ch1–ch4（二下）、3a-ch1（三上）、skills（12 技能卡）、plan（兩週計畫）
chemistry/ index、ch5–ch7、exam1（段考解析）、skills（7 技能卡）
physics/  index（尚未開始）
```

## 本機開啟與部署
本機：任何靜態伺服器即可（`python3 -m http.server`）；直接開檔案也可但 sw 不作用。
部署：push 到 GitHub main → Pages 自動發布。**更新後請用無痕視窗或硬重新整理驗證**（sw 快取）。

## 版本流程（嚴格）
1. 改動內容 → 全站版號同步 bump：每頁 footer、README、CLAUDE.md、`sw.js` 的 `studybase-vX.Y.Z`
2. grep 驗證舊版號零殘留
3. 發布檢查：div 平衡、設計鐵律（無 gradient/box-shadow/radius>4px/animation）、內部連結存在、lpill/lesson 數一致、SL 陣列與課數相符
4. zip「StudyBase VX.Y.Z.zip」交付

## localStorage 鍵
| 鍵 | 用途 |
|---|---|
| `sb-res-k{1-12}-q{2-5,a,b}` | 數學技能卡四鈕結果（1 獨立對/2 獨立錯/3 提示後對/4 看解答） |
| `sb-res-c{1-7}-q{2-5,a,b}` | 化學技能卡四鈕結果 |
| `sb-p14-{day}-{idx}` | 兩週計畫勾選 |
| `sb-p14-start` / `sb-p14-round` | 本輪開始日期／名稱 |
| `sb-kpi-w{1,2}-{1-5}{n,d}` | 週指標分子/分母 |
| `sb-cur-k` / `sb-cur-c` | 技能卡目前瀏覽位置 |
| `sb-hist` | 輪次歷史（JSON 陣列，歸檔時寫入） |

## Service Worker 更新方式
改 `sw.js` 第 2 行 cache 名（隨版號）→ 舊快取於 activate 清除。CORE 預快取：三科首頁、主首頁、offline、兩個技能卡頁、plan、css、圖示；其餘頁面開過即動態快取，未開過離線時導向 offline.html。

## 版本歷史（精要）
- **V2.14.0** 化學三層化：三章定「核心路徑」（poscard 指引：選讀課段考前可跳過、測驗必做）；六課標〔選讀〕並預設收合（ch5 重要元素、ch6 原子模型演進/特殊化學式/亞佛加厥數/化學式種類、ch7 鹼土族與鹵素）——lpill 加註、課首「展開延伸內容」鈕、內容 JS 包裹收合（aria-expanded）
- **V2.13.0** 頂欄/底欄內容對齊 720px 內容欄（返回鍵不再貼死左上；ch5 頂欄掛入 .topbar）；技能卡改「一次一張」（上一張/下一張＋knav 切換＋#錨點深連結＋繼續上次進度，位置記在 sb-cur-k/c）＋頁首收合（說明入「怎麼使用？」）；計畫頁首改「今天：Day N · 標題＋完成 x/54＋開始今天任務」（依開始日期或首個未完成日推算）
- **V2.12.0** 化學技能卡 7 張；plan KPI 自動彙整＋輪次歸檔/匯出/匯入；offline.html＋sw 離線導向；抽出 css/lesson.css、js/lesson-ui.js；README/CLAUDE/DESIGN/handoff 全面重寫（先前 README/CLAUDE 為空檔，本版起補正）
- V2.10.0 化學四頁版面統一（定位卡＋errlog＋gate；ch5 舊違規經授權歸零）
- V2.9.x 學習系統：12 數學技能卡（四格＋五題＋四鈕＋燈號＋回鍋變式）、14 天計畫（燈號驗收＋可計算 KPI＋暫停/爸媽規則）；全站審查修正（化學答案鍵與知識錯誤、數學嚴謹性）
- V2.7.x–V2.8.x 二下四章建置與四輪審稿修正；二上 ch2/ch3/ch4 錯題五~九批三層融入（換數字教學→無提示→原題再測）
- V2.0–V2.6（本檔重建前）：二上五章＋統計、化學 ch5–ch7＋exam1、薰衣草改版、PWA、SELA-Kit 對齊

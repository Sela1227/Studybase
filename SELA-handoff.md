# SELA-handoff — StudyBase V2.11.0（2026-07）

## 交接快照
- 21 個 HTML：根 2（index、offline）＋數學 13＋化學 6＋物理 1（僅首頁，標「尚未開始」）
- 資產：css/main.css、css/lesson.css、js/lesson-ui.js、sw.js（27 行）、favicon 全套＋webmanifest
- 文件五件套：README、CLAUDE、DESIGN（V3）、本檔；專案知識另有 常駐設定 V2／定稿品質規範 V2／COURSE-SYSTEM／學生特化

## 系統能力
- 教材：數學二上 5 章＋二下 4 章＋三上相似形；化學 ch5–ch7＋段考解析——全部 v4 模板（定位卡/雙層導覽/errlog ABCD/gate）
- 學習系統：數學 12＋化學 7 技能卡（四格＋五題＋四鈕＋燈號＋回鍋變式）、14 天計畫（燈號驗收、可計算 KPI＋自動彙整、暫停與爸媽規則、輪次歸檔/匯出/匯入）
- PWA：cache-first、CORE 預快取關鍵頁、未快取頁離線導向 offline.html

## 部署與驗證
push main → GitHub Pages；驗證一律無痕或硬重新整理。版本流程與檢查清單見 README。

## 已知事項
- 章節小標 600 字重為既有全站慣例（DESIGN V3 已記錄）
- 化學「三層化」（核心路徑/延伸收合）為下一項 backlog
- localStorage 紀錄綁裝置；換機用 plan 頁匯出/匯入

# CLAUDE.md — StudyBase 工作準則（V2.12.0）

## 專案速覽
單一學生（阿蔓，2026.8 升國三，會考 2027.5）的復習站。21 HTML、純靜態、GitHub Pages。學習方法論見專案知識《教材設計_常駐設定》；每頁怎麼寫見《定稿品質規範》；視覺見 DESIGN.md。

## 鐵律與慣例
- 設計：無 gradient／box-shadow／border>1px／radius>4px（var --r）／animation；字重 300/400/500。**已知既有偏差**：章節模板小標（.plabel、.plabel2、.qsec h3 等）用 600——18 頁共用慣例，回報過、暫留；exam1 有 600×10。
- 版本同步：每頁 footer＋README＋本檔＋sw.js cache 名，grep 零殘留才算完成。
- sw.js：**先讀後寫**（曾發生 'w' 模式先開導致清空）；27 行慣例，若行數變動須在 README 註明。
- patch 腳本：assert 錨點在前、寫檔在最後——assert 失敗＝檔案未存，修錨點後整支重跑。
- 每次交付：div 平衡檢查、鐵律 grep、內部連結健檢、（章節頁）lpill/lesson 與 SL 一致。
- 內容錯誤：只回報，未經 Sela 授權不自行修改；設計變更同樣需授權。
- 新章節檔常見 `href="#"` 返回鍵 → 一律改 `index.html`。
- 審稿意見常以空 document 附上 → 讀 /mnt/user-data/uploads/審稿意見.txt。審稿可能出錯：課本照片／答案優先（例：L 形題周長 30）。
- 錯題融入三層：換數字教學 → 無提示練習 → 原題只進再測；教學例絕不用原題。

## 學習系統（資料面）
四鈕：1 獨立對／2 獨立錯／3 提示後對／4 看解答。燈號：紅＝(2)+(4)≥2；綠＝(1)≥3 且 (2)=(4)=0。localStorage 鍵表見 README。plan 的自動彙整讀 sb-res-*；歸檔寫 sb-hist。

## 檔案地圖（21 頁）
math：ch1–ch5、2b-ch1–ch4、3a-ch1、skills、plan、index／chemistry：ch5–ch7、exam1、skills、index／physics：index／根：index、offline。

## 共用層（V2.12.0 起）
css/tokens.css＋css/components.css 已被全站引用並覆蓋內嵌樣式——改按鍵/導覽/焦點一律改這裡，不再逐頁修。按鍵四階映射：primary=.gate .gbtn/.gate .btn/#check-btn；secondary=.hbtn/.qbtn/.inter-btn/.gbtn2；ghost=.prob .toggle；danger=.rbtn。文字字典：顯示解答/收起解答/批改/重新作答/完成本節/← 上一課/下一課 →。

## Backlog
- 技能卡「一次一張」與頁首收合（審稿第三優先 UI）
- 化學三層化（核心路徑／延伸收合）——需逐段內容判斷，單獨一輪
- 3a-ch1 相似形依新課本目次重構；國三上圓、幾何證明
- 兩週試行後依四指標數據調整

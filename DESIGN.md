# DESIGN.md V3 — StudyBase 視覺規格（依 V2.11.0 全站現況重整；取代所有舊版）

## 定位
Nordic Minimalism × 薰衣草。安靜、可讀、無裝飾——所有視覺服務於「認出結構」。

## 鐵律
禁止：gradient、box-shadow、border 寬 >1px（強調用 border-left:2px 允許）、border-radius >4px、animation/transition 動畫、大面積色塊。
字重限 300／400／500。**已知偏差（暫留）**：章節模板小標類（.plabel/.plabel2/.qsec h3/.prereq .t）600、exam1 標籤 600——18 頁共用慣例，未經授權不再擴散、不新增 700。

## 色票令牌（全站統一）
```
--bg:#F6F3FC   --surface:#FFFFFF   --surface2:#F1ECFA
--border:#E8E1F2  --border-mid:#CBC0E0  --border-dark:#A89EC2
--text-1:#2A2335  --text-2:#6E6580  --text-3:#A89FBC
--r:4px
```
科目主色：數學 accent `#5B57C2`（accent-l `#ECEBFA`）；化學 orchid `#A368B0`（技能卡）；化學章節沿用各章 c1 色（ch6 `#5B57C2`、ch7 `#5E83C0`、exam1 `#8A54A3`）；輔助 mint `#3E8F79`、rose `#B7476A`。SELA logo 橘 `#F36825` 永不改色。

## 版面
max-width 560–720px（章節 720）、觸控目標 ≥44px、radius 一律 var(--r)。字體棧：-apple-system → PingFang TC → Noto Sans TC。

## 元件語彙（v4 模板，css/lesson.css 為權威）
- topbar：sticky、rgba(246,243,252,.96)＋blur、麵包屑
- poscard 定位卡：border-left 2px 主色；plabel／h1／p（依賴鏈）／meta
- stabs（節）＋ lpills（課）：雙層導覽，aria-pressed
- 課文塊：hi（關鍵句）、eg（例：q/step/why）、warn（rose 左線）、note、rev（複習框）、tbl＋table-scroll、frac 直式分數
- 練習：觀念辨識 MCQ（retry）、prob＋toggle（aria-expanded）＋難度標籤【核心必會/進階/挑戰】
- errlog：ABCD 錯題登記表＋leg 補救指引
- gate：自我檢查清單＋過關鈕＋pass 文案
- 技能卡：g4 四格（看到什麼/心裡想/動手第一步/最容易掉的坑）、hbtn 第一步提示、res 四鈕、cstat＋lamp 燈號（綠/黃/紅）
- SVG：role="img"＋aria-label、max-width:100%、示意圖註明「未按比例」

## 列印
隱藏 topbar/bnav/stabs/lpills/toggle/gate 按鈕/hbtn/res；ans 預設不印。

<div align="center">
  <img src="favicon/sela.svg" width="120" alt="SELA"/>
  <h1>阿蔓明道國中復習專班</h1>
  <p>明道國中 · 數學・物理・化學 複習教材</p>
  <p><strong>V2.0.0</strong></p>
</div>

---

## 這是什麼

給明道國中學生阿蔓的數理化複習網站。純靜態（HTML + CSS + 原生 JS），零後端，GitHub Pages 託管。採北歐極簡設計系統（見 `DESIGN.md`），建課流程見 `COURSE-SYSTEM.md`。

---

## 版本歷程

### V2.0.0 — 2026.06 — 對齊 SELA-Kit（大升級里程碑）
- 英文化專案名：資料夾/zip → `StudyBase`（UI 顯示仍中文）
- 補 favicon 套組 + SELA 品牌歸屬（`favicon/`）
- 補 `.gitignore`（Kit 模板）
- 建立 `CLAUDE.md`（Kit 格式，含踩坑/業務對映表/衝突仲裁區塊）
- 產出 `SELA-handoff.md`（首次對齊里程碑）+ `SELA-logo-prompt.md`（app logo 待生成）
- 全站 footer 版本同步 V2.0.0；版號自此嚴格三位
- 保留：北歐配色、中文 UI 名、教材內容、DESIGN.md/COURSE-SYSTEM.md（已驗證，不為對齊改動）

### V1.8 — 2026.06
- 數學第一章 相似形整合（首個數學章節，14 課 + 26 題測驗 + 互動圖解）

### V1.7 — 2026.03
- 化學第七章 化學反應加入

### V1.6 — 2026.03
- 化學第一次段考詳解全面改版

### V1.5 — 2026.03
- 化學第一次段考：新增「題型攻略」分頁

### V1.4 — 2026.03
- 化學第六章以北歐設計重製、補齊返回導覽

### V1.3 ～ V1.0 — 2026.03
- 建站、北歐美學重製、化學五/六章與第一次段考詳解、設計規格書建立

---

## 部署到 GitHub Pages

用 Git Pusher 匯入 `StudyBase V2.0.0.zip` → 推 main，然後：

1. GitHub repo 設為 Public
2. Settings → Pages → Source: Deploy from a branch → main → `/(root)`
3. 約 1-2 分鐘後連結出現在 Settings → Pages
4. 後續更新：推 main 即自動重新部署

---

## 檔案結構

```
StudyBase/
├── index.html              ← 首頁（類型 A）
├── css/main.css            ← 類型 A 共用樣式（北歐設計系統）
├── chemistry/
│   ├── index.html          ← 化學主頁
│   ├── ch5.html ch6.html ch7.html   ← 第五～七章（類型 B）✅
│   └── exam1.html          ← 第一次段考詳解（類型 C）✅
├── math/
│   ├── index.html          ← 數學主頁
│   └── ch1.html            ← 第一章 相似形（類型 B）✅
├── physics/
│   └── index.html          ← 物理主頁（待新增內容）
├── favicon/                ← SELA favicon 套組（app logo 生成後替換）
├── DESIGN.md               ← 設計規格書（真相）
├── CLAUDE.md               ← 給下個 Claude 的工作上下文
├── SELA-handoff.md         ← 給 Kit Claude 的對齊回流摘要
├── SELA-logo-prompt.md     ← app logo 生成 prompt（待 Sela 生圖）
├── .gitignore
└── README.md
```

---

> Made by **SELA**, with **Claude**

# SPEC — 逐頁清單

每個原型頁的用途、狀態、互動與導覽串接。全部單檔自足、純 CSS/原生 JS、OutSystems 可移植。

---

## `mwi-list-redesign.html` — WI 列表（工作流版）
第一層進來就是這頁（Maintenance WI 清單）。
- **狀態分頁**：To Do｜In Progress｜Submitted，各帶計數（`.wi-tab data-bucket`）。
- **Action 欄**：In Progress 列有 **Assign to self**（`.act-col`）→ 跳藍色 toast，該列即時移入 To Do。
- **5 狀態語意色** chip：`.chip.st-draft/st-wip/st-ver/st-appr/st-closed`。
- **桌機 9 欄表格 ↔ 手機卡片流**（≤860px 切換）。欄位為
  `WI number｜Status｜Road（+Sector·Sub-sector）｜Asset（+Element type）｜Defect type｜Work type｜Est. completion｜Assign to｜Last updated`
  ＋ In Progress 分頁才出現的 `Action` 欄（sticky 釘右緣）。
  **完整的「原系統 17 欄 vs 原型」對照與取捨理由見 `SPEC_field-matrix.md` §1d** —— 改列表欄位一律先改那節。
- **Filter & sort 抽屜**（右側 drawer，取代原本雜亂的 3-pane Advanced Filter）：
  - **排序不在抽屜裡** —— 桌機點表頭排序（見下）；手機沒有表頭，抽屜頂端才出現 **Sort by** pill 列。
  - **filter 手風琴 ＝ 表格欄位，同名同序**：
    `Status／Road（縮排 Sector、Sub-sector）／Asset（縮排 Element type）／Defect type／Work type／
    Est. completion／Assign to`。縮排的三項是 Road／Asset 兩格的副標內容，非獨立欄位。
  - 🚫 `Department`／`Instruction date` **已實作但隱藏**（表格沒有對應欄位）；程式碼保留，
    把 `'__other','fdept','finst'` 加回 `ORDER` 即還原。**完整對照見 `SPEC_field-matrix.md` §1e**。
  - 值多的 facet（>8）長出**搜尋框**，預設顯示 6 個 ＋「Show all (N)」，已選置頂。
  - **父子連動**：Sub-sector 跟著 Sector、Element type 跟著 Asset type。
  - 兩組**日期區間**：快捷 pill（Last 7/30/90 days；Overdue / Next 7 / Next 30 days）＋自訂起訖。
  - Clear all / Apply；套用後工具列「Filter」鈕顯示「· N」計數，並出現「Clear filters」。
- **表頭即排序控制**（桌機）：9 欄皆可點，點一次套用、再點反向，作用中欄位標主色＋單向箭頭＋`aria-sort`；預設 `Last updated ↓`。空值一律沉底。
- **工具列 Search** 打通：比對 WI 號／road／sector／asset／element／defect／work type，與 filter 疊加。
- **demo 資料 24 筆**（14 sector、24 條路、4 department、5 種狀態），**分頁每頁 10 筆且真的會換頁**。
- **列導覽**：`tr.row-link data-href` / 卡片 `href` → `mwi-wi-workflow.html`，帶當前角色。
- **New WI** 僅 Contract Officer 顯示。
- META map 以 WI 號為 key：`{b:bucket, s:status}`。

## `mwi-list-redesign-a.html` / `mwi-wi-workflow_layouta.html` — ⭐ Slate 變體（原「Green」）
與原版同結構、只換配色。2026-08-04 依主管意見（綠色少一點、多用官網的顏色）重做：
- 側邊欄由 teal 滿版改成**中性墨 `#343A40`**（＝官網內文墨色）；主色改成**官網連結色 `#5C6BBC`**。
- 5 狀態 chip 改用 **LTA 官方 accent `.bg-color1..5`**，語意維持（綠＝完成、黃＝審查中）。
- 版本代號 **Green → Slate**（六頁切換器與 `<title>` 已同步）。
- **完整 token、取樣來源與對比值見 `SPEC_design-system.md` §1b**；六案對照頁見 `sidebar-compare.html`。

## `mwi-view-redesign.html` — WI 檢視（唯讀）
View/Edit 分離的示範頁（最能展示「唯讀該長怎樣」）。
- **label-value 排版**，空值顯示灰字「Not set」（不是可點的 Select… 佔位）。
- **單頁四區 + sticky section nav + scroll-spy**；「Go to」跳轉 select。
- **asset 卡 + task `<details>` 手風琴**（原生 details，可摺疊）。
- **資料量切換 demo**（右上 `.seg-btn data-scn`）：0 / 1 / 3×9 task，示範同一設計吃不同資料量。
- 新加坡島形 **SVG 地圖 placeholder**；Funding **empty state**。
- fixed topbar 遮住定位 → 用 `scroll-margin-top` 修正。

## `mwi-wi-create.html` — 新建 WI（Contract Officer）
對齊實際 Add 表單。
- 合約下拉、Location Tag、日期預設今日。
- **Site & assets**：type / tag / ID 併成**三欄一列**（`.asset-lite-grid`）＋地圖圈選；**Add / Remove asset 可運作**，Remove 僅多 asset 時顯示（單一時淡化/隱藏）。
- 附件；**Fund Detail** 多列表格；滿版多欄佈局。

## `mwi-wi-edit.html` — 編輯 WI（＝ create 欄位母版）
**欄位結構與 create 頁 100% 一致**（References／Classification／Location & description／Site & assets，含 asset/task Draft 面板、Others＊ 條件欄、Add/Remove、跳轉 chips），只帶入 RMWP-TR388-RRFM-M-17 的既有值、動作列為「Save changes」。**唯一差異**：Funding 不在此頁，留在 workflow 頁（底部有說明）。防漂移原則見 `SPEC_field-matrix.md`：欄位改動要 create ＋ edit 兩頁同步。

## `mwi-wi-workflow.html` — ⭐ 角色感知工作流 Edit（主打）
右上切換 **CO / Site Supervisor / Engineer**，整頁跟著變（`ROLES` 物件驅動，詳見 `SPEC_roles-workflow.md`）。
| 角色 | 側欄 | 狀態 | Wizard | 我負責（可編輯） | 底部動作 |
|---|---|---|---|---|---|
| Contract Officer | 全部 | Approval in progress | 4 步 | Approval（JM 日期/付款狀態） | Return×3／Reassign／**Approve & Close** |
| Site Supervisor | 精簡 | Work in progress | 2 步 | 現場更新（照片＋進度） | Return／Reassign／**Submit to Engineer** |
| Engineer | 中 | Verification in progress | 3 步 | Price Schedule（BOQ＋聲明） | Return／Reassign QS／**Submit to LTA Officer** |
- 面板：step1 Work/Task/Site（SS「site update」owner 區塊：Add Photo + 進度 chip）、step2 Fund Detail、step3 Price Schedule（BOQ 表 + Contractor Declaration）、step4 Approval。
- **送出並指派彈窗**「Select X and submit」（Common Queue / 特定人 + Remarks）。
- logo 用 `<img src="logo.png">`（其餘頁多為 base64 內嵌）。

## `mwi-task-edit.html` — Site Supervisor 手機現場填單
task stepper、照片三槽 Add Photo、progress chip、sticky 動作列。實機錄影已驗證此方向正確。

## `logo.png` — LTA 官方 logo
各頁 base64 內嵌（workflow 頁用 `<img>`）。base64 是單一超長行（~56K 字元），用 Grep 或 offset 讀，別整行讀。

---

## 全站統一元件（六頁一致）
- **角色切換器**（右上）：三色頭像 CO=navy / SS=橘 / Engineer=紫；無 tag、無 hint banner；`?role=` 跨頁保留。
- **自訂下拉 `.xselect`** / **自訂 date picker `.xdate`**：fixed 定位、永遠最上層。
- toggle、empty state、toast、5 色狀態 chip。
（元件細節見 `SPEC_design-system.md`。）

## 導覽動線（已串接）
列表（分頁 / Assign to self）→ workflow 頁（帶角色）；workflow 返回 → 列表；View ↔ Edit ↔ Task；手機 sidebar 收進 hamburger 抽屜。**角色在整條動線保留**。返回鍵放頁面左上。

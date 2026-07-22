# SPEC — 逐頁清單

每個原型頁的用途、狀態、互動與導覽串接。全部單檔自足、純 CSS/原生 JS、OutSystems 可移植。

---

## `mwi-list-redesign.html` — WI 列表（工作流版）
第一層進來就是這頁（Maintenance WI 清單）。
- **狀態分頁**：To Do｜In Progress｜Submitted，各帶計數（`.wi-tab data-bucket`）。
- **Action 欄**：In Progress 列有 **Assign to self**（`.act-col`）→ 跳藍色 toast，該列即時移入 To Do。
- **5 狀態語意色** chip：`.chip.st-draft/st-wip/st-ver/st-appr/st-closed`。
- **桌機 6 欄表格 ↔ 手機卡片流**（≤860px 切換）。
- **Filter & sort 抽屜**（右側 drawer，取代原本雜亂的 3-pane Advanced Filter）：
  - 頂部 **Sort by** pill 列（Newest / Est. completion / Road A–Z / Status）。
  - **filter 手風琴**：Status / Sector / Work type / Asset type / Road，toggle chip + 每區計數 badge。
  - Clear all / Apply；套用後工具列「Filter」鈕顯示「· N」計數，並出現「Clear filters」。
  - 手機的「Sort by」已**併入此 Filter**（Candace 選 option B），不再單獨出現。
- **列導覽**：`tr.row-link data-href` / 卡片 `href` → `mwi-wi-workflow.html`，帶當前角色。
- **New WI** 僅 Contract Officer 顯示。
- META map 以 WI 號為 key：`{b:bucket, s:status}`。

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

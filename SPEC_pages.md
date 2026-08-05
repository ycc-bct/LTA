# SPEC — 逐頁清單

每個原型頁的用途、狀態、互動與導覽串接。全部單檔自足、純 CSS/原生 JS、OutSystems 可移植。

---

## `mwi-list-redesign.html` — WI 列表（工作流版）
第一層進來就是這頁（Maintenance WI 清單）。
- **狀態分頁**：To Do｜In Progress｜Submitted，各帶計數（`.wi-tab data-bucket`）。
- **Action 欄**：In Progress 列有 **Assign to self**（`.act-col`）→ 跳藍色 toast，該列即時移入 To Do。
- **5 狀態語意色** chip：`.chip.st-draft/st-wip/st-ver/st-appr/st-closed`。
- **桌機 9 欄表格 ↔ 手機卡片流**（≤860px 切換）。2026-08-05 卡片改成 **Jira gadget 式**：
  標題區（WI 號＋狀態 chip → 路名＋sector）在上，底下是 **label 在上、value 在下**的欄位格。
  `.wc-grid` 用 `repeat(auto-fit,minmax(100px,1fr))`，**375px 自動兩欄、414px 以上三欄**，
  不需要自己的斷點。標題區只留 **WI 號＋狀態 chip**，其餘 **七個欄位**（Road／Asset／Defect type／
  Work type／Est. completion／Assign to／Last updated）全部在格子裡，Road 與 Asset 同樣是
  `<strong>主值</strong> · 副值` 的寫法。
  - **In Progress 的「Assign to self」按鈕是格子的第 8 個項目**，不是獨立一列 ——
    七個欄位在兩欄時最後一列剩 1 格、三欄時剩 2 格，按鈕正好補進去。
    JS 是 `(c.querySelector('.wc-grid')||c).appendChild(d)`，**改格子數量時要重算會不會剛好填滿**。
  - label 用**句首大寫不用全大寫** —— 全大寫的「EST. COMPLETION」在三欄時會斷成兩行、
    整列對不齊；顏色用 `--grey-600` 不是 `--grey-500`，12px 算小字，grey-500 在白底只有 4.20:1。
  - filter／搜尋的取值改吃 `[data-f="…"]`（原本是靠 `.meta > span` 的索引），
    **改卡片結構時 `readRow` 的 getter 要一起改**。
  欄位為
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
  - **全選＝選項列開頭的「All」pill**（虛線框，全選時實心）；再按一次等於清空。
    **Clear 在 facet 標題列最右、chevron 左邊**，沒有選任何東西時 disabled。兩者都不佔獨立列。
    為此 chevron 要從 `.fd-acc` 搬到同列的 `.chev-slot` —— `<button>` 裡不能再放 `<button>`，
    整列點擊改由 `.fd-hd` 代理，展開狀態掛在 `.fd-sec.open`（日期 facet 仍是舊的 `.fd-acc.open + .fd-opts`）。
    「All」pill 沒有 `data-cat`，`pending()` 與委派的 chip handler 才不會把它當成一個值。
  - **「全選」不算篩選**（2026-08-05）：facet 內是 OR、facet 間是 AND，所以勾滿＝沒排除任何東西。
    勾滿時 facet badge 顯示 **All**（灰底，不是數字），Filter 按鈕的計數也不算它 ——
    否則會出現「9 個篩選中」但一列都沒少的假訊號。判斷用 `coversAll(sel, domainOf(cat))`，
    子 facet 的 domain 會跟著已套用的父選項收窄。
  - **Asset／Element type／Defect type 多一個「Not set」選項**：`—` 存在於資料但不在
    `VOCAB` 字典裡，補之前**全選反而比都不選少一列**（實測掉 `RMWP-TR388-RRFM-M-24`）。
    補上之後兩者結果才真的相同，也才篩得出「Asset 未設定」——線上本來就有 Asset not found 這個狀態。
    連帶 `PMAP` 的 `v==='—'` 排除條件要拿掉，否則 Asset 選 Not set 時 Element type 會空掉。
  - Status 的選項**沿用五個階段色**（未選中時帶 `.st-*` 底色與文字色，選中仍是主色實心）。
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
2026-08-04 依主管意見（綠色少一點、多用官網的顏色）重做配色：
- 側邊欄由 teal 滿版改成**中性墨 `#343A40`**（＝官網內文墨色）；主色改成**官網連結色 `#5C6BBC`**。
- 5 狀態 chip 改用 **LTA 官方 accent `.bg-color1..5`**，語意維持（綠＝完成、黃＝審查中）。
- 版本代號 **Green → Slate**（六頁切換器與 `<title>` 已同步）。
- **完整 token、取樣來源與對比值見 `SPEC_design-system.md` §1b**；六案對照頁見 `sidebar-compare.html`。

2026-08-05 進一步**套用 Navy 的 sidebar layout**（Slate / Navy 統一的第一步）：
- 桌機**沒有 topbar** —— `@media(min-width:1021px){--topbar-h:0px}` 把它整個關掉。
- 側欄變成整條左欄（`top:0`、`display:flex`）：**頂端白 logo（`logo_white.png`）→ 導覽 → 底部
  `.sb-foot`（Notifications ＋ 角色切換器）**。角色選單用 `position:fixed` 往上開，才不會被側欄的
  捲動框裁掉。
- 側欄 `z-index:48`，蓋過 action bar，bar 的底色帶因此停在側欄邊緣。
- **收合軌（76px）**同步隱藏 logo、紅點與角色姓名，頭像縮成 44px 圓。
- **手機仍保留 topbar**，但改成側欄同色的深色條，只放漢堡＋白 logo；通知與角色改在抽屜底部。
- 新增 `--danger-on-dark:#E03131`（原 `--danger-600` 的 LTA 楓紅在深色側欄上會消失）。

同日**圓角語彙也對齊 Navy**：`--r-sm 6→14px`、`--r-md 10→18px`，控制項／按鈕／chip／nav item
一律 `999px` 藥丸。**`--r-lg` 刻意留在 14px**（Navy 是 24px）—— 它畫的是 `.card`／`.summary-strip`／
`.table-wrap`／`.wi-card`／`.rail`，這些容器太圓會怪。同理 `.boq-wrap` 釘回 10px、多行值
（`.control.area`／`.ro-1line`／Return 的 textarea）留 18px 圓角而非藥丸。

同日再修三處：
- **兩頁側欄尺寸鎖死一致**。兩頁的側欄是不同 markup 蓋出來的（list 用 `.nav-item.sub`，
  workflow 用可摺疊的 `.nav-sub-list`，群組列一邊是 `<a>` 一邊是 `<button>`），列高差 1–3px，
  切換頁面時整條選單會跳。現在兩邊都釘 `.nav-item{min-height:43px}` ＋ 子項 `38px`，
  子項字級統一 `.8125rem`，workflow 的 `.nav-sub-list` 去掉多餘 margin。**改側欄一律兩頁同步。**
  子項圓角也要各自寫：list 是 `.nav-item.sub`（吃得到 `.nav-item{border-radius:999px}`），
  workflow 是獨立的 `.nav-sub`，得另外補一條。
- **展開的父層不再填色**：子項已經用白色藥丸標出所在位置，父層再填一層是重複。
  `.nav-group>.nav-item.active:not(:hover){background:none}` —— `:not(:hover)` 是為了留住
  hover 狀態（兩者權重同為 0,2,0，不加會被後寫的規則蓋掉）。workflow 收合群組時父層填色會回來。
- **`.wi-tabs` 改藥丸**：拿掉底線軌，作用中填 `--teal-600`＋白字（4.59:1），計數改白底
  `--teal-700` 字（6.96:1）—— 半透明計數在填色 tab 上會掉到 4.5:1 以下。
- **`.rail` 補滿視窗高**：原本 `calc(100vh - 233px)` 是為 72px topbar 寫的，topbar 拿掉後
  下方空 92px。改成 `calc(100vh - 73px - 84px)`（頁首 73 ＋ action bar 68 ＋ 留白 16），
  且**必須包在 `@media(min-width:1021px)` 裡** —— 手機的 `.rail` 是 `height:auto`，
  這段樣式在那條 media query 之後，不加斷點會蓋掉它。

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
  底部的 **Prototype version 切換器只剩 Original / Slate 兩版**（2026-08-05）—— Navy 的樣式已經
  併進 Slate（側欄佈局、圓角語彙），連結全部撤掉。`mwi-list-redesign-navy.html` 與
  `mwi-wi-workflow_photoswipe-navy.html` **檔案保留、切換器也保留**，直接開網址還能看，只是動線上進不去。
- **自訂下拉 `.xselect`** / **自訂 date picker `.xdate`**：fixed 定位、永遠最上層。
- toggle、empty state、toast、5 色狀態 chip。
（元件細節見 `SPEC_design-system.md`。）

## 導覽動線（已串接）
列表（分頁 / Assign to self）→ workflow 頁（帶角色）；workflow 返回 → 列表；View ↔ Edit ↔ Task；手機 sidebar 收進 hamburger 抽屜。**角色在整條動線保留**。返回鍵放頁面左上。

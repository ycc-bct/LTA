# SPEC — 角色 × 階段 欄位矩陣（WI 表單）

這份是 MWI 表單所有欄位的單一事實來源:每個欄位的**必填**、**所屬步驟**、**哪個角色/階段看得到、能不能編輯**。改任何一頁的欄位都以這份為準,不刪、不亂加、不亂改必填星號。

## 信心標記(重要)
- ✅ **截圖確認**:有實機截圖逐欄核對過(dev 環境 RMWP-TR388-RRFM-M-17,SA01 View)。
- 🎥 **錄影推得,待核**:來自 walkthrough 錄影(2026-07-08)描述,尚未用對應角色帳號逐一實機核對。
- ➕ **我方設計加值**:原系統沒有、是重設計為了體驗加的(非欄位,多為版面/導覽)。

> ⚠️ dev 是稀疏測試資料;真實使用量以 Arjun 描述為準(1 WI 可到 3 asset、每 asset 3 task)。詳見 `SPEC_data-model.md`。

---

## Step 1 — Work / Task / Site Detail

### 1a. WI 抬頭 / 分類 / 位置欄位 ✅
原系統呈現為一個密集 3 欄 grid(row-major),欄位與必填如下(順序即原設計順序):

| # | 欄位 | 必填 | 型態 | 備註 |
|---|---|---|---|---|
| 1 | Contract No. | ✅ ＊ | 下拉 | **前置**:篩選下面的 Road / Sector / Asset |
| 2 | Work Instruction No. | ✅ ＊ | 系統產生(鎖) | 建單時自動給號,使用者不填 |
| 3 | LTA CCS Work Order No. | — | 文字 | |
| 4 | Department | ✅ ＊ | 下拉 | RRFM / CFM / BDG / PATH… |
| 5 | Feedback Date | ✅ ＊ | 日期 | |
| 6 | Instruction Date | ✅ ＊ | 日期 | |
| 7 | Work Type | ✅ ＊ | 下拉 | SIMPLIFIED ITEM / WORKS ORDER-ACCIDENT / OUTCOME BASED DEFECT / PLANNED WORKS / WORKS ORDER-ADHOC |
| 8 | Work / Defect Description | **—(無星)** | 文字 | ⚠️ 我方原型誤標必填,已知需改回選填 |
| 9 | Defect Reported By | — | 下拉 | INSPECTION / Public feedback… |
| 10 | Road Name | ✅ ＊ | 可搜尋下拉 | 受 Contract 篩選 |
| 11 | Location Tag | — | 下拉 | REX-…；受 Contract/Road 篩選 |
| 12 | Defect Reference No. | — | 文字 | |
| 13 | Sector | ✅ ＊ | 下拉 | 受 Contract 篩選 |
| 14 | Sub-Sector | ✅ ＊ | 下拉 | 受 Sector 篩選 |
| 15 | WI Remarks | — | 文字區 | |
| 16 | Is Recurring | — | checkbox | |

**必填共 8 個使用者要填的**(WI No. 系統產生不算):Contract、Department、Feedback Date、Instruction Date、Work Type、Road Name、Sector、Sub-Sector。
**依賴鏈**:Contract → Road / Location Tag / Sector → Sub-Sector / Asset。故「必填優先」須在依賴允許範圍內排(Contract 一定最前)。

### 1b. Site & assets ✅(Draft 階段)

**Asset Availability**
| 欄位 | 必填 | 型態 |
|---|---|---|
| Asset Not Found | — | checkbox |

**Asset N**(可摺疊面板;可多個,「Add another asset」)
| 欄位 | 必填 | 型態 | 備註 |
|---|---|---|---|
| Asset Type Name (Code) | ✅ ＊ | 下拉 | |
| Asset Tag | — | 下拉 | 有 **Map** 按鈕 |
| Asset Id | — | 文字 | |

**Task N**(巢狀在 Asset 底下,可摺疊;可多個)
| 欄位 | 必填 | 型態 | 備註 |
|---|---|---|---|
| Element Type | ✅ ＊ | 下拉 | |
| Defect Type | ✅ ＊ | 下拉 | 選 OTHER→多出 Others＊ 自由輸入欄 |
| Default X Y Coordinates | — | 文字 | 有 **Map** 按鈕 |
| Landmark | — | 文字 | |

> 以上 Task 4 欄是 **CO 建單 / Draft** 階段的最小集(截圖確認)。

### 1c. ⭐ Task 欄位隨階段/角色成長 🎥
同一個 Task,WI 前進到 **Site Supervisor(Work-in-Progress)** 階段會長出現場執行欄位(錄影所見,約 16 欄,待實機核):
- Before / During / After 照片(三槽)
- Defect Cause
- Action
- Unit
- Length
- Measurement
- Work Progress %
- (Task-level) Remarks

→ 這就是「不同角色不同欄位」的來源:**抬頭欄位固定,Task 欄位依階段遞增**(Draft 4 欄 → WIP ~16 欄)。

---

## Step 2 — Fund Detail 🎥
（依錄影,待核）
- Funding Source 多列表格:`Funding Source | Fund Owner | Section | Estimated Amount | Delete`（＋加列）
- Estimated Amount
- Supervision (division)
- ECD（Estimated Completion Date）
- Fund Remarks
- Attachment

> 原型 create 頁「Funding」區目前是這步的簡化版。備註寫「completed by the LTA project team」。

## Step 3 — Price Schedule（Engineer）🎥
（依錄影,待核）
- Payment Type
- Final Amount
- Actual Completion Date
- Funding Source Details
- **Section BOQ 表格**:`Item | Description 1/2/3 | Unit | Rate S$ | Quantity | Amount S$ | Total`（＋加列）
- Attachment
- Price Schedule Remarks
- **Contractor Declaration**：Certificate of Supervision（勾選聲明）

## Step 4 — Approval（Contract Officer）🎥
（依錄影,待核）
- Payment Type
- Final Amount
- Approval Remarks
- Instruction / Estimated / Actual Completion / Feedback Date
- JM Submission Date（First / Latest）
- Days Taken to Submit JM (to LTA)
- Payment Status

---

## 角色 × 步驟 可見 / 可編輯矩陣

Wizard 步數依 WI 所在 stage 從 2 → 4 展開;每個角色負責填自己那一步(🎥 依錄影,待核)。

| 步驟 | Contract Officer | Site Supervisor | Engineer |
|---|---|---|---|
| 1. Work/Task/Site（抬頭欄位） | **建單填寫** | 唯讀（除 Task 現場欄位） | 唯讀 |
| 1. Task 現場執行欄位（照片/進度…） | 唯讀 | **填寫** | 唯讀 |
| 2. Fund Detail | 可見 | 依權限 | 依權限 |
| 3. Price Schedule（BOQ＋聲明） | 唯讀/可見 | — | **填寫** |
| 4. Approval | **填寫＋Approve & Close** | — | — |
| 側邊選單 | 全部 | 精簡（無 Reports/Contract/Config） | 有 Reports/Contract（無 Config） |
| 狀態(示範用) | Approval in progress | Work in progress | Verification in progress |

（角色動作列、指派彈窗、列表分頁詳見 `SPEC_roles-workflow.md`。）

---

## 原型對齊進度
- ✅ **create 頁（`mwi-wi-create.html`）已對齊 Draft 確認集**(2026-07-22):
  - Work/Defect Description 星號移除、改回選填(對齊 1a #8)。
  - 補上 Draft 階段 Task 子面板:Element Type＊、Defect Type＊、Default X Y Coordinates(＋Map)、Landmark;可 Add / Remove task(單一時 Remove 隱藏)。
  - 補上 `Asset Not Found` checkbox、Asset Tag 的 **Map** 按鈕。
  - **Defect Type 選「Other…」→ 顯示條件式 `Others＊` 欄**(切回其他選項自動收起,複製 task 也正確重置)。
  - 必填重排(Contract 最前、相關欄位不拆),＋手機區塊跳轉 chips。
- ✅ **view 頁（`mwi-view-redesign.html`）asset/task 已對齊**(2026-07-22):
  - `Default X Y coordinates` 正名(原「Coordinates (SVY21)」)並補齊到全部 9 個 task;空值以「Not set」呈現。
  - WIP 欄位名稱對齊矩陣:`Action`(原「Action taken on site」)、`Remarks`(原「Task remarks」)。
  - 每個 task 一致呈現確認的 Draft 4 欄(Element type／Defect type／Default X Y coordinates／Landmark)＋ WIP 欄位(照片、進度、Defect cause、Action、Remarks…)。
- ✅ **edit 頁（`mwi-wi-edit.html`）＝ create 的欄位母版**(2026-07-22):
  - edit 由 create **同一套欄位結構**建成(References／Classification／Location & description／Site & assets 的欄位順序、位置、asset/task 面板 100% 一致),只換成 RMWP-TR388-RRFM-M-17 的既有值。
  - 唯一刻意差異:**Funding 不在 edit**,留在 workflow 頁(edit 底部有一句說明導向)。
- ⭐ **防漂移原則**:**create 頁 = Step 1 欄位的「單一母版」**。edit 與 create 的欄位必須逐欄一致;任何欄位增減/改名/改順序,一律先改此矩陣(1a/1b),再同步套到 **create ＋ edit 兩頁**。不要只改其中一頁。
- ⏳ **待對齊**:
  1. **Task 成長(WIP)**:SS 手機端(`mwi-task-edit.html`)/ workflow 頁需呈現 WIP 的 ~16 欄現場執行欄位,精確欄位/必填/順序仍 🎥 待核。
  2. view 頁保留的 `Start work date`／`End work date`／`Quantity` 等為 🎥 WIP 示意,尚未實機核對;WIP 欄位正式清單待 Arjun 確認後回填。

## 待確認(問 Arjun/Salini)
- Step 2/3/4 與 WIP Task 16 欄的**精確欄位、必填、順序**(目前 🎥 錄影推得)。
- Asset / Task 的**多重數量上限**(Arjun 說 3×3)。
- 各角色對每一步、每個欄位的**可見 / 可編輯**精確權限。
- Element Type / Defect Type 等下拉的**正式值域與友善名**。

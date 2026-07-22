# LTA RMWP — MWI 重設計原型（交接包）

> **專案**：LTA（新加坡陸路交通管理局）RMWP — Road Maintenance Workflow Platform 的 UI/UX review。
> **這個資料夾**：Maintenance Work Instruction（MWI）模組的互動式 HTML 原型 + 規格文件。
> **協作**：Candace ＋ Janet Yu（設計）；交付對象 M Arjun Varma（開發，OutSystems）。
> **文件/UI 語言**：介面文字與交給 LTA 的文件一律**英式拼字**（colour / organise / prioritise）。

---

## ⭐ 工作原則（欄位忠實度）— 動手前先讀

這是這個專案的鐵則,重設計不能違反:

1. **以確認過的實際設計為準。** 欄位、必填、值域以實機截圖 / 錄影核對過的原系統為準,不能憑感覺改。有疑義時,以 `SPEC_field-matrix.md`（角色 × 階段 欄位矩陣）為單一依據。
2. **不亂改既有欄位。** 不刪除既有欄位、不自行新增欄位、不亂改必填星號（＊）。重設計只動 **style / layout / flow / 導覽 / 元件**,不動欄位本身的定義。
3. **要調整欄位集時,先標註再確認。** 若真的需要補齊或改欄位（例如補上原本漏做的 Task 欄位）,先在 `SPEC_field-matrix.md` 標清楚、標上信心來源,並跟 Arjun / Salini 確認後才改頁面——不是靜默改掉。
4. **信心來源要標清楚。** 矩陣裡每個欄位標 ✅ 截圖確認 / 🎥 錄影推得待核;凡 🎥 的都還沒實機逐欄核,改動要保守、待帳號到位再校準。

> 一句話:**可以讓它更好看、更好用、更好走;但不能悄悄改掉「有哪些欄位、誰必填」。**

---

## 這是什麼

Richa Sharma 2026-07 拍板，交付形式為 **HTML files（含 style + flow/layout 改善）**，不是截圖標註。
這裡的每個 `.html` 都是**單檔自足**（inline CSS + 原生 JS，無框架），可直接在瀏覽器開，也刻意寫成 **OutSystems 可移植**的形式（見 `SPEC_design-system.md` 的移植約束）。

設計基底：**SGDS-informed token（10 階色階、語意色）× LTA 品牌**（teal primary、navy secondary、maroon 警示）。政府系統 → 把 accessibility 當隱性標準。四個設計原則：Clarity over density｜Consistency across modules｜Confidence through state feedback｜Considered visual language。

---

## 檔案清單

### 原型（開來看）
| 檔案 | 是什麼 |
|---|---|
| `mwi-list-redesign.html` | WI 列表（工作流版）：To do / In progress / Submitted 分頁、Assign to self、5 狀態色、桌機表格 ↔ 手機卡片、**Filter & sort 抽屜** |
| `mwi-view-redesign.html` | WI 檢視（唯讀）：View/Edit 分離、單頁四區＋sticky nav＋scroll-spy、asset/task 摺疊、資料量 demo（0 / 1 / 3×9 task）、empty state |
| `mwi-wi-create.html` | 新建 WI（Contract Officer）：對齊實際 Add 表單、Site & assets 三欄列、Add/Remove asset、Fund Detail |
| `mwi-wi-edit.html` | 編輯 WI（單頁版） |
| `mwi-wi-workflow.html` | **主打**：角色感知工作流 Edit。右上切換 CO / Site Supervisor / Engineer，整頁跟著變 |
| `mwi-task-edit.html` | Site Supervisor 手機現場填單（照片、進度、sticky 動作列） |
| `logo.png` | LTA 官方 logo |

### 規格文件（改資料時更新這些）
| 檔案 | 內容 |
|---|---|
| `SPEC_design-system.md` | 設計 token、色階、字級、間距、共用元件、英式英文、OutSystems 移植約束 |
| `SPEC_roles-workflow.md` | 角色、隨 stage 長出的 wizard、端到端流程、底部動作分類、列表分頁、指派彈窗 |
| `SPEC_field-matrix.md` | **角色 × 階段 欄位矩陣**：每個欄位的必填＋信心來源（截圖確認/錄影待核）、asset/task 結構、task 欄位隨階段成長、各角色可見/可編輯 |
| `SPEC_data-model.md` | WI 資料盤點（41 支）、asset:task 範圍、值域、對設計的關鍵含意 |
| `SPEC_pages.md` | 逐頁清單：每個 HTML 的用途、狀態、互動、共用元件、導覽串接 |
| `OPEN-QUESTIONS.md` | 要跟 Arjun/Salini/Richa 確認的清單、已知假設、已修的技術坑 |

---

## 怎麼看

1. 直接用瀏覽器開任何 `.html`（Chrome 最準）。全部單檔，不需 build、不需 server。
2. 從 `mwi-list-redesign.html` 進最順：點列表列 → 進 `mwi-wi-workflow.html`（帶角色）。
3. 角色切換在**右上角頭像**，選了會**跨頁保留**（URL 帶 `?role=co|ss|eng`）。
4. `mwi-view-redesign.html` 右上有**資料量切換**（0 / 1 / 3×9 task），示範同一套設計吃不同資料量。

---

## 目前狀態 / 已知限制

- **示意資料**：WI 內容、task 名稱、BOQ 數字、選單選項多為 placeholder。真實 dev 資料見 `SPEC_data-model.md`。
- **角色/動作**：依 walkthrough 錄影推得，**尚未**用 CO/SS/Engineer 帳號實機逐一核對（我們只有 view-only sysadmin 帳號）。
- 未決事項集中在 `OPEN-QUESTIONS.md`。

## 給 Janet 的協作備註

- 每個原型獨立、互不 import，可各自改不會互相弄壞。
- 改了設計決策就同步更新對應的 `SPEC_*.md`，讓文件與原型一致（這正是把規格放進資料夾的目的）。
- 共用視覺 token 的定義在 `SPEC_design-system.md`；目前每頁各自 inline 一份 `:root` 變數，改 token 要每頁一起改（或未來抽成一支共用 css，OutSystems 端再決定）。

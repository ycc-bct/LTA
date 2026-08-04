# OPEN QUESTIONS — 待確認 / 假設 / 已知坑

原型有幾處是依 walkthrough 錄影與 dev 資料推得，尚未實機核對。這裡集中列出，方便找 Arjun / Salini / Richa 一次問清，並記下我們目前採用的假設，讓 Janet 接手時知道哪些是「暫定」。

---

## 1. 要向 Arjun / Salini 確認

1. **asset : task 的實際上限。** Arjun 說 3 asset × 3 task（=9）；dev 實測最多 1 asset × 2 task。設計目前吃 0–9 的整個範圍 —— 需確認上限是否就是 3×3，或更多。
2. **多 asset 的實際上限**（系統有「Add another asset」，但 dev 全是單 asset）。
3. **完整的「角色 × stage × 可用動作」矩陣**（CO、SS、Engineer、QS、DRC）。目前底部動作列是依錄影推的，未逐一實測。
4. **各角色可看到 / 可填的 wizard 步驟權限**（誰在哪一步能編輯什麼）。
5. **測試帳號**：CO / Site Supervisor / Engineer，用來實機校準角色化 demo（我們現在只有 view-only sysadmin）。
6. **縮寫與代碼的正式友善名清單**：RRFM / CFM / BDG / PATH（部門）、REX-… tag、以及超長 Funding Source 代碼 `EXT.ETRO...` 的友善顯示規則。
7. ⭐ **列表頁縮欄的可接受度**（2026-08-04 新增）。原系統 `MaintenanceWorkInstructions` 清單有 **17 欄**，原型原本只做 6 欄且沒留紀錄；現已補上 `Defect type` / `Assign to` / `Last updated`，完整對照見 `SPEC_field-matrix.md` §1d。要問的是：
   - 這 5 欄是真的需要出現在清單上，還是放進 filter／詳情頁就好？
     `CCS WO NO.`、`Instruction Date`、`Defect Reference No.`、`Last Updated By`、`WI Remarks`
   - `Sector NW2 · NW1` 這種寫法可以嗎？（第二個其實是 Sub-Sector，前綴只有一個「Sector」會有歧義）
   - ~~表頭的排序箭頭目前是死的~~ → **已接上**（2026-08-04）：桌機 9 欄皆可點、可反向；手機無表頭，排序留在 Filter 抽屜。
   - 使用者實際掃列表時，最常靠哪幾欄辨識？（決定哪些欄位值得佔桌機寬度）
8. ⭐ **filter 的正式值域字典檔**（2026-08-04 新增）。抽屜已補齊到線上的 11 種 filter type（對照見 `SPEC_field-matrix.md` §1e），但**所有選項值目前都是 placeholder**：
   - 需要 Sector / Sub-Sector / Department / Asset Type / Element Type / Defect Type / **Road Name** 的正式清單。
   - **Sub-Sector 從屬於哪個 Sector**、**Element Type 從屬於哪個 Asset Type** 的對應表（原型的連動目前是從 24 筆假資料推的）。
   - Road Name 實際有幾筆？（決定要不要做非同步搜尋而不是一次載入全部）
   - filter 是否該提供 `Assign to`？線上沒有，但原型的 To Do／In Progress 分頁就是靠它。
   - `Department` 與 `Instruction Date` 這兩種 filter type，**表格上沒有對應欄位**（篩完看不到依據）。原型目前先隱藏 —— 要補成表格欄位、還是只留 filter、還是兩者都不要？

## 2. 要向 Richa 對齊

7. UI/UX 檔案的**集中管理平台**：CodePen vs git repo。`~/code/LTA` 這個資料夾已可直接當 repo 起點。
8. 交付節奏：Quick wins（state、標籤/縮寫、View 模式、預設狀態樣式）先，還是連系統級工作（design system、視覺刷新、表格重構）一起。

## 3. 要向 OutSystems 端（Arjun）確認可行性

9. **兩步 → 單頁分區**、Fund / Price / Approval 多步呈現，OutSystems 端可接受度。
10. 自訂下拉 / date picker 用 `position:fixed` + 高 z-index escape 容器裁切 —— OutSystems 容器層級是否配合。
11. 角色化（側欄 / 動作 / 步驟依角色 stage 變）在 OutSystems 的實作方式（我們原型用 URL `?role=` + JS）。

---

## 4. 我們目前採用的假設（暫定，待上面確認後修正）

- 角色範圍先做 **CO / Site Supervisor / Engineer** 三種（Candace 指定）；QS / DRC 只當「可被 Return/Reassign 的對象」出現，未做各自的頁。
- Wizard 步數 2/3/4 依角色 stage，對應錄影所見；owner step（我要填的那步）= CO→Approval、SS→現場更新、Engineer→Price Schedule。
- 5 狀態語意色的配色是我們定的（見 `SPEC_design-system.md`），非 LTA 既有規範 —— 待對齊。
- WI 內容、task 名稱、BOQ 數字、選單選項多為 **placeholder**；值域見 `SPEC_data-model.md`。
- 手機「Sort by」併入 Filter（Candace 選 option B）。

---

## 5. 已修的技術坑（備忘，避免重踩）

- fixed topbar 遮住捲動定位列 → `scroll-margin-top`；sticky thead 在 `overflow:hidden` 容器會失效。
- 原生 select / date 各瀏覽器不一致 → 改自訂元件統一。
- 下拉 / 日曆被卡片 `overflow` 裁切 → 改 `position:fixed` + 高 z-index。
- `.field>label{display:block}`（權重 0,1,1）蓋掉 `.toggle{display:flex}`（0,1,0）→ 開關縮成 0。修法：`.field>label.toggle{display:flex}` + `.toggle .tk{display:inline-block}`。
- 移除 filter-chips 用了貪婪正則 `.*?</div>\s*</div>` → 刪到表格。改用精準比對 `<div class="filter-chips">\s*<span class="fchip">.*?</span>\s*</div>`。
- 大檔案 base64 logo 是單一超長行（~56K 字元）→ Read 整行會失敗，用 Grep 或 offset 讀。
- 用 Playwright 截圖驗證：`chromium.launch({ executablePath: '/opt/pw-browsers/chromium' })`，腳本要從有 node_modules 的資料夾跑。

---

## 6. 下一步（建議）
- 拿到三種角色測試帳號後，逐一實機核對底部動作列與步驟權限，回填 `SPEC_roles-workflow.md`。
- 跟 Arjun 對 asset:task 上限與友善名清單，回填 `SPEC_data-model.md`。
- 跟 Richa 決定 repo/CodePen，正式把這個資料夾轉成共享 repo。

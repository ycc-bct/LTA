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

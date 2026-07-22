# 給 Janet 的上手說明 — LTA RMWP / MWI 原型

歡迎接手!這份是最快讓你開始改東西的說明。細節都在其他 spec,這裡只講「怎麼動、動哪裡、別踩什麼」。

## 1. 這是什麼
LTA RMWP（Road Maintenance Workflow Platform）的 **MWI 模組 UI/UX 重設計原型**,交付形式是 **HTML**(Richa 拍板),之後給開發 M Arjun Varma。全部是**單檔自足**的 HTML——inline CSS + 原生 JS,無框架、免 build。

## 2. 怎麼開、怎麼改
- **開**:任何 `.html` 用瀏覽器直接打開就好(Chrome 最準),不用裝任何東西、不用跑 server。
- **改**:直接用你習慣的編輯器改 HTML(VS Code 之類),存檔後重新整理瀏覽器就看到。
- **看角色差異**:右上角頭像可切 CO / Site Supervisor / Engineer,整站會跟著變(角色存在網址 `?role=` 參數)。

## 3. 檔案是哪個
| 檔案 | 是什麼 |
|---|---|
| `mwi-list-redesign.html` | WI 列表（To do/In progress/Submitted 分頁、依角色不同、Assign to self、Filter 抽屜） |
| `mwi-view-redesign.html` | WI 檢視（唯讀,asset/task 摺疊、資料量 demo） |
| `mwi-wi-create.html` | 新建 WI ⭐**欄位母版** |
| `mwi-wi-edit.html` | 編輯 WI（欄位跟 create 一模一樣,只是帶值） |
| `mwi-wi-workflow.html` | 角色感知的工作流 Edit（CO/SS/Engineer 各自的步驟與動作） |
| `mwi-task-edit.html` | Site Supervisor 手機現場填單 |
| `logo.png` | LTA logo |
| `README.md` / `SPEC_*.md` / `OPEN-QUESTIONS.md` | 規格文件（見下） |

## 4. ⚠️ 動手前一定要知道的三條規則
1. **先讀 `README.md` 最上面的「工作原則」和 `SPEC_field-matrix.md`。** 欄位、必填、值域以確認過的實際系統為準,不能憑感覺加/刪欄位或改必填星號(＊)。
2. **create 和 edit 的欄位是綁定的。** `mwi-wi-create.html` 是欄位母版,`mwi-wi-edit.html` 必須逐欄一致。**改欄位就兩頁一起改**,並同步更新 `SPEC_field-matrix.md`。只改一頁 = spec 跑掉。
3. **配色/間距 token 目前每頁各自 inline 一份。** 改 `:root` 裡的顏色或尺寸,要**每一頁都改**,不然頁面之間會不一致。(單頁的文案、單頁的樣式微調最安全,可以放心改。)

## 5. 規格文件在哪
- `README.md` — 總覽 + 工作原則(鐵則)
- `SPEC_design-system.md` — 顏色/字級/元件 token
- `SPEC_field-matrix.md` — **角色 × 階段 欄位矩陣**(改欄位的唯一依據)
- `SPEC_roles-workflow.md` — 角色、工作流、動作
- `SPEC_data-model.md` — 41 支 WI 真實資料盤點
- `SPEC_pages.md` — 逐頁說明
- `OPEN-QUESTIONS.md` — 待跟 Arjun / Salini / Richa 確認的清單

## 6. 一起改、不要互相蓋掉(git)
這個資料夾是 git repo。基本流程:
```
git pull            # 開始前先拉最新
# ...改檔案...
git add -A
git commit -m "說明你改了什麼"
git push            # 推上去(第一次需要設定遠端,問 Candace)
```
- **開工前先 `git pull`**,收工 commit + push。
- 盡量**不要跟 Candace 同時改同一支檔案**;真的要,先講一聲或分檔案。
- commit 訊息寫清楚改了什麼,方便追。

有疑問先看 `OPEN-QUESTIONS.md`,或直接問 Candace。祝順利 🙂

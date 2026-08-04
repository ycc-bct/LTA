# SPEC — 設計系統（token / 元件 / 移植約束）

基底：**SGDS-informed × LTA 品牌**。所有原型頁面各自 inline 一份 `:root` 變數，以下為單一事實來源。改 token 時各頁一起改（或未來抽成共用 css）。

---

## 1. 色彩 token

### Teal（primary，對齊 LTA 主色）
```
--teal-50:#E9F5F5  --teal-100:#CDE9E9  --teal-200:#A3D7D8
--teal-300:#6FBFC1 --teal-400:#3FA6A9 --teal-500:#1B8D91
--teal-600:#00747A --teal-700:#005F64 --teal-800:#074A4E --teal-900:#0A393C
```
主色＝`--teal-600 #00747A`。primary 按鈕、連結、focus ring、active nav 都用它。

### Navy（secondary）
```
--navy-50:#EEF2F8  --navy-100:#DCE4F0  --navy-200:#B9C9E0
--navy-300:#8FA7C8 --navy-400:#6485AD --navy-500:#426690
--navy-600:#2B4E77 --navy-700:#1B365D --navy-800:#142848 --navy-900:#0E1D35
```
標題、section nav active 底、群組標籤、CO 頭像色。

### Grey（中性）
```
--grey-25:#FCFDFD --grey-50:#F7F9FA --grey-100:#EFF2F4 --grey-200:#E2E7EA
--grey-300:#CBD4D9 --grey-400:#9DAAB2 --grey-500:#6E7E88 --grey-600:#51616B
--grey-700:#3A4952 --grey-800:#27333B --grey-900:#161F26
```
頁底 `--grey-50`；卡片 `#fff`；框線 `--grey-200`；內文 `--grey-800`；次要字 `--grey-500`。

### 語意色（狀態、回饋）
```
success  --success-50:#EAF5EE  --success-600:#177245  --success-700:#0F5C37
warning  --warning-50:#FEF3E2  --warning-600:#A15C07  --warning-700:#7A4506
danger   --danger-50:#F9ECEB   --danger-600:#A93A32   --danger-700:#8F2D2B  (LTA maroon)
info     --info-50:#EBF1FB     --info-600:#175CD3      --info-700:#1247A4
```
**maroon `#8F2D2B` 保留給警示/危險**，不當一般裝飾色。

## 1b. ⭐ Slate 變體（原「Green」版）2026-08-04

> 主管意見：**綠色少一點,多帶一些官網上的顏色**。原本的 teal 滿版側邊欄改成中性 chrome,
> 主色換成官網自己的連結色,狀態色改用 LTA 官方 accent。**綠色只剩 closed 狀態的萊姆(約 4%)**,
> 故版本代號由 **Green 改為 Slate**(三頁切換器、`<title>` 已同步)。
> 套用檔案:`mwi-list-redesign-a.html`、`mwi-wi-workflow_layouta.html`。
> **原版與 `mwi-wi-workflow_layout3b.html` 不動**,仍用上面的 teal 色階。

### 取樣來源(2026-08-04,實際量測非目測)
| 來源 | 方法 | 值 |
|---|---|---|
| LTA 官網連結/導覽色 | computed styles,出現 **143 次** | `#5C6BBC` |
| 官網 CTA 按鈕 | computed styles | `#6A758A` |
| 官網頁面底 / 內文墨 | computed styles | `#F4F5F8` / `#343A40` |
| 官網 accent `.bg-color1..5` | Featured Projects 卡片漸層 | 見下 |
| logo 三色 | `logo.png` 530×155 逐像素量化 | `#243888` 靛藍 2483px / `#108C98` 湖水藍 2083px / `#882428` 楓紅 1056px |

> ⚠️ **官網介面本身完全沒有用到 teal/green** —— 導覽、按鈕、底色全是藍紫與灰藍。
> 這是「綠色少一點」在資料上的依據,不是主觀偏好。

### Primary — periwinkle(token 名沿用 `--teal-*`,避免改動 ~40 處用法)
```
--teal-50:#F1F1F6  --teal-100:#E1E2ED  --teal-200:#C5C9DD
--teal-300:#9AA3D5 --teal-400:#7B87C9  --teal-500:#6E7CC4
--teal-600:#5C6BBC ← 官網原色,primary 按鈕(白字 4.9:1)
--teal-700:#4453A4 ← hover / active 文字(6.96:1)
--teal-800:#384488  --teal-900:#2B3569
```

### Chrome — 中性,不帶語意
```
--chrome-600:#5E6A78 --chrome-700:#46505A
--chrome-800:#343A40 ← 側邊欄底(＝官網內文墨色,白字 11.3:1)
--chrome-900:#272C32 ← 側邊欄右框線
```
`nav-sub.active` 用**白底＋`--teal-700` 字**(8.93:1)。
實心 periwinkle 貼在 `#343A40` 上只有 **2.35:1**,不夠,且無解 —— 要達 3:1 就得亮到讓白字掉到 3.84:1。

### 5 狀態 — LTA 官方 accent,語意未被犧牲
| 狀態 | LTA 來源 | 底 / 字 | 對比 |
|---|---|---|---|
| Draft | color5 灰藍 | `--st-draft-bg #EFF1F2` / `--st-draft-ink #3F4852` | 8.20:1 |
| Work in progress | color2 珊瑚 | `--st-wip-bg #FBE9E6` / `--st-wip-ink #8E1704` | 7.87:1 |
| Verification in progress | color1 黃 | `--st-ver-bg #FBF6E6` / `--st-ver-ink #8E6B04` | 4.57:1 |
| Approval in progress | color4 藍紫 | `--st-appr-bg #E8E8FA` / `--st-appr-ink #100D84` | 12.28:1 |
| Approved & closed | color3 萊姆 | `--st-done-bg #F4F8EA` / `--st-done-ink #5B761B` | 4.80:1 |

⭐ **先前擔心的「品牌一致 vs 語意優先」兩難,在這組色上不用二選一** ——
LTA 的 color1–5 天然湊齊了中性/完成/審查中/待處理,綠＝完成、黃＝審查中的直覺還在。
WIP 刻意用珊瑚而非藍紫,是為了避開與 primary periwinkle(230°)色相太近(241°)。

### danger
`--danger-600` 由 `#A93A32` 校準到 **logo 楓紅 `#882428`**(白底 9.01:1),`--danger-700:#6E1D20`。

### 角色識別色(頭像 `.rs-av` ／ timeline 角色徽章 `.rb`)
| 角色 | token | 值 | 色相 | 白字 | 由來 |
|---|---|---|---|---|---|
| Contract Officer | `--role-co` | `#10757E` | 185° | 5.43:1 | logo 湖水藍 —— 主導角色用品牌錨點色 |
| Site Supervisor | `--role-ss` | `--` `#AD210B` | 8° | 7.00:1 | color2 珊瑚 ＝ 它負責的 Work-in-progress |
| Engineer | `--role-eng` | `#866709` | 45° | 5.31:1 | color1 黃 ＝ 它負責的 Verification |

- **SS / Eng 的頭像色 ＝ 它們負責階段的狀態色**,不是隨機分配;CO 同時負責 Draft 與 Approval 兩階,
  無法對到單一狀態,故用 logo 湖水藍當錨點(也保留一點 logo 的綠,但只佔一個 34px 圓)。
- 三者色相 185°／8°／45° 都**遠離 primary periwinkle(231°)**,頭像不會被誤讀成可按的按鈕。
- 未指定角色時 `.rs-av` 預設 `--chrome-700`(中性),原本是 `--navy-600`。

### 三個版本的區隔
| 版本 | 側邊欄 | primary |
|---|---|---|
| 原版 | 白 `#fff` | teal `#00747A` |
| **Slate** | **中性墨 `#343A40`** | **periwinkle `#5C6BBC`** |
| Navy | 靛藍 `#273B8A` | 靛藍 |

比較用的對照頁保留在 `sidebar-compare.html`(A–F 六案,含取樣色票與對比值)。

---

### 5 狀態語意色（原版／layout3b 用；Slate 變體見 §1b）
| 狀態 | class | 底 / 字 |
|---|---|---|
| Draft | `.chip-draft` / `.st-draft` | warning-50 / warning-700 |
| Work in progress | `.chip-wip` / `.st-wip` | info-50 / info-700 |
| Verification in progress | `.st-ver` | teal-50 / teal-700 |
| Approval in progress | `.chip-appr` / `.st-appr` | navy-50 / navy-700 |
| Approved & closed | `.st-closed` | success-50 / success-700（或 grey） |

---

## 2. 字級 / 字體
```
--font-sans: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif
--font-mono: ui-monospace, SFMono-Regular, "SF Mono", Menlo, Consolas, monospace
--fs-xs:.75rem  --fs-sm:.8125rem  --fs-base:.875rem  --fs-md:.9375rem
--fs-lg:1.0625rem  --fs-xl:1.375rem  --fs-2xl:1.75rem
```
內文 base 14px。程式碼/代碼（REX-…、tag）用 mono，讓 raw code 一眼可辨。

## 3. 圓角 / 陰影 / 版面
```
--r-sm:6px  --r-md:10px  --r-lg:14px  --r-pill:999px
--shadow-sm:0 1px 2px rgba(22,31,38,.06)
--shadow-md:0 2px 8px rgba(22,31,38,.07),0 1px 2px rgba(22,31,38,.05)
--shadow-lg:0 8px 24px rgba(22,31,38,.10)
```
- Topbar 高 72px，`position:fixed`；logo 52px（手機 42px）。
- 側邊選單白底、右框線；active item teal-50 底＋teal-700 字。
- focus ring：`2px solid teal-600` + offset 2px（無障礙）。

---

## 4. 共用元件（全站一致）

- **角色切換器 `.rolesw`**（右上，六頁一致）：頭像三色區分
  `.rs-co` = navy-600｜`.rs-ss` = `#C2410C`（橘）｜`.rs-eng` = `#7C3AED`（紫）。**避開綠色**（綠＝toast/success 語意）。選單精緻、無 LTA/Contractor tag、無 hint banner。選角色 → `?role=` 跨頁保留。
- **自訂下拉 `.xselect`**：取代原生 `<select>`。`position:fixed; z-index:9999` 永遠最上層、不被卡片裁切。hover teal-50、選中 teal-50＋粗體。以 progressive enhancement 升級 `select.control`。
- **自訂 date picker `.xdate`**：LTA teal 月曆、上/下月、Today/Clear、fixed 最上層。顯示友善格式「5 Jun 2026」。
- **Toggle 開關 `.toggle`**：注意 `.field>label.toggle{display:flex}` 才不會被 `.field>label{display:block}` 權重蓋掉（見坑）。
- **狀態 chip**：5 色如上。
- **Toast**：成功動作（如 Assign to self）跳藍色 toast。
- **Empty state**：空資料用品牌插圖 + 一句說明，不要留白像壞掉。
- **按鈕**：primary＝teal-600 填色；hover teal-700；ghost＝邊框；文字鈕＝tertiary。動作鈕右對齊。
- **View vs Edit 分離**：唯讀畫面用 label-value 排版（空值顯示灰字「Not set」），**不**複用可編輯表單元件（disabled 框、必填星號、Select… 佔位）。

---

## 5. OutSystems 移植約束（寫原型時已遵守）

1. **純 CSS custom properties** 做 token → 對應 OutSystems theme 變數。
2. **原生 `<details>`** 做摺疊（asset/task 手風琴），無框架依賴。
3. **最小 vanilla JS**，無 build step、無 npm 依賴；每頁單檔自足。
4. 自訂下拉/日曆用 `position:fixed`＋高 z-index，避免被容器 `overflow:hidden`/stacking context 裁切 —— 移植時注意 OutSystems 的容器層級。
5. 無 localStorage/sessionStorage；狀態走 URL param（`?role=`）或記憶體。

## 6. 英式英文（一律遵守）
colour / organise / prioritise / centre / labelled…。UI 字串與交付文件都用英式拼字。

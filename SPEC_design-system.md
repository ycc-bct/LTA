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

## 1b. ⭐ Iris 變體（Green → Slate → Iris）2026-08-04 起

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
--teal-600:#5C6BBC ← 官網原色,primary 按鈕(白字 4.90:1)
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
| **Iris** | **淡紫 `#F4F5FC`**（2026-08-05 前是中性墨 `#343A40`） | **periwinkle `#5C6BBC`** |
| Navy | 靛藍 `#273B8A` | 靛藍 |

比較用的對照頁：`sidebar-compare.html`（A–F 六案，中性 chrome 時期）、
`sidebar-slate-compare.html`（五個較淺的中性底）、
`palette-light-compare.html`（**採用的這一版**，以 `#5C6BBC` 為主色的四個清爽方向，選了 D）。

### 1b-2. Iris chrome —— 淺色側欄（2026-08-05 取代中性墨）
主管要「清爽輕盈一點、但不要單調」。`#343A40` 的白字對比 8.36:1，門檻只要 4.5 ——
亮度全花在安全上，這就是它顯重的原因。改法是把側欄換成**主色的一層淡霧**，
再用 LTA 自己的 accent 幫常用區塊上色，避免淺色變成一片灰。
```
--chrome-bg:#EDEFF9      側欄底（主色 #5C6BBC 的極淡階，2026-08-06 由 #F4F5FC 再深一階）
--chrome-bd:#DDE1F2      右框線 / 分隔線
--chrome-ink:#3F4652     導覽主項      8.29:1
--chrome-ink-sub:#51616B 導覽子項      5.60:1
--a-teal:#10757E · --a-coral:#AD210B · --a-amber:#866709 · --a-lime:#5B761B
```
⚠️ 子項原本是 `#6E7E88`，在 `#F4F5FC` 上只有 **3.66:1**（我一度誤記成 4.55）——
子項是 13px，門檻 4.5，底色再深一階後更糟，所以一併換成 `--grey-600`。
logo 用 **`logo_color.png`**（淺色 chrome 上用彩色版，不需要白色版）。
- **作用中項目改成白色浮起的膠囊**（`background:#fff` ＋ 1px 陰影）——
  淺底上要靠「浮起」而不是「加深」來表示選取。
- **九個區塊各有一個 icon 色**，全部 ≥4.5:1（在 `#F4F5FC` 上）：
  | 區塊 | 色 | 來源 | 對比 |
  |---|---|---|---|
  | Overview | `#10757E` | logo 湖水藍 | 4.99 |
  | Work instruction | `#5C6BBC` | primary | 4.51 |
  | Dashboard | `#AD210B` | color2 珊瑚 | 6.44 |
  | Reports | `#866709` | color1 琥珀 | 4.88 |
  | User | `#5B761B` | color3 萊姆 | 4.77 |
  | Sector | `#243888` | logo 靛藍 | 9.68 |
  | Contract | `#882428` | logo 楓紅 | 8.29 |
  | Asset inventory | `#0F5C37` | 深青綠 | 7.41 |
  | Configuration | `#46505A` | 石板中性（設定慣例用中性） | 7.56 |
  最初只給前五個上色、其餘留 `--grey-400`，但**淺底上的淡灰會被讀成「還沒改」**而不是
  「刻意的第二層」。Work instruction 231° 與 Sector 228° 同色相，靠明度與距離區分 ——
  品牌裡沒有第十個色相。未上色的 icon（Notifications）用 `--grey-600`，淺底才站得住。
  用 `data-sec="…"` 掛在 nav item 上，不要靠 `nth-of-type`（`.sb-brand` 也是 div，會算錯）。
- `.nav-item.active` **不加陰影**（只有子項的白色膠囊留 1px 陰影，那是唯一標示所在位置的東西）。

### 1b-3. 列表表格配色（2026-08-06，兩個 list 頁都套）
- **表頭改主色系**：Iris `#d6ddfb` 底 ＋ `--teal-900 #2B3569` 字（8.59:1）；
  原版維持 `--teal-50` 底 ＋ `--teal-800` 字 —— `#d6ddfb` 是 `#5C6BBC` 的淡階，屬於 Iris 家族，
  套到還是青綠色系的原版會打架。sticky 的 `th.act-col` 要跟著換，否則捲動時會露出舊底色。
- **列斑馬紋**：偶數列鋪 **`#fcfcef`**（奶油白，兩版共用；對白底 1.03:1）。
  比先前的 `#F3F3E0`（1.12）更輕，是「隱約有條紋」而不是「這列被標記了」；
  內文 `--grey-800` 11.13:1、副標 `--grey-600` 5.33:1，都還很寬裕。
  - **`.zebra` 是 `render()` 掛上去的，不能用 `tr:nth-child(even)`** —— 列會依 bucket／
    filter／分頁被隱藏與重新 append，DOM 位置跟讀者實際看到的順序無關，用 nth-child 會出現
    連續兩列同色。`page.forEach(function(tr,i){…toggle('zebra', i%2===1)})` 才對得上。
  - sticky 的 `td.act-col` 有自己的白底，**必須一起吃斑馬色**，否則它會浮在有色列上。
- **hover 是「疊一層」，不是「換一個顏色」**：
  ```css
  .wi-table tbody tr:hover td,.wi-table tbody tr:focus-within td{
    background-image:linear-gradient(rgba(43,53,105,.08),rgba(43,53,105,.08))}
  ```
  疊的是該版自己最深的品牌墨（Iris `43,53,105` ＝ teal-900；原版 `10,57,60`）8%，
  用 **`background-image`** 疊在既有的 `background-color` 之上 —— 白列與斑馬列各自往下踩
  **同樣的 1.15:1**，所以 hover 不會把條紋抹平，也不必為每種底色各寫一條 hover 規則。
  - ⚠️ **上面每一條底色都要寫 `background-color`，不能用 `background` 簡寫** ——
    簡寫會把 `background-image` 一起重設成 `none`，veil 就消失了。
  - `td.act-col` 是 td，通用規則就蓋得到，不必再寫 sticky 專用的 hover。
- **hover 時 status chip 轉白底**（`tr:hover .chip{background:#fff}`），文字與圓點維持狀態色。
  chip 的底全是粉彩，壓在 veil 過的列上一定會有一個糊掉。
- **所有 chip 加 1px 內陰影 `rgba(22,31,38,.14)`**（不是 border，避免 2px 的版面位移）。
  ⚠️ 這條是必要的，不是裝飾：**五個 chip 底對任何一種淺色斑馬底，邊界對比都在 1.01～1.14 之間**，
  沒有描邊就會有一顆糊掉。換底色只會換成哪一顆糊掉 ——
  `#FCF8EB` 是 ver（1.02，因為 `--st-ver-bg` 就是同一支黃的 18%）、
  `#F3F3E0` 是 draft（1.01）、`#fcfcef` 是 ver（1.05）。所以描邊留著。
- `.cell-sub` 由 `--grey-500` 改 `--grey-600`：12px 在白底本來就只有 4.20:1，加了斑馬底更低。
- 側欄夠淺，**彩色 logo 直接用 `logo_color.png`**，不必再維護 `logo_white.png`。手機 topbar 同色。
- 通知紅點改 `--a-coral #AD210B`（淺底 6.44:1）；`--danger-on-dark` 只剩 Navy 版在用。
- ⚠️ **純 `#5C6BBC` 不能當側欄底**：白字只有 4.90:1，次項目一降透明度（84%）就掉到 3.98，
  整條導覽會被迫同一個明度。要做「側欄是一塊主色」得用 `#4E5DB0` 以上。

---

### 5 狀態語意色（原版／layout3b 用；Iris 變體見 §1b）
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

### 2b. 標題階層（2026-08-05 重新定義，Original ＋ Iris 五頁）
```
--fs-h1:1.25rem      /* 20px  頁標題      —— 上限，不得超過 */
--fs-h1-sm:1.125rem  /* 18px  同一標題在手機 */
--fs-h2:1rem         /* 16px  區塊／卡片／抽屜標題 —— 上限，不得超過 */
```
**任何讀起來像頁標題或區塊標題的東西一律引用這三個 token，不要寫死 rem。**

| | 頁標題（H1） | 區塊標題（H2） |
|---|---|---|
| 列表頁（兩版） | `.page-head h1` | `.card-head h2`／`.fd-head h2`；手機卡片 `.wi-card .road` 同級 |
| workflow Original／Stacked | `.context-bar .ctx strong`（WI 號） | `.card-head h2` |
| workflow Iris | `.b-top h1`（WI 號） | `.sec-title` |

改動前的實測與問題：

| 頁 | 原 H1 | 原 H2 | 問題 |
|---|---|---|---|
| 列表（兩版） | 28px | 17px | H1 超過上限 8px |
| workflow Original／Stacked | **15px** | 17px | **階層反了** —— 頁標題比區塊標題小 |
| workflow Iris | 22px | 22px | **完全沒有階層**，且兩者都超標 |

例外（都在上限內，刻意保留）：
- Iris workflow 手機的 `.b-top h1` 用 **16px** 而非 18px —— 那條 bar 是單行 `nowrap`，
  裝的是完整 WI 號，18px 會被截掉。
- `.context-bar .ctx strong` 的第二條規則**刻意不分斷點**（註解寫著 one layout at every width），
  所以手機字級要另外開一條 `@media(max-width:1020px)`，直接寫進那條規則會讓桌機也變 18px。

### 2c. 標題文字＝導覽文字（2026-08-05）
wizard step、section jump chip（Original）／rail sub（Iris）與它們指到的區塊標題**必須逐字相同**。
七個名稱是唯一來源，三頁共用：

```
Work instruction · Location · Site & assets · Attachment · Fund Detail · Price Schedule · Approval
```
step 1 在兩版都叫 **Work / Task / Site Detail**（它涵蓋前四個區塊，不對應單一標題）。

改動前的落差 —— 導覽說一套、標題說另一套：

| 導覽 | 原本的標題 |
|---|---|
| Work instruction | Work instruction ~~details~~ |
| Location | Location ~~& description~~ |
| Fund Detail | ~~Funding~~ |
| Price Schedule | Price ~~s~~chedule |

方向是**讓標題去對齊導覽**，因為導覽名稱兩版本來就一致、也貼近線上 wizard 的用字。
這些字串沒有被任何 JS 當 key（查過），純顯示文字。

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

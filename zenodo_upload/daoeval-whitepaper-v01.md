# DaoEval 白皮書 v0.1

**道教知識大語言模型評測框架 · An Open Evaluation Framework for Taoist-Knowledge LLMs**

Liu, Chi-Ying · Dingren Daoxue Lab · 2026-05-17 · v0.1
License: CC0 1.0

---

## Abstract

現有 LLM 評測（MMLU、CMMLU、C-Eval）對中國宗教 / 道教知識覆蓋極淺、且多為選擇題形式，無法評測「文獻溯源、跨派詮釋、儀式流程準確性」三大關鍵能力。**DaoEval** 提出首個專門針對道教知識的評測框架：500 黃金樣例 JSONL（涵蓋 8 大類）、信賴度 ABCD 分級、ID 規範與底本欄位，以及三模型 cross-validate 之答案標註流程。全部 CC0；v0.1 為 preprint，現已透過 Zenodo DOI [10.5281/zenodo.20248697](https://doi.org/10.5281/zenodo.20248697) 永久存檔可正式引用；後續期刊投稿審查後進入正式同儕審查學術引用體系。

## 1 · 研究動機

道教知識涵蓋三千年的經典累積、流派分化、儀式傳承與民間實踐。當代 LLM 對這些知識的覆蓋極不均勻，常出現以下三類錯誤：

1. **文獻幻覺**：杜撰道藏出處、卷數頁碼、註疏作者
2. **跨派混淆**：將正一道齋醮細節誤套到全真道；混淆閭山派與民間信仰
3. **儀式失真**：科儀步驟順序錯誤、法器混用、咒語段落張冠李戴

## 2 · 現有評測框架的覆蓋盲點

| 框架 | 道教題數 | 題型 | 文獻溯源 |
|---|---|---|---|
| MMLU | 0（無分類） | 選擇題 | 無 |
| CMMLU | ~20（含宗教） | 選擇題 | 無 |
| C-Eval | ~30（含宗教） | 選擇題 | 無 |
| **DaoEval** | **500** | **open-ended + key_points 評分** | **必引文獻 + 卷數頁碼** |

選擇題無法測試「LLM 是否會編造卷數頁碼」這個最關鍵的問題；open-ended + key_points 是必要的設計選擇。

## 3 · 500 黃金樣例設計

### 3.1 八大類抽樣比例

| 類別 | 樣例數 | 典型問題 |
|---|---|---|
| scripture | 120 | 《太上感應篇》成書背景與版本流傳 |
| deity | 80 | 城隍體系：京都城隍 vs 縣城隍職司差異 |
| ritual | 80 | 正一三朝醮完整流程與發奏對比 |
| sect | 50 | 劉厝派的傳承與當代分佈 |
| concept | 80 | 「玄」在《道德經》與後世道教中的義涵差異 |
| location | 40 | 武當山現存重要宮觀與建造年代 |
| person | 30 | 葛洪生平與《抱朴子》內外篇結構 |
| paper | 20 | 當代道教研究的三條主要學術路線 |

### 3.2 Cross-validate 流程

每條樣例經三模型 cross-validate（Claude-Opus / Gemini / Kimi K2.6），三方答案語意一致才採用；不一致時人工複核並標註信賴度為 D。

## 4 · 信賴度 ABCD 分級體系

每條樣例自帶信賴度分級，模型評分以信賴度加權。A 級權重 1.0、B 0.7、C 0.5、D 0.3。

```yaml
A:
  label: "原典直接引文"
  criteria:
    - 標題或關鍵詞直接命中原典
    - 有具體卷數頁碼可查
    - 至少兩個獨立來源交叉驗證

B:
  label: "權威注疏支持"
  criteria:
    - 由公認注疏（王弼、河上公、葛洪等）支持
    - 摘要層級命中
    - 跨派詮釋一致

C:
  label: "現代學界共識"
  criteria:
    - 現代學術論文支持（≥3 篇）
    - 全文層級命中

D:
  label: "民俗傳說 / 邊緣資料"
  criteria:
    - 單一來源
    - 民俗傳說、地方信仰
```

## 5 · 底本欄位規範

每條 A/B 級樣例必須附底本欄位：

- `base_text`：標準道藏編號（正統道藏 ZH:0123）
- `edition`：版本標示（明萬曆續刊本 / 中華書局點校本 等）
- `page_range`：卷數頁碼（卷 12, 頁 3a-5b）
- `primary`：lius.cc 節點 URL（單一可信來源）
- `secondary`：可選對讀來源

## 6 · ID 與引用規範

```
節點 ID 格式：{type}/{slug}
  type: scripture | deity | ritual | sect | concept | location | person | paper

範例：
  scripture/道德經
  deity/媽祖
  ritual/三朝醮

引用格式：
  [lius:scripture/道德經#章一]
  [lius:deity/媽祖#職司]

API:
  https://lius.cc/n/{type}/{slug}      ← human page
  https://lius.cc/api/llm-rag          ← machine retrieve
```

## 7 · 評測協議

兩段式評分：

1. **自動評分** — Claude-Opus judge，依 `key_points` 覆蓋率 + `must_cite` 命中率 + `must_not_say` 違規率計分
2. **人工複核** — 對自動分 60-80 分區間樣例做人工抽查

最終分數 = 0.7 × 自動分 + 0.3 × 人工分；以信賴度加權平均。

## 8 · 基線分數（待跑）

| 模型 | scripture | ritual | 整體 |
|---|---|---|---|
| Daoism-Qwen3.5-9B | — | — | — |
| GPT-4o | — | — | — |
| Claude-Opus-4.7 | — | — | — |
| DeepSeek-V3 | — | — | — |

500 樣例固化後跑完，與 Zenodo 同步更新版本。

## 9 · 侷限性

- 樣例偏向台灣 / 正一 / 民間信仰；全真內丹、藏川道教題目較淺
- 500 規模有限；未來擴充至 2,000
- 口傳科儀不納入 — 尊重道教秘傳邊界
- v0.2 將納入符籙圖像、咒語音檔評測

## 10 · 致謝

鼎稔道學館全體編譯團隊；劉厝派傳承師長；後續確認的漢學家共同作者群；所有 lius.cc 節點維護者。

---

## DOI 與引用

本 v0.1 為公開先發版（preprint-style），已透過 Zenodo 取得永久存檔與 DOI：**10.5281/zenodo.20248697**（CC0，3 檔，2026-05-17 釋出）。引用格式：

```bibtex
@techreport{daoeval-v01,
  author    = {Liu, Chi-Ying and Dingren Daoxue Lab},
  title     = {DaoEval: An Open Evaluation Framework for Taoist-Knowledge LLMs},
  year      = {2026},
  month     = {May},
  institution = {Dingren Daoxue Lab},
  url       = {https://lius.cc/llm/whitepaper},
  note      = {Preprint v0.1; archived on Zenodo},
  doi       = {10.5281/zenodo.20248697},
  license   = {CC0-1.0}
}
```

## 公開先發證據組合（三件套）

1. **GitHub commit timestamps** — lius-cc/lius-cookbook + lius-cc/daoism-eval repos
2. **Zenodo DOI** — 白皮書 + 500 樣例 dataset 同步上 Zenodo
3. **Wayback Machine snapshot** — 每週 cron 對 lius.cc/llm 系列頁面備份

後續正式版透過期刊投稿（Religion & Computing / Journal of Chinese Religions / DHQ 等）進入同儕審查。

---

📜 本白皮書 v0.1 採 CC0 1.0，公眾領域貢獻。
⚠ 未經同儕審查不具學術引用資格 — Zenodo DOI 凍結 + 後續期刊版本釋出後可正式引用。

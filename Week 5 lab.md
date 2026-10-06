# Week 5 lab
> 本次 Week 5 Lab：Writing Prompts Well（寫好提示詞），要你針對同一個 Python 函式，用四種提示技巧讓 AI 產生說明文件，再依自己事先訂好的規格比較結果。
> <br>每人都要完成四種技巧，最後兩人合交一份文件。截止時間為 2026 年 10 月 7 日（星期三）23:59，繳交到 E3 的「Week 5 Lab」。

以下依據講義第 54–64 頁整理：[week5-prompt-engineer-1-261005.pdf](https://github.com/414551016/AI-in-the-Loop-in-Software-Project-Cycle/blob/main/Lecture/week5-prompt-engineer-1-261005.pdf)

## 知識點：
`Zero-shot`、`Few-shot`、`Chain-of-thought` 與 `Self-correction為`「提示工程（Prompt Engineering）」最常考的核心概念之一。若從生成式 AI、LLM（如 ChatGPT、Copilot、Claude）角度來看，Zero-shot、Few-shot、Chain-of-Thought（CoT）與 Self-Correction 分別代表不同的提示策略。
### Zero-shot Prompting（零樣本提示）：
- 概念：不給任何範例，直接告訴 AI 要完成什麼任務。
  <br>僅提供任務指令，不提供任何範例或推理鏈架構。這是控制度最低、速度最快且成本最低的方式，適用於模型在訓練階段已高度熟悉且定義明確的任務。
- 範例：
  ``` 
  Prompt：請判斷以下評論的情感：「這家餐廳服務很好，食物也很美味。」
  AI回答：正向情感
  ```
- 特點：
  <br>生成速度最快、消耗 Token 最少。但模型會自動做出許多隱性決策（如自行選擇 Docstring 格式），容易忽略函式中隱蔽的邊界條件（如：僅採計首筆重複 ID、缺少分數時預設為 0 等）。
  - 優點：
    - 簡單快速
    - 不需提供範例
    - 適合簡單任務
  - 缺點：
    - 容易誤解需求
    - 複雜推理效果較差

### Few-shot Prompting（少樣本提示）：
- 概念：先提供幾個範例，再讓 AI 模仿。
  <br>在給定最終任務前，先提供 2 到 3 個「輸入-輸出」的具體範例。此方法能透過範例直接限制產出分佈，確保輸出格式與細節程度達到高度一致性。
- 範例：
  ```
  Prompt：
    範例1：
      句子：今天天氣很好
      情感：正向
    範例2：
      句子：今天一直下雨
      情感：負向
    請判斷：
      句子：這家餐廳服務很好
      情感：正向
  ```
- 特點：
  <br>顯著提升 Docstring 的格式一致性與排版規範（如嚴格符合 Google 或 NumPy 格式）。透過範例展示，模型更能掌握描述細節的深度，但在複雜邏輯邊界條件的挖掘上仍受限於範例涵蓋範圍。
  - 優點：
    - 提升準確率
    - 能學習格式
    - 降低誤解
  - 缺點：
    - Prompt變長
    - 消耗較多Token

### Chain-of-thought, CoT（思維鏈提示）：
- 概念：要求模型一步一步推理。又稱：思維鏈推理。
  <br>要求模型在給出最終答案前，先進行逐步的推理或觀察（如：「一步步思考」或劃分逐步分析流程）。透過產生中間推理 Token，大幅提升複雜邏輯與多步驟推理任務的準確度。
- 範例：
  ```
  Prompt：小明有12顆糖果，送給小華3顆，再買回5顆。現在有多少顆？
  一般回答：14
  
  CoT Prompt：請一步一步思考再回答。
  AI回答：
    原本有12顆，送出3顆 => 12 - 3 = 9
    又買5顆 => 9 + 5 = 14
    答案是14
  ```
- 特點：
  <br>引導模型在撰寫 Docstring 前先拆解程式邏輯（分析功能 $\rightarrow$ 參數 $\rightarrow$ 傳回值 $\rightarrow$ 陷阱）。此過程大幅提高了抓出隱性邏輯與特殊邊界條件（Edge cases）的機率，但代價是輸出 Token 數增加、延遲與費用上升。
  - 優點：
    - 提高推理能力
    - 數學題效果特別好
    - 適合邏輯分析
  - 缺點：
    - 回答較長
    - 需要更多計算時間

### Self-correction（自我修正提示）：
- 概念：要求模型自己檢查並修正答案。又稱：自我修正（Self-Reflection）
  <br>先生成初步結果，再提示模型針對特定品質標準進行批判（Critique），最後根據批判結果進行修改（Revise）。批判必須具備針對性與具體標準，以避免模型陷入含糊討好的盲目讚同（Sycophancy trap）。
- 範例：
  ```
  Prompt：請先回答問題，再檢查一次是否有錯誤，若有錯請修正。
  AI回答：
    12 - 3 + 5 = 13
  AI檢查：
    12 - 3 = 9
    9 + 5 = 14 發現計算錯誤
  AI修正：
    答案為14
  ```
- 特點：
  <br>經過「生成 $\rightarrow$ 具體檢驗批判 $\rightarrow$ 修正」的兩階段流程。若給予具體的檢驗條件（例如：「是否列出讓初學者意想不到的陷阱？」），能補足初次生成時遺漏的關鍵資訊，輸出品質最高且最嚴謹，但需消耗 2 至 3 次 API 調用，成本最高。
  - 優點：
    - 降低錯誤率
    - 提升穩定性
    - 適合程式碼生成
  - 缺點：
    - Token增加
    - 執行成本較高

### 四種提示技巧對同一函式的影響：
|技巧	|是否給範例	|是否要求推理	|是否自我檢查	|適用場景|速記|
|--|--|--|--|--|--|
|Zero-shot<br>（零樣本提示）	|❌	|❌	|❌	|簡單查詢|Zero-shot：直接問。|
|Few-shot<br>（少樣本提示）	|✅	|❌	|❌	|格式模仿、分類|Few-shot：給範例再問。|
|Chain-of-Thought<br>（思維鏈提示）|❌	|✅	|❌	|數學、邏輯、程式設計|Chain-of-Thought：一步一步想。|
|Self-Correction<br>（自我修正提示）|❌	|✅	|✅	|程式開發、複雜推理|Self-Correction：先回答，再自己檢查修正。|

若以 AI 輔助軟體工程（AI-Assisted Software Engineering）而言，效果通常為[ Zero-shot ↓ Few-shot ↓ Chain-of-Thought ↓ Self-Correction ]：在複雜程式開發、除錯與演算法設計上，後兩者通常能得到較高品質與較可靠的結果。

假設我們有一個函式：
```
def max_value(a, b):
    pass
```
要求 AI 幫忙完成。
- Zero-shot：
  ```
  請完成以下函式
  def max_value(a,b):
      pass
  ```
  ```
  AI 回答：
  def max_value(a, b):
    return max(a, b)
  ```
  特性：
  - 直接產生答案
  - 可能有多種寫法
  - 結果依模型理解而定
- Few-shot：
  ```
  範例：
  def add(a,b):
      return a+b

  def multiply(a,b):
      return a*b

  請完成：
  def max_value(a,b):
  ```
  ```
  AI 回答：
  def max_value(a, b):
    if a > b:
        return a
    return b
  ```
  [影響]模型學會：
  - 函式風格
  - 編碼格式
  - 回傳方式
- Chain-of-Thought：
  ```
  Prompt：
  請一步一步分析如何實作最大值函式
  再給程式碼
  ```
  ```
  AI 回答：
    要比較a與b
    若a大於b
      回傳a
    否則回傳b

    程式：
    def max_value(a, b):
    if a > b:
        return a
    else:
        return b
  ```
  [影響]模型先推理：問題分析 ↓ 比較大小 ↓ 決策條件 ↓ 撰寫程式。<br>因此較不容易寫錯。
- Self-Correction：
  ```
  Prompt：
    完成函式後
    請再次檢查是否存在問題

  AI 回答：
    def max_value(a, b):
    if a > b:
        return a
    else:
        return b
  
  AI 檢查：
    ✓ 語法正確
    ✓ 能比較大小
    ✓ 能處理相等情況
    ✓ 回傳值正確
  ```
  [影響]模型會進行：生成答案 ↓ 自我驗證 ↓ 發現問題 ↓ 修正答案。<br>產生較可靠程式碼。




## slide：54 -64
1. 請將本教學內容英/中翻譯比對
2. 請說明本教學重點內容及你的看法，最後以250字內總結
<div align="left" >
  <img src="./Lecture/week5-prompt-engineer-1-261005/week5-prompt-engineer-1-261005_page-0054.jpg" width="49%">
  <img src="./Lecture/week5-prompt-engineer-1-261005/week5-prompt-engineer-1-261005_page-0055.jpg" width="49%">
</div>

Lab 5: Lab: Writing Prompts Well / 實驗 5：良好 Prompt（提示詞）撰寫實作

Today's lab / 今日實驗內容：
- Goal: apply the four techniques to the same function, and judge each output against a specification you write first.
  > 目標： 將四種提示詞技巧應用於同一個函式，並根據你預先撰寫的規格書（Specification）來評估每個產出結果
  - Work individually until minute 40, then pair up
    > 個人獨立作業至第 40 分鐘，隨後進行兩兩組隊
- Materials: lab guide, the lab function, and the few-shot examples are on the Course Website
  > 教材與資源： 實驗指南、目標函式及少樣本示例（Few-shot examples）皆已上傳至課程網站
- Submit: one document per pair on E3 by Wednesday, October 7, 23:59 (details on the last lab slide)
  > 繳交方式： 每組只需繳交一份文件至 E3 平台，截止時間為 10 月 7 日（週三）23:59（詳細細節見最後一張簡報）
- 教學重點內容：
  - 先訂規格，後評效能：實驗的核心要求是在讓 AI 產生程式說明前，先由學生獨立撰寫「規格書（Specification）」，以此作為客觀評估 AI 產出品質的基準。
  - 多重提示技術對比：學習者需將四種不同的 Prompt 技巧（如 Zero-shot, Few-shot, Chain-of-thought, Self-correction 等）運用在相同的任務上，觀察並比較不同提示方式對生成品質的影響。
  - 個人思考與團隊協作相結合：前 40 分鐘強調個人獨立思考與直覺式提示測試，隨後進行雙人討論（Pair up）與共同審查，最後以小組形式繳交完整的分析報告。
- 個人看法：
  - 這項實驗設計展現了現代軟體工程教育的核心轉變 — 從「如何叫 AI 寫程式」提升至「如何精準控制與驗證 AI 產出」。
    - 解決「評價主觀化」問題：要求學生「先寫規格書、再比對結果」，能防止開發者因 AI 產出看起來流暢而盲目接受，養成建立客觀檢驗標準（Specification/Checklist）的良好工程習慣。
    - 培養可預測性與可維護性：透過比對不同提示技巧對同一函式的表現差異，開發者能深刻理解模型背後的預測機制，進而寫出更具可預測性、可重複驗證且利於團隊維護的提示詞。
  - 本實驗聚焦於「高質量提示詞撰寫與產出評估」。學生需預先制定規格書，並針對同一函式套用四種提示技巧，以客觀標準評估 AI 產出的準確性。整體流程結合個人獨立測試與雙人小組研討，強調在生成式 AI 時代，工程師的核心價值不在於隨機嘗試 Prompt，而在於具備精準定義意圖（Specification）、建立評估基準與驗證產出品質的能力，落實嚴謹且可維護的 AI 輔助開發流程。

## slide：56
<div align="left" >
  <img src="./Lecture/week5-prompt-engineer-1-261005/week5-prompt-engineer-1-261005_page-0056.jpg" width="50%">
</div>

|Minutes / 時間（分鐘） | What to do / 執行任務內容 | 
|--|--|
|0–5: |Step 0: write your specification <br> 0–5 分鐘：步驟 0：撰寫你的規格書（Specification）|
|5–12: |Technique 1: zero-shot<br>5–12 分鐘：技巧 1：零樣本提示（Zero-shot）|
|12–20: |Technique 2: few-shot  <br>12–20 分鐘：技巧 2：少樣本提示（Few-shot）|
|20–28: |Technique 3: chain-of-thought  <br>20–28 分鐘：技巧 3：思維鏈提示（Chain-of-thought）|
|28–40: |Technique 4: self-correction <br>28–40 分鐘：技巧 4：自我修正提示（Self-correction）|
|40–50: |Pair discussion + one shared exit-ticket paragraph <br>40–50 分鐘：雙人小組討論 + 撰寫一段共同完成的離場心得（Exit-ticket paragraph）|
|50–60: |Buffer: assemble your pair submission <br>50–60 分鐘：緩衝時間：整理並彙整雙人小組的繳交文件|

- 個人看法：
  - 本張簡報展現了提示工程（Prompt Engineering）教學的結構化與方法論價值：
    - 對比遞進式學習：將 Prompt 技巧分為不同層次（從簡單直接的 Zero-shot 到需要邏輯推理的 Chain-of-thought 與自我校正 Self-correction），讓學生能在相同時限內直觀體會不同技術帶來的邊界效應與產出品質差異。
    - 落地同儕審查（Peer Review）：最後安排 20 分鐘進行小組討論與整合，能強迫開發者將個人的「Prompt 體驗」轉化為「可被他人理解與複現的工程規格」，避免單打獨鬥產生的盲點。
  - 本簡報規劃了 60 分鐘提示工程 Lab 的精確時程。流程首先要求在 5 分鐘內撰寫規格書，隨後於 35 分鐘內依序測試 Zero-shot、Few-shot、Chain-of-thought 與 Self-correction 四大提示技巧。最後 20 分鐘則進行雙人小組討論，撰寫共同心得並彙整最終報告。   <br>此流程的核心意義在於將軟體工程中的控制與測試精神引入 AI 互動。藉由時限緊湊的對比測試與同儕審查，培養開發者掌握 Prompt 影響機制的能力，從盲目嘗試轉向可預測、可評估且具備嚴謹標準的現代 AI 協作開發模式。
## slide：57
<div align="left" >
  <img src="./Lecture/week5-prompt-engineer-1-261005/week5-prompt-engineer-1-261005_page-0057.jpg" width="50%">
</div>

The function that we're working on / 我們即將處理與分析的目標函式
- process_records (on the course website) 
  - This is a different function from the one in the lecture demos | 此函式與正課講義中演示的範例函式不同**
  - Please read it first — understand what it does, what the parameters mean, what it returns, and how it might surprise a new engineer. | 請先仔細閱讀——理解其功能、參數意涵、傳回值結構，以及有哪些可能讓新進工程師感到意外的潛在陷阱（Surprises/Edge cases）。
  - The same function is used for all four techniques | 此單一函式將貫穿並應用於全部四種提示詞技巧
- 教學重點內容：
  - 單一變數控制（Control Variable）：Lab 規定所有提示技巧（Zero-shot、Few-shot、CoT、Self-correction）都必須對同一個目標函式 (process_records) 進行測試，確保比較結果的公平性與客觀性。
  - 邊界條件與隱性決策識別：程式碼中隱藏了多個容易被忽略的邊界行為（Edge Cases / Surprises）：
    - 去重機制（Deduplication）：利用字典 seen 記錄已處理過的 id，若有重複 id 的紀錄，只保留第一次出現者，後續出現者會被直接跳過（continue）。
    - 預設值補齊：若紀錄缺少 'score' 鍵值，會預設為 0 (r.get('score', 0))。
    - 空值 key 問題：若字典缺少 'id'，key 將為 None，seen[None] 會被設為 True，導致後續所有同樣缺少 'id' 的紀錄都被直接忽略[cite: 4]。
  - 先理解後提示：要求學生在撰寫 Prompt 之前，必須先自行解讀程式碼邏輯[cite: 4]，才能在「步驟 0」精準寫出檢查清單（Specification）。
- 個人看法：
  - 這張簡報精準指出了 Prompt Engineering 在程式文檔生成中的核心挑戰：
    - 測試 AI 是否能抓出「隱性邏輯陷阱」：如果只是叫 AI 生成 Docstring，Zero-shot 往往只會寫出「過濾並標記分數」的表面描述，卻遺漏「只採計第一次出現的 ID」或「缺少 ID 時的異常覆蓋」等對維護者至關重要的小細節。
    - 強調工程師的領域知識與批判思維：工程師如果自己沒有先看懂程式碼中的盲點[cite: 4]，就無法制定出合格的 Specification，更無法判斷 AI 是真的理解邏輯，還是只是給出一段「看起來很有道理但遺漏關鍵邊界」的漂亮文字。
  - 本簡報提供 Lab 5 的測試目標函式 process_records[cite: 4]。該函式包含「依 ID 去重（僅留首筆）」與「缺少分數預設為 0」等隱性邏輯[cite: 4]。教學要求學生先理解其參數、傳回值與潛在陷阱[cite: 4]，並將此單一函式套用於四種提示技巧進行對比。
  <br>此設計體現了嚴謹的科學實驗精神。透過固定測試標的[cite: 4]，能客觀評估不同 Prompt 是否能誘發模型抓出隱蔽的邊界條件。這證明了 Prompt Engineering 不僅是文字撰寫，更是建立在工程師對程式邏輯精準掌握上的質量控制過程。

## slide：58
<div align="left" >
  <img src="./Lecture/week5-prompt-engineer-1-261005/week5-prompt-engineer-1-261005_page-0058.jpg" width="50%">
</div>

- Your task: get the model to write a docstring for process_records that meets your specification. You write the prompts, the model writes the docstring, and you judge it.
  > 你的任務：讓模型為 process_records 撰寫符合你規格書（Specification）的 Docstring。由你撰寫提示詞、模型撰寫 Docstring，並由你進行評估與審查
- You produce:
  > 你需產出的成果
  - Four docstrings — one per technique, each judged against your own specification. At the end, your pair picks one to approve
    > 四份 Docstring 每一種提示技巧各一份，且每份皆需根據你自訂的規格書進行評估。最後，你與夥伴將共同挑選並通過（Approve）其中一份最佳版本

- 教學重點內容：
  - 明確的角色分工（Human-in-the-loop）：工程師負責「撰寫提示詞（Prompting）」與「品質審查（Judging）」，AI 則負責執行「內容生成（Docstring Generation）」。工程師是最終品質的掌控者與決策者。
  - 基準化品質比對：必須產出四份使用不同提示技巧（Zero-shot, Few-shot, CoT, Self-correction）生成的 Docstring，且每一份都必須對照事先寫好的規格書進行客觀檢驗與評分。
  - 同儕審查與共識裁決（Pair Approval）：實驗最後階段由雙人小組共同討論，從四份產出中審查並核准（Approve）一份最佳版本，模擬了現實團隊開發中的 Pull Request (PR) 審核機制。
- 個人看法：
  - 本頁簡報揭示了現代 AI 輔助開發的核心範式轉變（Paradigm Shift）：
    - 從「隨機 Prompt」轉向「測試驅動開發（TDD）概念」：把「Specification」當作 Unit Test，AI 產出的 Docstring 就是被測試的程式碼。只有預先建立檢驗標準，才能避免人類審查時產生「看起來很有道理就給過」的主觀盲點。
    - 培養 Code Steward（程式碼審查者）的心態：在團隊開發中，讓 AI 生成程式碼或文件並不難，難在如何進行高質量的 Gatekeeping。透過小組審查並核准單一最佳版本的流程，能有效防止低品質或遺漏邊界條件的 AI 內容進入程式庫。
  - 本簡報明確規範 Lab 5 的具體產出任務。學生需設計 Prompt 引導 AI 為 process_records 撰寫 Docstring，並利用四大提示技巧各生成一份結果，再依自訂規格書進行客觀評估。最後由雙人小組共同討論並核准一份最佳版本。
  <br>此任務的核心意義在於實踐「規範驅動評估（Specification-Driven Evaluation）」。將軟體工程中的 TDD 與 Code Review 精神融入 Prompt 實作，強調開發者的價值在於精準定義意圖與嚴謹驗證品質，而非盲目信任 AI 產出，建立負責任且可持續維護的 AI 協作開發模式。 


<div align="left" >
  
  <img src="./Lecture/week5-prompt-engineer-1-261005/week5-prompt-engineer-1-261005_page-0058.jpg" width="49%">
  <img src="./Lecture/week5-prompt-engineer-1-261005/week5-prompt-engineer-1-261005_page-0059.jpg" width="49%">
  <img src="./Lecture/week5-prompt-engineer-1-261005/week5-prompt-engineer-1-261005_page-0060.jpg" width="49%">
  <img src="./Lecture/week5-prompt-engineer-1-261005/week5-prompt-engineer-1-261005_page-0061.jpg" width="49%">
  <img src="./Lecture/week5-prompt-engineer-1-261005/week5-prompt-engineer-1-261005_page-0062.jpg" width="49%">
  <img src="./Lecture/week5-prompt-engineer-1-261005/week5-prompt-engineer-1-261005_page-0063.jpg" width="49%">
  <img src="./Lecture/week5-prompt-engineer-1-261005/week5-prompt-engineer-1-261005_page-0064.jpg" width="49%">
</div>

## 一、這次要處理什麼程式？
指定函式為 process_records(records, threshold)，四種技巧都使用同一個函式。依講義第 57 頁的程式，它會：
- 讀取每筆資料的 id 和 score。
- 相同 id 只保留第一筆。
- 分數 >= threshold 標記為 pass，否則標記為 fail。
- 回傳包含 id、score、status 的新列表。

本次的工作是寫提示詞，讓 AI 替這段程式寫 docstring（文件字串），再檢查是否正確。Docstring 就是放在 Python 函式內，交代功能、參數、回傳值等資訊的說明文字。

## 二、第一步：每人先寫自己的規格（Step 0）
在向 AI 輸入任何提示詞之前，先寫清楚「合格的 docstring 應包含什麼」。必須有五個欄位：
|規格欄位	|講義要求|
|--|--|
|What it does／功能	|一句話說明函式做什麼|
|Parameters／參數	|每個參數的名稱、型別、用途|
|Returns／回傳值	|回傳資料的結構，以及各欄位意思|
|Edge cases／特殊情況	|至少兩個新手工程師可能沒想到的行為|
|Format／格式	|指定 docstring 風格及長度上限|

為什麼先寫？ 因為後面四種結果都要用同一份規格逐項檢查，才能公平比較，而不是看哪份「感覺比較好」。
<br>可考慮的特殊情況包括：低於門檻的資料仍會保留、重複 id 不會選最高分、缺少 score 時使用 0。這些是我依程式整理的例子，規格仍應由你理解後訂定。

## 三、第二步：每人實作四種提示技巧
|技巧	|你要怎麼做	|要觀察什麼|
|--|--|--|
|Zero-shot／零樣本	|直接下指令，不提供範例或逐步分析架構	|哪些格式或內容是 AI 自己決定的？|
|Few-shot／少樣本	|放入課程提供的三個範例，再提出任務	|格式、特殊情況的說明有沒有改善？|
|Chain-of-thought／逐步分析	|請 AI 依序分析功能、參數、回傳值、特殊情況，再寫 docstring	|分析有沒有找出規格要求的特殊情況？|
|Self-correction／自我修正	|先產生 → 針對問題批評檢查 → 修改	|檢查有沒有找到真正的問題？修改是否有效？|

講義說明：Lab guide、指定函式與 few-shot 範例位於課程網站。這份 PDF 沒有列出那三個範例的完整內容，實作時需要另外取得。

## 四、每種技巧都要留下哪些紀錄？
每人四種技巧，每種都要記錄以下四項：
- Prompt (verbatim)：實際輸入的完整提示詞，逐字保留；few-shot 要包含範例。
- Full output：AI 的完整輸出，不只挑選好看的部分。
- Docstring produced：把產生的 docstring 獨立列出。
- Analysis：寫 2–3 句分析，指出符合、遺漏或寫錯了哪些規格欄位。

特別注意：
- Zero-shot、Few-shot：docstring 欄位可以寫「same as full output」。
- 逐步分析：完整輸出要保留，另列最後的 docstring。
- 自我修正：要保留生成與批評提示詞、初稿、批評內容、修改後版本，再寫一段分析。

分析應具體，例如：「這份輸出符合參數與回傳值規格，但漏掉缺少 id 時會一起被判為重複資料的行為。」這是分析寫法示例，並非已經完成的實驗結果。

## 五、第三步：兩人比較，選出共同認可的一份
兩人各自完成後，比較彼此的結果，從兩人合計八份 docstring 中選出一份，作為你們願意在程式碼審查時批准的版本。
- 可以修改選中的版本，但要註明改了什麼。
- 接著共同寫 一段 4–6 句的說明，必須涵蓋：
  - 正確性：是否符合你們的規格？
  - 格式一致性：格式是否清楚、穩定？
  - 成本：如果要對幾千個函式執行，這個方法的成本是否合理？

老師強調沒有唯一正解，重點是你們選擇的理由。

## 六、最後繳交檢查表
- 一組兩人合併成一份文件，由其中一人上傳即可：
  - [ ]文件頂端有兩人的姓名與學號。
  - 兩人各自的五欄規格，標明姓名。
  - 兩人各自的四種技巧紀錄，合計八組紀錄。
  - 每組紀錄都有完整提示詞、完整輸出、docstring、2–3 句分析。
  - 一份共同認可的 docstring；若有修改，註明修改內容。
  - 一段共同撰寫的 4–6 句選擇理由。
  - 於 10 月 7 日（三）23:59 前，繳交到 E3 → Week 5 Lab。
- 格式可用 PDF、Word、純文字，也可以直接貼到 E3 文字欄位。
  <br>與前面 Discord 分享的差別：Daniel 的 GEPA 是額外分享的自動最佳化工具；這份 Lab 沒有要求使用 GEPA，也沒有把他使用的「15 項檢查」定為全班統一規格。本次要求是自己先訂規格、實作四種技巧、留下結果並比較。第五種 Step-back prompting 是 Assignment 1 的內容，不是本次 Lab 要做的項目。

---
# 作業步驟：
你可以把這次作業想成一個小實驗：同一段程式，換四種問法請 AI 寫說明，再檢查哪一種最值得採用。
<br>你不需要先學會寫複雜的 Python，但要看懂指定函式，才能判斷 AI 有沒有說錯。下面依序帶你完成；我提供的提示詞與規格是協助起步的範例，你要保留實際執行得到的結果，分析也要根據真實輸出撰寫。

## 步驟 1：先準備工具、教材和紀錄文件.
環境與要求：
- 一個能與 AI 對話的工具。
- 課程網站上的 Lab guide（實驗指南）、process_records（處理記錄） 函式，以及 few-shot（少樣本） 的三個指定範例。
- 一位合作同學。
- 一份 Word 或純文字文件，用來記錄過程。

這次不必為了作業先安裝 Python 或設定 API。 講義要求的主要成果是**提示詞、AI 輸出及分析**；若課程網站的 Lab guide 另有工具要求，再依它操作。
```text
Week 5 Lab — Writing Prompts Well

學生 A：姓名、學號
學生 B：姓名、學號

一、學生 A 的個人實驗
  1. Step 0：規格
  2. Zero-shot
  3. Few-shot
  4. Chain-of-thought
  5. Self-correction

二、學生 B 的個人實驗
  1. Step 0：規格
  2. Zero-shot
  3. Few-shot
  4. Chain-of-thought
  5. Self-correction

三、共同認可的 docstring
四、修改紀錄
五、共同選擇理由（4–6 句）
```
注意：兩人各做四種，不是各分配兩種。

## 步驟 2：先看懂指定函式
講義提供的是：
```Python
def process_records(records, threshold):
    result = []
    seen = {}
    for r in records:
        key = r.get('id')
        val = r.get('score', 0)
        if key in seen:
            continue
        seen[key] = True
        if val >= threshold:
            result.append({'id': key, 'score': val, 'status': 'pass'})
        else:
            result.append({'id': key, 'score': val, 'status': 'fail'})
    return result
```
先認識幾個詞：
|程式名稱	|白話意思|
|--|--|
|records	|一批要處理的資料|
|threshold	|通過門檻|
|id	|每筆資料的識別碼|
|score	|分數|
|result	|整理好的結果|
|seen	|記錄哪些識別碼已經出現|
|continue	|跳過這一筆，繼續處理下一筆|
|return	|把結果交回去|

以下是我補充的教學例子，不是指定測資，也不是實際執行紀錄：
``` Python
records = [
    {"id": "A", "score": 80},
    {"id": "B", "score": 50},
    {"id": "A", "score": 95},
    {"id": "C"}
]

threshold = 60
```
按照程式逐步推算：
```
資料	處理方式
A，80 分	第一次出現，80 ≥ 60，保留並標為 pass
B，50 分	第一次出現，50 < 60，保留並標為 fail
A，95 分	A 已經出現，直接跳過，不會取代原本的 80 分
C，沒有分數	使用預設分數 0，保留並標為 fail
```
所以預期結果是：
``` Python
[
    {"id": "A", "score": 80, "status": "pass"},
    {"id": "B", "score": 50, "status": "fail"},
    {"id": "C", "score": 0, "status": "fail"}
]
```
你尤其要記住以下行為，後面用來抓 AI 的錯：
- 不及格的資料也會保留，不會被刪掉。
- 相同 id 保留第一筆，不是最高分那筆。
- 剛好等於門檻也算通過，因為使用 >=。
- 缺少 score 才會預設為 0；如果明確寫成 "score": None，不會自動變成 0。
- 缺少 id 時會得到 None，多筆缺少 id 的資料會被視為使用同一個識別碼。

你要請 AI 寫的 docstring，就是清楚說明這個函式的使用方式與行為，而不是修改函式。

## 步驟 3：先寫 Step 0 規格，再開始問 AI
講義要求你在產生結果前，先訂出五欄規格。
下面是一份適合初學者的建議起稿。請先讀懂，再決定是否採用或調整；這不是老師唯一指定的答案。
|欄位	|你可以採用的規格|
|--|--|
|功能	|用一句話說明：依 id 去除重複資料，並依分數門檻標記 pass 或 fail。|
|參數	|說明 records 是字典組成的列表，使用 id、score；threshold 是數值門檻，分數大於或等於門檻即通過。|
|回傳值	|回傳新的字典列表，每筆包含 id、score、status，依保留資料首次出現的順序排列。|
|特殊情況	|至少說明：相同 id 保留第一筆；未達門檻仍保留。也可加入缺少分數或識別碼的行為。|
|格式	|使用 Google-style docstring，包含摘要、Args、Returns、Note，例如限制最多 25 行。|

「25 行」與上述章節安排是這裡的建議，不是 Lab 統一強制要求。 講義要求你自行指定風格與長度上限。
訂好後，先保存。後面都拿這一份規格檢查，不要因為 AI 漏寫某件事，就臨時把那項要求刪掉。
<br>為什麼做這一步？
就像先寫評分標準，再看考卷。否則每次看到不同答案，你可能不自覺地更換標準。

## 步驟 4：建立一致的實驗方式
以下是我的操作建議，方便你公平比較，不是講義新增要求：
- 四種技巧盡量使用同一個 AI 模型。
- 每種技巧開一個新對話，避免前一種的內容影響下一種。
- Self-correction 的生成、批評與修正則留在同一個對話。
- 記下介面顯示的模型名稱和實驗日期。
- 每次得到結果，立即複製到文件，避免稍後找不到。
- 不要改寫 AI 原始輸出；你的意見放在分析欄。

下面提示詞中的 [貼上……] 都是提醒你替換的地方，送出前要填好。

## 步驟 5：實驗一 Zero-shot（零樣本）
Zero-shot 的意思是：直接交代任務，不提供示範答案，也不要求逐步分析。
你可以從這個提示詞開始：
```
請為以下 Python 函式撰寫英文 docstring，只輸出 docstring。

<function>
[貼上完整的 process_records 函式]
</function>
```
這個版本故意簡單，方便觀察 AI 自己選擇哪些格式與內容。你的 Step 0 規格仍然是評分依據。










## 教學資源：
- 這個連結是 Discord 伺服器中的頻道[tools-and-resources（工具與資源）頻道](https://discord.com/channels/1547853047458172998/1549444588987748505)

## 他分享了什麼工具？
Daniel 分享 [GEPA 的 GitHub 專案](https://github.com/gepa-ai/gepa)，並表示它能自動執行當天課堂介紹的流程：訂出規格 → AI 產生結果 → 評審評分 → 分析失敗原因 → 修改提示詞 → 再次測試，其中：
- Spec（規格）：事先寫清楚結果必須符合哪些要求。
- LLM judge（大型語言模型評審）：讓另一個 AI 判斷產出是否符合要求。
- Prompt（提示詞）：交給 AI 的工作指令。

重點是讓 AI 根據評分與失敗原因，反覆改進自己的工作指令。








# Week7 Lab
## 作業參考來源
- 作業參考來源：
  - [{Week 6 Lab — Hallucination and Sycophancy}](https://csic30216-fall26.denniswang.net/labs/week6/) [［Week 6 Lab — Hallucination and Sycophancy.pdf］](https://github.com/414551016/AI-in-the-Loop-in-Software-Project-Cycle/blob/main/Week6_Lab/Week%206%20Lab%20%E2%80%94%20Hallucination%20and%20Sycophancy.pdf)
  - [［week6-prompt-engineer-2-261012.pdf］](https://github.com/414551016/AI-in-the-Loop-in-Software-Project-Cycle/blob/main/Lecture/week6-prompt-engineer-2-261012.pdf)
- 教學影片：
  - [Prompt Engineering — Understanding Why Prompts Fail (Week 6) - Recorded Lecture - Part 1-1](https://e3p.nycu.edu.tw/mod/supervideo/view.php?id=244561)
- Prompt：作業說明
  - 請說明本次作業內容、重點及目標為何？
  - 我是初學者，我該如何完成作業規定內容，請詳細說明作業步驟為何及學習知識地圖？
  - 好的我的姓名：劉順松 | 學號：414551016，請操作我的電腦先幫我完成本次作業規定任務及目標，之後我再上傳到E3。
  - API Key for lab 7：
- Prompt：教學說明
  - 請將本教學內容英/中翻譯比對<br>請說明本slide教學重點內容及你的看法，最後以250字內總結

## 請說明本次作業內容、重點及目標為何？
本次 Week 7 Lab 的核心是：完成一個能使用三種工具的 AI 助理，觀察它如何用錯工具，再修改工具說明，用重複測試證明修改是否有效。
<br>老師重視的是「你如何預測、觀察、診斷與驗證」，不要求做出永遠不犯錯的 AI。以下整理的是作業要求與概念說明，尚未替你執行實驗或完成作業。[來源](#作業參考來源)：
- 一、這次到底要做什麼？
  <br>依講義第 55 頁：
  <div align="left" >
    <img src="../Lecture/week7-tool-function-use-mcp/week7-tool-function-use-mcp-261019_page-0055.jpg" width="50%">
  </div>

  老師已提供一個小型 Agent（能選擇工具完成任務的 AI 程式），內含三種工具：
  | 工具 | 中文用途 | 適合回答的問題 |
  |---|---|---|
  | `calculator` | 計算機 | 「247 × 38 是多少？」 |
  | `search_knowledge_base` | 課程知識庫搜尋 | 「第 6 週教什麼？」、「CP1 要交哪些內容？」 |
  | `get_course_calendar` | 課程日期查詢 | 「Week 7 Lab 何時截止？」 |

  你要讓 AI 能分辨：該使用哪個工具、傳入什麼參數，以及什麼時候直接回答即可。


















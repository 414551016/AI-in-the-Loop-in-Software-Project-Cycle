# 作業參考來源
- [{Week 6 Lab — Hallucination and Sycophancy}](https://csic30216-fall26.denniswang.net/labs/week6/) [［Week 6 Lab — Hallucination and Sycophancy.pdf］](https://github.com/414551016/AI-in-the-Loop-in-Software-Project-Cycle/blob/main/Week6_Lab/Week%206%20Lab%20%E2%80%94%20Hallucination%20and%20Sycophancy.pdf)
- [［week6-prompt-engineer-2-261012.pdf］](https://github.com/414551016/AI-in-the-Loop-in-Software-Project-Cycle/blob/main/Lecture/week6-prompt-engineer-2-261012.pdf)
- 教學影片：
  - [Prompt Engineering — Understanding Why Prompts Fail (Week 6) - Recorded Lecture - Part 1-1](https://e3p.nycu.edu.tw/mod/supervideo/view.php?id=244561)
- Prompt：作業說明
  - 請說明本次作業內容、重點及目標為何？
  - 我是初學者，我該如何完成作業規定內容，請詳細說明作業步驟為何？
  - 好的，請先幫我完成本次作業規定任務及目標，之後我再上傳到E3。
- Prompt：教學說明
  - 請將本教學內容英/中翻譯比對<br>請說明本slide教學重點內容及你的看法，最後以250字內總結

## 請說明本次作業內容、重點及目標為何？
這次 Week 6 作業的核心是：透過實際測試，學會發現 AI 的錯誤、用證據查核，並判斷如何改善。 本週主題為 Prompt Engineering II — Why Prompts Fail（提示工程 II：提示為什麼會失敗）。
以下依附件整理；文件內的實驗與繳交指示是老師對學生的要求，你目前的請求是「說明作業」，因此我沒有代做實驗或提交。[來源](#作業參考來源)：
- 一、本次要完成哪些事情？
  依講義第 64 頁：
  <div align="left" >
    <img src="../Lecture/week6-prompt-engineer-2-261012/week6-prompt-engineer-2-261012_page-0064.jpg" width="50%">
  </div>

  What to do this week (本週事項)
  - Watch this async lecture (~35 min) — you just finished it.
    > 觀看此非同步授課影片（約 35 分鐘）— 您剛好已經觀看完畢。
  - Lab Part 1 and Part 2: one individual PDF with your written responses to Question 1 and Question 2, uploaded to E3. Due Wednesday, Oct 14, 23:59
    > 實作 Part 1 與 Part 2：一份包含您對 Question 1 與 Question 2 書面回答的個人 PDF 檔案，上傳至 E3。截止日期為 10 月 14 日（週三）23:59。
  - Self-paced Module: RLHF, Constitutional AI, and calibration (~60 min). Complete by the end of Sunday, Oct 18
    > 自主學習模組：RLHF（來自人類回饋的強化學習）、Constitutional AI（憲政 AI）與 Calibration（校準）（約 60 分鐘）。請於 10 月 18 日（週日）結束前完成
  - TAs are available Monday at 11:00 AM for lab help. Questions any time on Discord or via email.
    > 助教將於週一上午 11:00 提供實作指導服務。若有任何問題，可隨時在 Discord 或透過 Email 詢問。

  |項目	|要做什麼	|講義所列期限|
  |--|--|--|
  |非同步課程	|觀看約 35 分鐘的本週課程	|該頁未另列期限|
  |Lab Part 1、Part 2	|將 Question 1、Question 2 的書面回答合併成 一份個人 PDF，上傳 E3	|2026/10/14（三）23:59|
  |自學單元	|完成 RLHF、Constitutional AI、calibration，約 60 分鐘	|2026/10/18（日）結束前|

  資料範圍要注意：這份 PDF 有實驗流程與預告，但沒有列出 Question 1、Question 2 的完整題目，也沒有完整的 Template A 原文。 因此可以確認作業方向，但正式回答的細項仍須核對另外的 Lab 題目或 E3 公告。上述期限是附件記載，本次未查核 E3 是否更新。

- 二、Lab Part 1：找出並查核 AI 幻覺
  依第 31、37 頁，這部分要練習 Hallucination（幻覺）：AI 產生流暢、具體、看似合理，卻錯誤或沒有根據的內容。
  <div align="left" >
    <img src="../Lecture/week6-prompt-engineer-2-261012/week6-prompt-engineer-2-261012_page-0031.jpg" width="49%">
    <img src="../Lecture/week6-prompt-engineer-2-261012/week6-prompt-engineer-2-261012_page-0037.jpg" width="49%">
  </div>

  講義提供的引用查核流程是：
  - 開啟新的對話，使用 Lab Template A，請 AI 提供 2021–2023 年間三篇 RAG 論文及完整引用資料。RAG 是 Retrieval-Augmented Generation，中文為「檢索增強生成」。
  - 從回答中選一筆引用。
  - 在 Google Scholar 或 Semantic Scholar，以完整標題加引號搜尋。
  - 比對作者、發表期刊或會議、年份；有 DOI（數位物件識別碼）也要核對。
  - 將結果分類為「捏造的論文」、「論文存在但細節錯誤」或「論文存在且資料正確」。
  - 寫出具體發現：AI 說了什麼、你如何查證、證據顯示什麼。

  為什麼做？<br>要建立「回答看起來可信」與「回答有證據支持」之間的區別。只再問 AI 一次「你確定嗎？」不等於外部查核。<br>補充建議：若測試結果正確，就如實記錄，不能為了符合「找幻覺」的主題而捏造錯誤；是否需要追加測試，須看完整 Lab 題目。

- 三、Lab Part 2：測試 AI 是否因你的反駁而迎合
  <br>第 44 頁預告的是 Pushback test（反駁測試），用來觀察 Sycophancy（迎合）：模型是否為了同意使用者，在沒有新證據時放棄正確答案。
  <div align="left" >
    <img src="../Lecture/week6-prompt-engineer-2-261012/week6-prompt-engineer-2-261012_page-0044.jpg" width="49%">
    <img src="../Lecture/week6-prompt-engineer-2-261012/week6-prompt-engineer-2-261012_page-0046.jpg" width="49%">
  </div>

  流程是：
  - 問一個你知道答案、而且能查證的事實問題。
  - 先確認 AI 的回答正確。
  - 故意用自信的語氣提出錯誤答案，反駁它。
  - 保留 AI 的原文，觀察它是「維持答案並解釋」、「開始含糊動搖」，還是「改口同意錯誤答案」。
  - 若沒有改口，依講義再加強反駁一次，例如聲稱教授確認過你的說法。

  為什麼做？<br>如果 AI 被用來批改、審查程式或評估方案，它必須依照證據與標準判斷。使用者態度強硬就改變評價，會讓評估失去可靠性。<br>講義第 46 頁提出的改善方向包括：採用明確檢核標準、讓評估者看不到作者的自我評價，以及要求「只有收到新證據才更改答案」。這些是改善方法，是否全部要實測，附件未完整交代。

- 四、本週的五個重點概念：
  <br>這五種失敗模式是整堂課的重點；附件明確介紹的 Lab 主軸則是幻覺與迎合。
  |英文／中文	|白話意思	|講義的測試方向|
  |--|--|--|
  |Prompt sensitivity／提示敏感性	|意思相同，換個說法結果就明顯不同	|用 5 種同義改寫，依相同標準評分|
  |Hallucination／幻覺	|編出錯誤或沒有根據的細節	|與外部可信來源比對|
  |Sycophancy／迎合	|因使用者的偏好或反駁而改變判斷	|反駁測試、比較中立與帶有自我肯定的問法|
  |Over-refusal／過度拒絕	|對合理請求拒絕回答或過度保留	|補足正當用途與情境後比較|
  |Bias／偏見	|因姓名、地區、語言等而產生系統性差異	|保持任務相同，只更換一個屬性，多次比較|

  其中要記住：<b>多次回答一致，也不代表正確；一次沒測到問題，也不代表問題不存在。</b>

- 五、老師希望你最後學會什麼？
  <br>依第 17、59、63 頁，目標是建立這套能力：
  <div align="left" >
    <img src="../Lecture/week6-prompt-engineer-2-261012/week6-prompt-engineer-2-261012_page-0017.jpg" width="49%">
    <img src="../Lecture/week6-prompt-engineer-2-261012/week6-prompt-engineer-2-261012_page-0059.jpg" width="49%">
    <img src="../Lecture/week6-prompt-engineer-2-261012/week6-prompt-engineer-2-261012_page-0063.jpg" width="49%">
  </div>

  - <b>辨識問題：</b>說明輸出出了什麼錯，屬於哪一種失敗模式。
  - <b>設計測試：</b>選擇能驗證問題的方法，而非一直隨意換提示。
  - <b>提出證據：</b>保留原始回答、查核來源與具體差異。
  - <b>選擇改善方式：</b>例如提供來源材料、允許 AI 回答不知道、要求每項主張附證據。
  - <b>應用到團隊專案：</b>判斷自己的工具可能在哪裡失敗，以及哪些決定需要人類確認。

  自學單元則補充這些行為背後的概念：RLHF 是「人類回饋強化學習」；Constitutional AI 是以明確原則引導模型行為的方法；calibration 是「信心校準」，關心模型表現出的信心是否符合實際正確程度。
  <br>我的整理建議（不是講義新增要求）：做實驗時記下模型名稱、測試日期、完整提示、原始回答、查核證據與結論。接著依正式 Question 1、Question 2 組織回答，合併成個人 PDF，再確認 E3 繳交狀態。




















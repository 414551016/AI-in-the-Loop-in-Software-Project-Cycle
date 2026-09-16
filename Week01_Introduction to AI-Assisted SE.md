# Week01_Introduction to AI-Assisted SE
- 教學資源：
  - [
Introduction to AI-Assisted SE (Week 1) - Lecture Slides](https://github.com/414551016/AI-in-the-Loop-in-Software-Project-Cycle/blob/main/Lecture/week1-Introduction-260907.pdf)
  - [
人工智慧輔助軟體工程入門（第一週）- 自學模組](https://csic30216-fall26.denniswang.net/self-paced/week1/)
  - [圖解變形金剛](https://jalammar.github.io/illustrated-transformer/)

### Week 1 課堂逐字稿
#### slide：2
<div align="left" >
  <img src="./Lecture/Week01/Week01-02.jpg" width="50%">
</div>
[00:00:38–00:02:27]
> Hi, can everyone hear me?<br>嗨,大家能聽到我嗎?

#### slide：3
<div align="left" >
  <img src="./Lecture/Week01/Week01-03.jpg" width="50%">
</div>
[00:02:30–00:02:33]
> - as the first announcement about the wait list<br>作為關於等待列表的第一次宣佈
> - is that we're not taking manual add drop for this class.<br>也就是說,我們不 手動加降本類。

#### slide：4
[00:02:33–00:02:38]
<div align="left" >
  <img src="./Lecture/Week01/Week01-04.jpg" width="50%">
</div>

> - We apologize for this,<br>我們為此道歉,

#### slide：5
[00:04:17–00:04:23]
<div align="left" >
  <img src="./Lecture/Week01/Week01-05.jpg" width="50%">
</div>

> - Okay, so let's get started with the content <br>好吧,讓我們開始的內容

#### slide：6
<div align="left" >
  <img src="./Lecture/Week01/Week01-06.jpg" width="50%">
</div>

這張投影片是在說明本堂課的 Learning Objectives（學習目標）。核心不是單純學「怎麼寫 Prompt」，而是建立一套 在 AI Agent 時代，工程師如何與 AI 協作、監督與控制的思維。
最重要的主軸可以濃縮成三件事：說清楚要什麼 → 觀察 AI 怎麼做 → 必要時把控制權拿回來
- 1. 工程師在 Agentic Era 的三個責任：specifying intent, evaluating trajectories, and knowing when to take back control
  > - Specifying intent：明確描述意圖
  >   - 工程師必須把真正需求說清楚。
  >   - 不能只說「幫我完成」，而要交代目標、限制、輸出格式、判斷標準。
  >   - 這正是你前面 Lab 1 的 Intent–Output Gap。
  > 例如只寫：Explain this code.
  > <br>模型就必須自行猜測：要多詳細？給誰看？要不要講 recursion？
  > <br>如果改成：Explain this code to a junior engineer who has never learned recursion.
  > <br>你的 Intent 就比較明確。
  > - Evaluating trajectories：評估 AI 的執行軌跡
  > <br>這裡的 trajectory 不只是看最後答案，而是看 AI「一路怎麼做」。例如 AI Agent 要完成一個程式任務：理解需求 ↓ 分析問題 ↓ 決定修改哪些檔案 ↓ 產生程式碼 ↓ 執行測試 ↓ 修改錯誤 ↓ 輸出結果
  > <br>工程師不能只問：最後程式能不能跑？
  > <br>而要問：這就是 trajectory evaluation。
  >   - AI 做了哪些決定？
  >   - 它有沒有誤解需求？
  >   - 是否修改了不該修改的地方？
  >   - 是否用了錯誤假設？
  >   - 是否真的驗證過結果？
  > - Knowing when to take back control：知道何時要取回控制權
  > <br>AI Agent 可以有自主能力，但工程師仍然必須知道：哪些事情可以交給 AI，哪些事情不能讓 AI 自己決定。
  > <br>例如：AI 可以自行：
  >   - 整理程式碼
  >   - 產生測試
  >   - 查找 bug
  >   - 草擬文件
  > 但若涉及：工程師就應該重新介入。
  >   - 刪除重要資料
  >   - 修改 production system
  >   - 安全權限
  >   - 資料庫 migration
  >   - 關鍵架構決策
  >   - 不可逆操作
  > 因此 Agentic AI 的核心不是：「AI 越自主越好」。而是：適當授權 + 持續監督 + 必要時人工接管。
- 2. Understand the course structure, components, expectations, and resources
  > 第二個學習目標是：了解整門課的架構、課程組成、要求以及可用資源。
  > <br>也就是學生需要知道：
  >   - Lab 要做什麼
  >   - Assignment 要做什麼
  >   - Project 如何進行
  >   - 作業如何評分
  >   - GitHub / AI tools 如何使用
  >   - 課程提供哪些資源
  > <br>這點看起來只是行政資訊，但其實老師希望你開始建立：Course workflow / project workflow 而不是每週只完成單一作業。
- 3. Identify a decision the model made without being asked
  > 第三點非常重要：找出至少一個「你沒有要求，但模型自己做出的決定」。這正是本課程一直強調的概念。
  > <br>例如你問：Explain this Python function.
  > <br>AI 可能自行決定：
  >   - 使用專業術語
  >   - 假設你知道 Python
  >   - 假設你知道 recursion
  >   - 用條列式回答
  >   - 說它是 Quicksort
  >   - 不解釋 time complexity
  > 這些都是：Implicit decisions（隱含決策）老師要求你不只發現它，還要進一步問：如果是我，我會做相同決定嗎？
  > <br>例如：
  > <br>AI 的決定：假設讀者已經了解 recursion
  > <br>我的評估：我不會做相同選擇，為我的真正目標讀者是初學者。這其實就是 Human Oversight（人工監督） 的雛形。
- 4. Write your first Prompt Engineering Log
  > 第四點：根據今天的 Lab，完成第一份 Prompt Engineering Log。也就是你前面已經做的 Week 1 Lab。
  > <br>它不是單純把 Prompt 抄下來，而是紀錄：我原本的 Intent ↓ 我寫出的 Prompt ↓ AI 怎麼理解 ↓ AI 做了哪些自行決定 ↓ Output 是否符合需求 ↓ 我修改什麼 ↓ 結果是否改善
  > <br>因此 Prompt Engineering Log 其實是一種：工程實驗紀錄，而不是「聊天紀錄」。

**這一頁真正的核心邏輯**
<br>我建議你把這頁記成下面這個架構：
<div align="left" >
  <img src="./Lecture/Week01/Week01-06-1.jpg">
</div>

**總結：在 AI Agent 時代，工程師的工作不只是「下 Prompt」，而是要清楚定義意圖、監督 AI 的決策與執行過程，並在 AI 偏離目標或風險過高時重新取得控制權。**
<br>[00:04:47–00:05:31]
> - So the learning objective for today is <br>所以今天的學習目標是
> - I want everybody to be able to describe <br>我希望每個人都能描述
> - what we're going to learn for this semester, <br>這學期我們要學什麼
> - the three responsibilities in the agentic area, <br>代理領域的三項責任,
> - era of coding, specifying the intent, <br>規定意圖,
> - evaluating trajectories <br>評估軌跡
> - and knowing when to take back control from the AI. <br>並且知道何時從AI手中奪回控制權.
> - And I also hope that you could understand <br>我也希望你能夠明白
> - the core structure, the components, expectations and the resources. <br>核心結構、組成部分、期望以及資源。
> - So I also want you to identify at least one decision <br>所以我還要你至少確定一個決定
> - that model made without being asked during the demo and evaluate whether you would have made the same choice. <br>演示時未詢問的模型並評估你是否會做出同樣的選擇。
> - And finally, you would be writing your first prompt in this classroom and log it based on today's lab. <br>最後,你會寫你的第一個提示記錄在今天的實驗室上


#### [00:05:32–00:05:36]
<div align="left" >
  <img src="./Lecture/Week01/Week01-07.jpg" width="50%">
</div>

這張投影片是在說明這門課整學期的 Learning Objectives（學期學習目標）。和上一張「Today」相比，上一張是在講單堂課要學什麼；這一張則是在說明整學期修完之後，你應該具備哪些能力。
<br>這三個目標可以理解成三個層次：會使用 AI → 會把 AI 正確整合進軟體工程 → 能評估 AI 對更大系統與社會的影響。
- Articulate and apply effective practices of AI-assisted engineering and prompt engineering in software development<br>在軟體開發中闡述並應用 AI 輔助工程與提示詞工程的有效實踐
  > 重點是：不只知道 Prompt Engineering（提示詞工程）是什麼，而是要能實際運用在軟體開發中。你需要學會如何清楚表達需求、設計 Prompt、使用 AI 協助寫程式、除錯、測試、文件撰寫與需求分析，並判斷 AI 的輸出是否符合工程需求。這裡的關鍵字是 Articulate（闡述） + Apply（應用），也就是「能說明，也能實作」。
- Understand core software engineering concepts…and how to effectively and ethically incorporate AI<br>理解軟體工程的核心概念…以及如何有效且合乎道德地融入 AI 技術
  > 這表示課程並不是「AI 工具使用課」，而仍然以 Software Engineering（軟體工程）為核心。你需要理解需求分析、版本控制、測試、Code Review（程式碼審查）、團隊協作、文件化、軟體品質等基本工程方法，再思考 AI 應該放在哪個位置。另一個重點是 Ethically（符合倫理地），包括隱私、著作權、資料來源、AI hallucination（AI 幻覺）、責任歸屬與安全問題。
- Analyze the broader implications and societal impacts of AI-assisted engineering and its infrastructure<br>分析 AI 輔助工程及其基礎設施的深遠意義與社會影響
  > 這是最高一層。除了會「使用 AI」，還要能分析 AI-assisted engineering（AI 輔助工程） 帶來的長期影響，例如工程師角色是否改變、AI 是否造成過度依賴、責任如何分配、AI infrastructure（AI 基礎設施） 的能源與運算成本、資料中心、安全、偏見、隱私及社會影響。換句話說，老師不只希望你成為 AI 使用者，而是能站在更高層次思考：「導入 AI 之後，整個軟體工程生態發生了什麼改變？」
**總結**
<br>本課程整學期的核心目標，是讓學生從「會使用 AI」進一步成為「能負責任地與 AI 協作的軟體工程師」。首先學習 Prompt Engineering 與 AI-assisted engineering，將 AI 實際應用於程式開發、測試與文件工作；其次理解軟體工程基本原則，並以有效、安全且符合倫理的方式導入 AI；最後提升到系統與社會層次，分析 AI 對工程師角色、基礎設施、隱私、安全、責任與社會的長期影響。核心可歸納為：Apply AI、Integrate AI、Analyze AI。
  > - Level 1：Use AI：<br>學會 Prompt Engineering 與 AI-assisted development。
  > - Level 2：Engineer with AI<br>把 AI 正確、有效、負責任地整合進 Software Engineering。
  > - Level 3：Think about AI systems<分析 AI 工程對技術、組織、基礎設施與社會的影響。>
考試或報告時，建議特別記住三組關鍵詞：Apply → Integrate → Analyze 也就是：應用 AI → 整合 AI → 評估 AI 的影響。


> - So let's take the whole picture of the whole course. <br>因此,讓我們把整個航道的全貌照一照.
> - The semester, we want you to be able to articulate and apply effective practices of AI-assisted engineering and prompt engineering in software development. <br>學期,我們要你講清楚，應用AI輔助工程的有效做法，並迅速進行軟體開發工程。

#### slide：8
<div align="left" >
  <img src="./Lecture/Week01/Week01-08.jpg" width="50%">
</div>

#### slide：9
<div align="left" >
  <img src="./Lecture/Week01/Week01-09.jpg" width="50%">
</div>

#### slide：10
<div align="left" >
  <img src="./Lecture/Week01/Week01-10.jpg" width="50%">
</div>

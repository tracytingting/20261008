---
title: 選擇題測驗卷網站講義（學生版）

---

---
title: 選擇題測驗卷網站講義（學生版）

---

---
title: 選擇題測驗卷網站講義（學生版）
tags: [114程式設計與實習_上學期]

---

# 選擇題測驗卷網站講義（學生版）

學號：＿＿＿＿＿＿＿＿　　姓名：＿＿＿＿＿＿＿＿

> **填寫方式**
> 1. 每個學習都要放：**執行截圖**、**三次問 AI 的提示詞**、**最後採用的程式碼**。
> 2. 問 AI 的提示詞請**逐字貼上**自己實際輸入的內容（不要寫摘要），第一次、第二次、第三次依序記錄。
> 3. 程式碼貼在「點開貼上」的收合區塊裡，貼上**你最後真正採用、而且能執行**的版本。

---

## 學習1：產生一個選擇題測驗卷網站

https://cfchen58.synology.me/115/week4/stage1/

**這個階段的目標：** 用 p5.js 做出一個一次顯示一題、四個選項、答完會顯示對錯與總分的測驗網站（題目先寫在程式裡）。
**這個階段會修改的檔案：** index.html、sketch.js

### 執行截圖

（把截圖拖曳到這裡，或貼上圖片連結）![動畫](https://hackmd.io/_uploads/BJjUo34oMe.gif)


![學習1截圖](請貼上截圖)

### 第一次問 AI

```tex!
使用p5.js撰寫一個選擇題網頁測驗系統，我已經產生一個p5.js專為，請把程式碼為到sketch.js
案內，每條指令都需要加上中文註解，測碧系統題目設定為五題，測驗題目的內容為程式設計
p5.js簡易指令練習測驗，系統採用全螢幕畫布，使用者答錯時，系統會在正確答案選項上，加上d0f4de背景顏色，該選項要上下跳動，答錯的選項採用540b0e背景說色，選項左右移動，選擇題選項共有四個選項，當五題結束後，需要顯示答對的題數，每次顯示一個題目，需要有下一題的按鈕


第二次問 AI
tex

```

### 第三次問 AI

```tex!
（逐字貼上你第三次問 AI 的提示詞）
```

### 程式碼內容

:::info
:::spoiler 點開貼上學習1的程式碼
```javascript=// p5.js「程式設計：p5.js 簡易指令練習測驗」完整測驗系統。 // 說明本程式的測驗主題與用途。
// 本程式可直接貼入一般 p5.js Editor 的 sketch.js 檔案中執行。 // 說明程式的使用方式。

const quizQuestions = [ // 建立五題測驗資料陣列。
  { // 建立第一題物件。
    question: "在 p5.js 中，哪一個函式通常只會在程式開始時執行一次？", // 設定第一題題目。
    options: ["setup()", "draw()", "mousePressed()", "windowResized()"], // 設定第一題四個選項。
    answer: 0 // 設定第一題正確答案索引。
  }, // 結束第一題物件。
  { // 建立第二題物件。
    question: "在 p5.js 中，哪一個函式會持續重複執行來產生動畫？", // 設定第二題題目。
    options: ["size()", "draw()", "fill()", "text()"], // 設定第二題四個選項。
    answer: 1 // 設定第二題正確答案索引。
  }, // 結束第二題物件。
  { // 建立第三題物件。
    question: "下列哪一個指令可以設定圖形的填滿顏色？", // 設定第三題題目。
    options: ["stroke()", "line()", "fill()", "background()"], // 設定第三題四個選項。
    answer: 2 // 設定第三題正確答案索引。
  }, // 結束第三題物件。
  { // 建立第四題物件。
    question: "在滑鼠事件中，mouseX 通常代表什麼資訊？", // 設定第四題題目。
    options: ["滑鼠的垂直座標", "畫布的寬度", "滑鼠的水平座標", "目前的題號"], // 設定第四題四個選項。
    answer: 2 // 設定第四題正確答案索引。
  }, // 結束第四題物件。
  { // 建立第五題物件。
    question: "下列哪一個指令可以在畫布上畫出橢圓形？", // 設定第五題題目。
    options: ["rect()", "ellipse()", "triangle()", "point()"], // 設定第五題四個選項。
    answer: 1 // 設定第五題正確答案索引。
  } // 結束第五題物件。
]; // 結束五題測驗資料陣列。

let currentQuestion = 0; // 記錄目前題目索引。
let selectedOption = -1; // 記錄目前選項索引，負一代表尚未作答。
let score = 0; // 記錄目前答對題數。
let quizFinished = false; // 記錄測驗是否已結束。
let lastInputTime = 0; // 記錄最後一次輸入時間，避免觸控重複觸發滑鼠事件。
let layout = {}; // 儲存響應式版面位置資料。

function setup() { // 定義 p5.js 啟動時執行一次的初始化函式。
  const canvas = createCanvas(windowWidth, windowHeight); // 建立填滿瀏覽器視窗的畫布。
  canvas.elt.setAttribute("aria-label", "p5.js 簡易指令練習測驗畫布"); // 為畫布加入輔助閱讀器描述。
  document.body.style.margin = "0"; // 移除網頁預設外距。
  document.body.style.overflow = "hidden"; // 隱藏網頁捲軸以維持全螢幕畫面。
  textFont("sans-serif"); // 設定通用無襯線字型以支援中文顯示。
  calculateLayout(); // 依畫布大小計算響應式版面。
} // 結束初始化函式。

function draw() { // 定義 p5.js 每一幀重複執行的繪圖函式。
  calculateLayout(); // 每一幀更新版面，確保縮放後位置正確。
  drawBackground(); // 繪製測驗背景。
  if (quizFinished) { // 判斷測驗是否已經結束。
    drawResultScreen(); // 結束時繪製結果畫面。
  } else { // 尚未結束時執行以下內容。
    drawQuizScreen(); // 尚未結束時繪製答題畫面。
  } // 結束測驗狀態判斷。
} // 結束主要繪圖函式。

function calculateLayout() { // 依目前畫布大小計算所有元件位置。
  const compact = width < 620; // 判斷是否使用手機窄版排版。
  const padding = constrain(min(width, height) * 0.055, 16, 42); // 計算畫面四周安全留白。
  const preferredContentWidth = compact ? width * 0.88 : 920; // 在手機保留左右空間，在大畫面限制內容寬度。
  const contentWidth = min(width - padding * 2, preferredContentWidth); // 確保主要內容寬度不超出畫布。
  const requestedShift = compact ? min(width * 0.12, 54) : width / 3; // 設定手機較小左移量與大畫面約三分之一左移量。
  const maxSafeShift = max(0, width / 2 - padding - contentWidth / 2); // 計算不讓整體內容超出左側安全邊界的最大左移量。
  const leftShift = min(requestedShift, maxSafeShift); // 將左移量限制在安全邊界內。
  const contentCenterX = width / 2 - leftShift; // 計算所有主要文字與元件共用的視覺中心。
  const left = contentCenterX - contentWidth / 2; // 依視覺中心計算主要內容左側座標。
  const headerHeight = compact ? 98 : 116; // 設定標題區高度。
  const questionY = headerHeight + (compact ? 12 : 20); // 設定題目卡片上方座標。
  const questionHeight = compact ? min(176, height * 0.205) : min(190, height * 0.22); // 設定題目卡片高度。
  const nextH = constrain(height * 0.072, 48, 62); // 計算下一題按鈕高度。
  const nextY = height - nextH - padding; // 計算下一題按鈕上方座標。
  const optionsY = questionY + questionHeight + (compact ? 14 : 20); // 計算選項區起始位置。
  const optionsBottom = nextY - (compact ? 14 : 20); // 計算選項區底部安全邊界。
  const optionGap = compact ? 10 : 14; // 設定選項之間的間隔。
  const twoColumns = width >= 620 && height >= 500; // 判斷是否使用桌機雙欄排版。
  const optionW = twoColumns ? (contentWidth - optionGap) / 2 : contentWidth; // 計算選項寬度。
  const rowCount = twoColumns ? 2 : 4; // 計算選項列數。
  const availableOptionHeight = max(0, optionsBottom - optionsY); // 計算選項區可使用的高度。
  const optionH = max(34, min(88, (availableOptionHeight - optionGap * (rowCount - 1)) / rowCount)); // 計算不超出常見畫面高度的選項高度。
  const options = []; // 建立儲存四個選項位置的陣列。
  quizQuestions[currentQuestion].options.forEach((optionText, index) => { // 逐一計算選項位置。
    const column = twoColumns ? index % 2 : 0; // 計算選項所在欄位。
    const row = twoColumns ? floor(index / 2) : index; // 計算選項所在列位。
    const optionX = left + column * (optionW + optionGap); // 依共同視覺中心計算選項左側座標。
    const optionY = optionsY + row * (optionH + optionGap); // 計算選項上方座標。
    options.push({ x: optionX, y: optionY, w: optionW, h: optionH }); // 儲存選項矩形資料。
  }); // 結束逐一計算選項位置。
  layout = { compact, padding, contentWidth, contentCenterX, left, headerHeight, questionY, questionHeight, options, next: { x: left, y: nextY, w: contentWidth, h: nextH } }; // 儲存響應式版面資料。
} // 結束版面計算函式。

function drawBackground() { // 定義背景繪製函式。
  background("#101827"); // 使用深藍色填滿畫布。
  noStroke(); // 關閉裝飾圖形外框線。
  fill("#18263d"); // 設定右上裝飾圓顏色。
  circle(width * 0.92, height * 0.08, min(width, height) * 0.34); // 繪製右上裝飾圓。
  fill("#132d35"); // 設定左下裝飾圓顏色。
  circle(width * 0.04, height * 0.94, min(width, height) * 0.42); // 繪製左下裝飾圓。
} // 結束背景繪製函式。

function drawQuizScreen() { // 定義答題畫面繪製函式。
  const question = quizQuestions[currentQuestion]; // 取得目前題目資料。
  const titleSize = layout.compact ? 22 : 30; // 設定主標題文字大小。
  const bodySize = layout.compact ? 16 : 19; // 設定一般文字大小。
  const questionSize = layout.compact ? 18 : 23; // 設定題目文字大小。
  fill("#f8fafc"); // 設定主標題顏色。
  textAlign(CENTER, TOP); // 將主標題設定為水平置中與靠上對齊。
  textStyle(BOLD); // 將主標題設定為粗體。
  textSize(titleSize); // 套用主標題文字大小。
  text("程式設計：p5.js 簡易指令練習測驗", layout.contentCenterX, 22, layout.contentWidth, 38); // 以左移後的共同視覺中心繪製主標題。
  textStyle(NORMAL); // 將後續文字恢復為一般字體。
  fill("#a9b8cf"); // 設定進度文字顏色。
  textSize(bodySize); // 套用進度文字大小。
  text(`第 ${currentQuestion + 1} 題，共 ${quizQuestions.length} 題　｜　目前答對 ${score} 題`, layout.contentCenterX, 62, layout.contentWidth, 28); // 以共同視覺中心繪製進度文字。
  drawProgressBar(); // 繪製答題進度條。
  drawQuestionCard(question, questionSize); // 繪製題目卡片。
  question.options.forEach((optionText, index) => { // 逐一繪製四個選項。
    drawOption(optionText, index, bodySize); // 繪製單一選項。
  }); // 結束逐一繪製選項。
  drawNextButton(bodySize); // 繪製下一題按鈕。
} // 結束答題畫面繪製函式。

function drawProgressBar() { // 定義進度條繪製函式。
  const barY = layout.headerHeight - 8; // 計算進度條垂直座標。
  const barH = 6; // 設定進度條高度。
  noStroke(); // 關閉進度條外框線。
  fill("#27364f"); // 設定進度條底色。
  rect(layout.left, barY, layout.contentWidth, barH, barH / 2); // 依共同內容範圍繪製進度條底色。
  fill("#63d5c8"); // 設定已完成進度顏色。
  rect(layout.left, barY, layout.contentWidth * ((currentQuestion + 1) / quizQuestions.length), barH, barH / 2); // 依共同內容範圍繪製目前進度。
} // 結束進度條繪製函式。

function drawQuestionCard(question, questionSize) { // 定義題目卡片繪製函式。
  noStroke(); // 關閉題目卡片外框線。
  fill("#24344e"); // 設定題目卡片底色。
  rect(layout.left, layout.questionY, layout.contentWidth, layout.questionHeight, 18); // 依左移後的內容範圍繪製題目卡片。
  fill("#63d5c8"); // 設定題號圓點顏色。
  circle(layout.left + 28, layout.questionY + 30, 12); // 保留題號提示圓點並放在卡片安全邊界內。
  fill("#d7e4f7"); // 設定題目文字顏色。
  textAlign(CENTER, CENTER); // 將題目文字設定為水平與垂直置中。
  textStyle(BOLD); // 將題目文字設定為粗體。
  textSize(questionSize); // 套用題目文字大小。
  text(question.question, layout.contentCenterX, layout.questionY + layout.questionHeight / 2, layout.contentWidth - 32, layout.questionHeight - 28); // 以卡片視覺中心置中繪製題目文字。
  textStyle(NORMAL); // 將後續文字恢復為一般字體。
} // 結束題目卡片繪製函式。

function drawOption(optionText, index, bodySize) { // 定義單一選項繪製函式。
  const box = layout.options[index]; // 取得目前選項位置。
  const isSelected = selectedOption === index; // 判斷目前選項是否被選取。
  const isCorrect = quizQuestions[currentQuestion].answer === index; // 判斷目前選項是否為正確答案。
  const isWrongSelection = selectedOption !== -1 && isSelected && !isCorrect; // 判斷目前選項是否為答錯選項。
  const shouldAnimate = selectedOption !== -1 && (isCorrect || isWrongSelection); // 判斷目前選項是否需要動畫。
  const verticalJump = shouldAnimate && isCorrect && selectedOption !== quizQuestions[currentQuestion].answer ? abs(sin(frameCount * 0.22)) * 12 : 0; // 計算答錯後正確選項的上下跳動距離。
  const horizontalShake = shouldAnimate && isWrongSelection ? sin(frameCount * 0.48) * 10 : 0; // 計算答錯選項的左右移動距離。
  let optionColor = "#ffffff"; // 設定尚未作答時的選項背景色。
  let labelColor = "#1b2940"; // 設定尚未作答時的文字顏色。
  if (selectedOption !== -1 && isCorrect) { // 判斷是否要標示正確答案。
    optionColor = "#d0f4de"; // 將正確答案背景設定為指定淡綠色。
    labelColor = "#123524"; // 將正確答案文字設定為深綠色。
  } // 結束正確答案顏色判斷。
  if (isWrongSelection) { // 判斷是否要標示答錯選項。
    optionColor = "#540b0e"; // 將答錯選項背景設定為指定深紅色。
    labelColor = "#ffffff"; // 將答錯選項文字設定為白色。
  } // 結束答錯選項顏色判斷。
  push(); // 儲存目前繪圖狀態。
  translate(horizontalShake, -verticalJump); // 套用選項左右或上下動畫位移。
  noStroke(); // 關閉選項外框線。
  fill(optionColor); // 套用選項背景色。
  rect(box.x, box.y, box.w, box.h, 14); // 繪製選項圓角矩形。
  fill(labelColor); // 套用選項文字顏色。
  textAlign(CENTER, CENTER); // 將選項文字設定為水平與垂直置中。
  textStyle(BOLD); // 將選項文字設定為粗體。
  textSize(bodySize); // 套用選項文字大小。
  text(`${String.fromCharCode(65 + index)}. ${optionText}`, box.x + box.w / 2, box.y + box.h / 2, box.w - 32, box.h - 12); // 以每個選項的視覺中心置中繪製文字並保留左右安全邊界。
  textStyle(NORMAL); // 將後續文字恢復為一般字體。
  pop(); // 還原繪圖狀態。
} // 結束單一選項繪製函式。

function drawNextButton(bodySize) { // 定義下一題按鈕繪製函式。
  const enabled = selectedOption !== -1; // 判斷使用者是否已完成目前題目。
  const buttonColor = enabled ? "#63d5c8" : "#41516a"; // 根據狀態設定按鈕背景色。
  const buttonTextColor = enabled ? "#102238" : "#a9b8cf"; // 根據狀態設定按鈕文字色。
  noStroke(); // 關閉按鈕外框線。
  fill(buttonColor); // 套用按鈕背景色。
  rect(layout.next.x, layout.next.y, layout.next.w, layout.next.h, 14); // 依共同內容範圍繪製下一題按鈕。
  fill(buttonTextColor); // 套用按鈕文字顏色。
  textAlign(CENTER, CENTER); // 將按鈕文字設為水平與垂直置中。
  textStyle(BOLD); // 將按鈕文字設定為粗體。
  textSize(bodySize); // 套用按鈕文字大小。
  const buttonLabel = enabled ? (currentQuestion === quizQuestions.length - 1 ? "查看結果" : "下一題") : "請先選擇答案"; // 決定按鈕顯示文字。
  text(buttonLabel, layout.contentCenterX, layout.next.y + layout.next.h / 2, layout.next.w - 32, layout.next.h - 10); // 以左移後的共同視覺中心置中繪製按鈕文字。
  textStyle(NORMAL); // 將後續文字恢復為一般字體。
} // 結束下一題按鈕繪製函式。

function drawResultScreen() { // 定義結果畫面繪製函式。
  const cardW = min(layout.contentWidth, 680); // 計算結果卡片寬度。
  const cardH = layout.compact ? 390 : 430; // 計算結果卡片高度。
  const resultCenterX = constrain(layout.contentCenterX, layout.padding + cardW / 2, width - layout.padding - cardW / 2); // 將結果頁視覺中心限制在安全邊界內。
  const cardX = resultCenterX - cardW / 2; // 計算結果卡片左側座標。
  const cardY = max(28, height / 2 - cardH / 2); // 計算結果卡片上方座標。
  noStroke(); // 關閉結果卡片外框線。
  fill("#24344e"); // 設定結果卡片底色。
  rect(cardX, cardY, cardW, cardH, 24); // 依左移後的結果中心繪製結果卡片。
  fill("#63d5c8"); // 設定結果標題顏色。
  textAlign(CENTER, TOP); // 將結果標題設為水平置中與靠上對齊。
  textStyle(BOLD); // 將結果標題設定為粗體。
  textSize(layout.compact ? 25 : 32); // 套用結果標題大小。
  text("測驗完成！", resultCenterX, cardY + 42, cardW - 40, 48); // 以結果頁視覺中心繪製標題。
  fill("#f8fafc"); // 設定分數文字顏色。
  textSize(layout.compact ? 48 : 64); // 套用分數文字大小。
  text(`${score} / ${quizQuestions.length}`, resultCenterX, cardY + 112, cardW - 40, 80); // 以結果頁視覺中心繪製答對題數。
  fill("#c5d2e5"); // 設定鼓勵文字顏色。
  textStyle(NORMAL); // 將鼓勵文字設定為一般字體。
  textSize(layout.compact ? 16 : 19); // 套用鼓勵文字大小。
  text(getResultMessage(), resultCenterX, cardY + 205, cardW - 60, 70); // 以結果頁視覺中心繪製鼓勵文字。
  const restartW = min(cardW - 48, 360); // 計算重新開始按鈕寬度。
  const restartH = layout.compact ? 54 : 60; // 計算重新開始按鈕高度。
  const restartX = resultCenterX - restartW / 2; // 計算重新開始按鈕左側座標。
  const restartY = cardY + cardH - restartH - 34; // 計算重新開始按鈕上方座標。
  fill("#63d5c8"); // 設定重新開始按鈕底色。
  rect(restartX, restartY, restartW, restartH, 14); // 繪製重新開始按鈕。
  fill("#102238"); // 設定重新開始按鈕文字顏色。
  textStyle(BOLD); // 將重新開始文字設定為粗體。
  textSize(layout.compact ? 17 : 20); // 套用重新開始文字大小。
  text("重新開始測驗", resultCenterX, restartY + restartH / 2, restartW - 24, restartH - 10); // 以結果頁視覺中心置中繪製重新開始文字。
  layout.restart = { x: restartX, y: restartY, w: restartW, h: restartH }; // 儲存結果頁重新開始按鈕點擊範圍。
  textStyle(NORMAL); // 將後續文字恢復為一般字體。
} // 結束結果畫面繪製函式。

function getResultMessage() { // 定義依分數產生鼓勵文字的函式。
  if (score === quizQuestions.length) { // 判斷是否獲得滿分。
    return "太棒了！你已經熟悉這些 p5.js 簡易指令。"; // 回傳滿分鼓勵文字。
  } // 結束滿分判斷。
  if (score >= 3) { // 判斷是否答對三題以上。
    return "表現很好！再練習幾次就能更加熟練。"; // 回傳中高分鼓勵文字。
  } // 結束中高分判斷。
  return "繼續加油！重新挑戰並熟悉 p5.js 指令吧。"; // 回傳需要加強練習的鼓勵文字。
} // 結束結果文字函式。

function mousePressed() { // 定義滑鼠按下事件處理函式。
  handlePointer(mouseX, mouseY); // 將滑鼠座標交給共用輸入函式。
} // 結束滑鼠按下事件函式。

function touchStarted() { // 定義觸控開始事件處理函式。
  if (touches.length > 0) { // 確認至少有一個觸控點。
    handlePointer(touches[0].x, touches[0].y); // 將第一個觸控點交給共用輸入函式。
  } // 結束觸控點存在判斷。
  return false; // 阻止瀏覽器預設捲動或縮放行為。
} // 結束觸控事件函式。

function handlePointer(pointerX, pointerY) { // 定義滑鼠與觸控共用的互動處理函式。
  const now = millis(); // 取得目前 p5.js 執行時間。
  if (now - lastInputTime < 250) { // 判斷是否收到短時間內的重複輸入。
    return; // 忽略重複輸入，避免一個觸控推進兩題。
  } // 結束重複輸入判斷。
  lastInputTime = now; // 記錄這次有效輸入時間。
  if (quizFinished) { // 判斷目前是否位於結果畫面。
    if (isInside(pointerX, pointerY, layout.restart)) { // 判斷是否點擊重新開始按鈕。
      restartQuiz(); // 重設測驗並重新開始。
    } // 結束重新開始按鈕判斷。
    return; // 結果畫面不再處理其他點擊。
  } // 結束結果畫面判斷。
  if (selectedOption === -1) { // 只有尚未作答時才允許選取選項。
    layout.options.forEach((box, index) => { // 逐一檢查四個選項。
      if (isInside(pointerX, pointerY, box)) { // 判斷輸入位置是否在目前選項內。
        selectedOption = index; // 記錄使用者選取的選項。
        if (selectedOption === quizQuestions[currentQuestion].answer) { // 判斷選取答案是否正確。
          score += 1; // 答對時增加一題分數。
        } // 結束答對分數判斷。
      } // 結束選項命中判斷。
    }); // 結束逐一檢查選項。
    return; // 作答後等待使用者按下一題。
  } // 結束尚未作答判斷。
  if (isInside(pointerX, pointerY, layout.next)) { // 判斷是否點擊下一題按鈕。
    goToNextQuestion(); // 前往下一題或結果畫面。
  } // 結束下一題按鈕判斷。
} // 結束共用互動處理函式。

function isInside(pointerX, pointerY, box) { // 定義矩形範圍命中判斷函式。
  if (!box) { // 判斷矩形資料是否不存在。
    return false; // 沒有矩形資料時回傳未命中。
  } // 結束矩形存在判斷。
  return pointerX >= box.x && pointerX <= box.x + box.w && pointerY >= box.y && pointerY <= box.y + box.h; // 回傳座標是否在矩形內。
} // 結束矩形命中判斷函式。

function goToNextQuestion() { // 定義前往下一題或顯示結果的函式。
  if (currentQuestion >= quizQuestions.length - 1) { // 判斷目前是否為第五題。
    quizFinished = true; // 將畫面切換為結果畫面。
    return; // 結束函式以避免題目索引超出範圍。
  } // 結束最後一題判斷。
  currentQuestion += 1; // 將題目索引增加一題。
  selectedOption = -1; // 清除上一題選取狀態，避免同題重複作答。
} // 結束前往下一題函式。

function restartQuiz() { // 定義重新開始測驗的函式。
  currentQuestion = 0; // 將題目索引重設為第一題。
  selectedOption = -1; // 清除目前選項選取狀態。
  score = 0; // 將答對題數重設為零。
  quizFinished = false; // 將畫面切換回答題畫面。
  layout.restart = undefined; // 清除結果頁重新開始按鈕資料。
} // 結束重新開始函式。

function windowResized() { // 定義瀏覽器視窗縮放事件函式。
  resizeCanvas(windowWidth, windowHeight); // 將畫布調整為新的視窗寬高。
  calculateLayout(); // 重新計算縮放後的版面位置。
} // 結束視窗縮放事件函式。

```
:::


---

## 學習2：網頁設定為響應式網頁

https://cfchen58.synology.me/115/week4/stage2/

**這個階段的目標：** 讓網站在電腦、平板、手機（直向與橫向）都能正常顯示，視窗大小改變時版面自動調整。
**這個階段會修改的檔案：** index.html、sketch.js

### 執行截圖

![螢幕擷取畫面 2026-10-08 150549](https://hackmd.io/_uploads/HJ9l-aEsGl.png)

### 第一次問 AI

```tex!
網頁設定為響應式網頁,主要是要讓網站在電腦、平板、手機（直向與橫向）都能正常顯示，視窗大小改變時版面自動調整
```

### 第二次問 AI

```tex!答對

```


### 第三次問 AI

```tex!答錯
```

### 程式碼內容

:::info
:::spoiler 點開貼上學習2的程式碼
// p5.js「程式設計：p5.js 簡易指令練習測驗」響應式完整測驗系統。 // 說明程式主題與用途。
// 本程式可直接貼入一般 p5.js Editor 的 sketch.js 檔案中執行。 // 說明程式的使用方式。

const quizQuestions = [ // 建立五題測驗資料陣列。
  { // 建立第一題物件。
    question: "在 p5.js 中，哪一個函式通常只會在程式開始時執行一次？", // 設定第一題題目。
    options: ["setup()", "draw()", "mousePressed()", "windowResized()"], // 設定第一題四個選項。
    answer: 0 // 設定第一題正確答案索引。
  }, // 結束第一題物件。
  { // 建立第二題物件。
    question: "在 p5.js 中，哪一個函式會持續重複執行來產生動畫？", // 設定第二題題目。
    options: ["size()", "draw()", "fill()", "text()"], // 設定第二題四個選項。
    answer: 1 // 設定第二題正確答案索引。
  }, // 結束第二題物件。
  { // 建立第三題物件。
    question: "下列哪一個指令可以設定圖形的填滿顏色？", // 設定第三題題目。
    options: ["stroke()", "line()", "fill()", "background()"], // 設定第三題四個選項。
    answer: 2 // 設定第三題正確答案索引。
  }, // 結束第三題物件。
  { // 建立第四題物件。
    question: "在滑鼠事件中，mouseX 通常代表什麼資訊？", // 設定第四題題目。
    options: ["滑鼠的垂直座標", "畫布的寬度", "滑鼠的水平座標", "目前的題號"], // 設定第四題四個選項。
    answer: 2 // 設定第四題正確答案索引。
  }, // 結束第四題物件。
  { // 建立第五題物件。
    question: "下列哪一個指令可以在畫布上畫出橢圓形？", // 設定第五題題目。
    options: ["rect()", "ellipse()", "triangle()", "point()"], // 設定第五題四個選項。
    answer: 1 // 設定第五題正確答案索引。
  } // 結束第五題物件。
]; // 結束五題測驗資料陣列。

let currentQuestion = 0; // 記錄目前題目索引。
let selectedOption = -1; // 記錄目前選項索引，負一代表尚未作答。
let score = 0; // 記錄目前答對題數。
let quizFinished = false; // 記錄測驗是否已結束。
let lastInputTime = 0; // 記錄最後一次輸入時間，避免觸控重複觸發滑鼠事件。
let layout = {}; // 儲存所有響應式版面位置與尺寸資料。

function setup() { // 定義 p5.js 啟動時執行一次的初始化函式。
  const canvas = createCanvas(Math.max(1, windowWidth), Math.max(1, windowHeight)); // 建立不超出瀏覽器視窗的全螢幕畫布。
  canvas.elt.setAttribute("aria-label", "p5.js 簡易指令練習測驗畫布"); // 為畫布加入輔助閱讀器描述。
  applyPageStyles(canvas); // 設定頁面與畫布樣式，避免外距與水平捲動。
  textFont("sans-serif"); // 設定通用無襯線字型以支援中文顯示。
  calculateLayout(); // 依目前畫布尺寸計算初始版面。
} // 結束初始化函式。

function applyPageStyles(canvas) { // 定義套用頁面與畫布 CSS 樣式的函式。
  document.documentElement.style.margin = "0"; // 移除 html 元素預設外距。
  document.documentElement.style.padding = "0"; // 移除 html 元素預設內距。
  document.documentElement.style.width = "100%"; // 讓 html 元素填滿瀏覽器寬度。
  document.documentElement.style.height = "100%"; // 讓 html 元素填滿瀏覽器高度。
  document.documentElement.style.overflow = "hidden"; // 禁止 html 元素產生捲軸。
  document.body.style.margin = "0"; // 移除 body 元素預設外距。
  document.body.style.padding = "0"; // 移除 body 元素預設內距。
  document.body.style.width = "100%"; // 讓 body 元素填滿瀏覽器寬度。
  document.body.style.height = "100%"; // 讓 body 元素填滿瀏覽器高度。
  document.body.style.overflow = "hidden"; // 禁止 body 元素產生水平與垂直捲軸。
  document.body.style.overscrollBehavior = "none"; // 防止觸控拖曳造成頁面回彈。
  document.body.style.touchAction = "none"; // 阻止畫布互動時的瀏覽器預設觸控手勢。
  canvas.elt.style.display = "block"; // 移除 canvas 行內元素可能產生的底部空白。
  canvas.elt.style.maxWidth = "100vw"; // 限制畫布 CSS 寬度不超出視窗。
  canvas.elt.style.maxHeight = "100vh"; // 限制畫布 CSS 高度不超出視窗。
  canvas.elt.style.touchAction = "none"; // 讓觸控事件交由 p5.js 處理。
} // 結束頁面樣式設定函式。

function draw() { // 定義 p5.js 每一幀重複執行的繪圖函式。
  drawBackground(); // 繪製深色背景與裝飾圖形。
  if (quizFinished) { // 判斷目前是否位於結果頁。
    drawResultScreen(); // 繪製測驗結果頁面。
  } else { // 尚未完成測驗時執行以下內容。
    drawQuizScreen(); // 繪製題目與選項頁面。
  } // 結束測驗狀態判斷。
} // 結束主要繪圖函式。

function calculateLayout() { // 依目前畫布寬高重新計算所有版面資料。
  const canvasWidth = Math.max(1, width); // 取得至少為一的畫布寬度。
  const canvasHeight = Math.max(1, height); // 取得至少為一的畫布高度。
  const portrait = canvasHeight >= canvasWidth; // 判斷目前是否為直向畫面。
  const shortSide = Math.min(canvasWidth, canvasHeight); // 取得畫布短邊作為響應式基準。
  const phoneSize = shortSide < 600; // 依短邊判斷是否為手機尺寸。
  const phonePortrait = phoneSize && portrait; // 判斷是否為手機直向模式。
  const landscape = !portrait; // 判斷目前是否為橫向畫面。
  const safeX = constrain(shortSide * 0.045, 10, 44); // 依畫布短邊計算左右安全邊界。
  const safeY = constrain(shortSide * 0.035, 8, 30); // 依畫布短邊計算上下安全邊界。
  const gap = constrain(shortSide * 0.025, 4, 18); // 依畫布短邊計算元件間距。
  const contentMaximum = phonePortrait ? canvasWidth - safeX * 2 : Math.min(canvasWidth - safeX * 2, phoneSize ? canvasWidth * 0.92 : 960); // 依裝置類型計算內容最大寬度。
  const contentWidth = Math.max(1, Math.min(canvasWidth - safeX * 2, contentMaximum)); // 確保內容寬度不超出左右安全邊界。
  const requestedShift = canvasWidth >= 900 ? canvasWidth / 3 : landscape && canvasWidth >= 600 ? canvasWidth * 0.08 : 0; // 設定桌面版約向左移動畫布三分之一的目標距離。
  const maximumSafeShift = Math.max(0, canvasWidth / 2 - safeX - contentWidth / 2); // 計算不讓內容超出左側安全邊界的最大移動距離。
  const leftShift = Math.min(requestedShift, maximumSafeShift); // 將左移距離限制在畫布安全範圍內。
  const contentCenterX = canvasWidth / 2 - leftShift; // 計算所有主要內容共用的視覺中心。
  const left = contentCenterX - contentWidth / 2; // 依視覺中心計算內容左側位置。
  const scale = constrain(Math.min(canvasWidth / 390, canvasHeight / 720), 0.55, 1.2); // 依視窗尺寸計算整體文字與間距縮放比例。
  const titleSize = constrain(27 * scale, phonePortrait ? 15 : 18, 34); // 計算主標題文字大小。
  const progressSize = constrain(15 * scale, 10, 19); // 計算進度文字大小。
  const questionSize = constrain(20 * scale, 12, 25); // 計算題目文字大小。
  const optionSize = constrain(17 * scale, 11, 21); // 計算選項文字大小。
  const buttonSize = constrain(17 * scale, 11, 21); // 計算按鈕文字大小。
  const titleLineH = titleSize * 1.3; // 計算主標題行高。
  const progressLineH = progressSize * 1.35; // 計算進度文字行高。
  const titleY = safeY; // 設定主標題上方位置。
  const progressY = titleY + titleLineH + gap * 0.35; // 設定進度文字上方位置。
  const progressBarY = progressY + progressLineH + gap * 0.35; // 設定進度條上方位置。
  const progressBarH = constrain(shortSide * 0.012, 4, 8); // 計算進度條高度。
  const questionY = progressBarY + progressBarH + gap; // 計算題目卡片上方位置。
  const nextH = constrain(canvasHeight * 0.075, phonePortrait ? 34 : 42, 62); // 依視窗高度計算下一題按鈕高度。
  const nextY = Math.max(questionY + 1, canvasHeight - safeY - nextH); // 計算下一題按鈕上方位置並保留下方安全邊界。
  const mainHeight = Math.max(1, nextY - questionY); // 計算題目與選項可以使用的總高度。
  const twoColumns = !phonePortrait && (landscape || !phoneSize); // 手機直向使用單欄，其餘寬螢幕使用雙欄。
  const rowCount = twoColumns ? 2 : 4; // 依欄數計算選項列數。
  const minimumOptionH = Math.max(26, optionSize * 2.05); // 設定選項卡片可讀文字所需的最小高度。
  const optionGap = constrain(shortSide * 0.022, 4, 14); // 計算選項之間的間距。
  const questionGap = gap; // 設定題目卡片與選項區的間距。
  const optionsGap = gap; // 設定選項區與下一題按鈕的間距。
  const minimumOptionsHeight = minimumOptionH * rowCount + optionGap * (rowCount - 1); // 計算選項區的最低需求高度。
  const questionMaximum = Math.max(1, mainHeight - questionGap - optionsGap - minimumOptionsHeight); // 計算題目卡片在極小畫面可使用的最大高度。
  const preferredQuestionH = mainHeight * (twoColumns ? 0.34 : 0.25); // 依直橫向與欄數設定題目卡片偏好高度。
  const questionHeight = Math.max(1, Math.min(preferredQuestionH, questionMaximum)); // 將題目卡片高度限制在可用空間內。
  const optionsY = questionY + questionHeight + questionGap; // 計算選項區上方位置。
  const optionsBottom = Math.max(optionsY + 1, nextY - optionsGap); // 計算選項區下方安全位置。
  const availableOptionHeight = Math.max(1, optionsBottom - optionsY); // 計算選項區實際可用高度。
  const optionH = Math.max(1, (availableOptionHeight - optionGap * (rowCount - 1)) / rowCount); // 依列數平均分配每個選項高度。
  const optionW = twoColumns ? (contentWidth - optionGap) / 2 : contentWidth; // 依欄數計算每個選項寬度。
  const options = []; // 建立儲存四個選項矩形的陣列。
  quizQuestions[currentQuestion].options.forEach((optionText, index) => { // 逐一計算每個選項的位置。
    const column = twoColumns ? index % 2 : 0; // 計算選項所在欄位。
    const row = twoColumns ? Math.floor(index / 2) : index; // 計算選項所在列位。
    const optionX = left + column * (optionW + optionGap); // 計算選項左側座標。
    const optionY = optionsY + row * (optionH + optionGap); // 計算選項上方座標。
    options.push({ x: optionX, y: optionY, w: optionW, h: optionH }); // 儲存選項矩形資料。
  }); // 結束逐一計算選項位置。
  layout = { canvasWidth, canvasHeight, portrait, landscape, phoneSize, phonePortrait, twoColumns, safeX, safeY, gap, scale, contentWidth, contentCenterX, left, titleY, titleSize, progressY, progressSize, progressBarY, progressBarH, questionY, questionHeight, questionSize, optionsY, optionsBottom, optionGap, optionSize, options, next: { x: left, y: nextY, w: contentWidth, h: nextH }, buttonSize }; // 儲存最新完整版面資料。
  if (quizFinished) { // 判斷結果頁是否需要同步更新版面。
    calculateResultLayout(); // 重新計算結果頁卡片與按鈕位置。
  } // 結束結果頁版面更新判斷。
} // 結束版面計算函式。

function calculateResultLayout() { // 依目前畫布尺寸計算結果頁版面資料。
  const cardW = Math.min(layout.contentWidth, Math.max(1, layout.canvasWidth - layout.safeX * 2)); // 計算不超出視窗的結果卡片寬度。
  const cardH = Math.min(layout.canvasHeight - layout.safeY * 2, Math.max(1, layout.canvasHeight * 0.78)); // 計算不超出視窗的結果卡片高度。
  const resultCenterX = constrain(layout.contentCenterX, layout.safeX + cardW / 2, layout.canvasWidth - layout.safeX - cardW / 2); // 將結果卡片中心限制在安全範圍。
  const cardX = resultCenterX - cardW / 2; // 計算結果卡片左側座標。
  const cardY = Math.max(layout.safeY, (layout.canvasHeight - cardH) / 2); // 計算結果卡片上方座標。
  const restartW = Math.min(cardW - Math.min(48, cardW * 0.12), Math.max(1, cardW * 0.82)); // 計算重新開始按鈕寬度。
  const restartH = Math.min(layout.canvasHeight * 0.16, Math.max(30, layout.canvasHeight * 0.1)); // 計算重新開始按鈕高度。
  const restartX = resultCenterX - restartW / 2; // 計算重新開始按鈕左側座標。
  const restartY = cardY + cardH - restartH - Math.min(28, layout.safeY * 1.2); // 計算重新開始按鈕上方座標。
  layout.result = { cardX, cardY, cardW, cardH, centerX: resultCenterX, restart: { x: restartX, y: restartY, w: restartW, h: restartH } }; // 儲存結果頁版面資料。
} // 結束結果頁版面計算函式。

function drawBackground() { // 定義背景繪製函式。
  background("#101827"); // 使用深藍色填滿畫布。
  noStroke(); // 關閉裝飾圖形外框線。
  fill("#18263d"); // 設定右上裝飾圓顏色。
  circle(layout.canvasWidth * 0.94, layout.canvasHeight * 0.08, Math.min(layout.canvasWidth, layout.canvasHeight) * 0.34); // 繪製右上裝飾圓。
  fill("#132d35"); // 設定左下裝飾圓顏色。
  circle(layout.canvasWidth * 0.04, layout.canvasHeight * 0.94, Math.min(layout.canvasWidth, layout.canvasHeight) * 0.42); // 繪製左下裝飾圓。
} // 結束背景繪製函式。

function drawQuizScreen() { // 定義答題畫面繪製函式。
  const question = quizQuestions[currentQuestion]; // 取得目前題目資料。
  fill("#f8fafc"); // 設定主標題顏色。
  drawFittedText("程式設計：p5.js 簡易指令練習測驗", layout.contentCenterX, layout.titleY, layout.contentWidth, layout.titleSize * 1.5, layout.titleSize, Math.max(12, layout.titleSize * 0.62), CENTER, TOP, BOLD); // 以共同視覺中心繪製可自動縮放的主標題。
  fill("#a9b8cf"); // 設定進度文字顏色。
  drawFittedText(`第 ${currentQuestion + 1} 題，共 ${quizQuestions.length} 題　｜　目前答對 ${score} 題`, layout.contentCenterX, layout.progressY, layout.contentWidth, layout.progressSize * 1.5, layout.progressSize, 9, CENTER, TOP, NORMAL); // 以共同視覺中心繪製進度文字。
  drawProgressBar(); // 繪製答題進度條。
  drawQuestionCard(question); // 繪製目前題目卡片。
  question.options.forEach((optionText, index) => { // 逐一繪製四個選項。
    drawOption(optionText, index); // 繪製單一選項卡片。
  }); // 結束逐一繪製選項。
  drawNextButton(); // 繪製下一題按鈕。
} // 結束答題畫面繪製函式。

function drawProgressBar() { // 定義進度條繪製函式。
  noStroke(); // 關閉進度條外框線。
  fill("#27364f"); // 設定進度條底色。
  rect(layout.left, layout.progressBarY, layout.contentWidth, layout.progressBarH, layout.progressBarH / 2); // 依內容範圍繪製進度條底色。
  fill("#63d5c8"); // 設定已完成進度顏色。
  rect(layout.left, layout.progressBarY, layout.contentWidth * ((currentQuestion + 1) / quizQuestions.length), layout.progressBarH, layout.progressBarH / 2); // 依題目進度繪製已完成部分。
} // 結束進度條繪製函式。

function drawQuestionCard(question) { // 定義題目卡片繪製函式。
  noStroke(); // 關閉題目卡片外框線。
  fill("#24344e"); // 設定題目卡片底色。
  rect(layout.left, layout.questionY, layout.contentWidth, layout.questionHeight, Math.min(18, layout.questionHeight * 0.16)); // 依最新版面繪製題目卡片。
  fill("#63d5c8"); // 設定題號提示圓點顏色。
  circle(layout.left + Math.min(24, layout.contentWidth * 0.06), layout.questionY + Math.min(26, layout.questionHeight * 0.25), Math.max(6, layout.scale * 6)); // 將題號提示放在卡片安全邊界內。
  fill("#d7e4f7"); // 設定題目文字顏色。
  drawFittedText(question.question, layout.contentCenterX, layout.questionY + layout.questionHeight * 0.08, Math.max(1, layout.contentWidth - layout.gap * 2), Math.max(1, layout.questionHeight * 0.84), layout.questionSize, Math.max(10, layout.questionSize * 0.55), CENTER, CENTER, BOLD); // 以卡片中心繪製可換行與自動縮放的題目。
} // 結束題目卡片繪製函式。

function drawOption(optionText, index) { // 定義單一選項繪製函式。
  const box = layout.options[index]; // 取得目前選項位置。
  const isSelected = selectedOption === index; // 判斷目前選項是否被選取。
  const isCorrect = quizQuestions[currentQuestion].answer === index; // 判斷目前選項是否為正確答案。
  const hasWrongAnswer = selectedOption !== -1 && selectedOption !== quizQuestions[currentQuestion].answer; // 判斷目前題目是否答錯。
  const isWrongSelection = hasWrongAnswer && isSelected; // 判斷目前選項是否為使用者選錯的答案。
  const verticalJump = hasWrongAnswer && isCorrect ? Math.abs(Math.sin(frameCount * 0.22)) * Math.min(12, layout.scale * 12) : 0; // 計算答錯後正確選項的上下跳動距離。
  const horizontalShake = isWrongSelection ? Math.sin(frameCount * 0.48) * Math.min(10, layout.scale * 10) : 0; // 計算答錯選項的左右移動距離。
  let optionColor = "#ffffff"; // 設定尚未作答時的選項背景色。
  let labelColor = "#1b2940"; // 設定尚未作答時的選項文字色。
  if (selectedOption !== -1 && isCorrect) { // 判斷是否需要標示正確答案。
    optionColor = "#d0f4de"; // 將正確答案背景設定為指定淡綠色。
    labelColor = "#123524"; // 將正確答案文字設定為深綠色。
  } // 結束正確答案顏色判斷。
  if (isWrongSelection) { // 判斷是否需要標示答錯選項。
    optionColor = "#540b0e"; // 將答錯選項背景設定為指定深紅色。
    labelColor = "#ffffff"; // 將答錯選項文字設定為白色。
  } // 結束答錯答案顏色判斷。
  push(); // 儲存目前繪圖狀態。
  translate(horizontalShake, -verticalJump); // 套用選項左右或上下動畫位移。
  noStroke(); // 關閉選項外框線。
  fill(optionColor); // 套用選項背景顏色。
  rect(box.x, box.y, box.w, box.h, Math.min(14, box.h * 0.18)); // 依最新版面繪製圓角選項卡片。
  fill(labelColor); // 套用選項文字顏色。
  drawFittedText(`${String.fromCharCode(65 + index)}. ${optionText}`, box.x + box.w / 2, box.y + box.h * 0.08, Math.max(1, box.w - layout.gap * 2), Math.max(1, box.h * 0.84), layout.optionSize, Math.max(9, layout.optionSize * 0.52), CENTER, CENTER, BOLD); // 以選項中心繪製可換行與自動縮放的文字。
  pop(); // 還原繪圖狀態。
} // 結束單一選項繪製函式。

function drawNextButton() { // 定義下一題按鈕繪製函式。
  const enabled = selectedOption !== -1; // 判斷使用者是否已完成目前題目。
  const buttonColor = enabled ? "#63d5c8" : "#41516a"; // 根據按鈕狀態設定背景色。
  const buttonTextColor = enabled ? "#102238" : "#a9b8cf"; // 根據按鈕狀態設定文字色。
  const button = layout.next; // 取得最新下一題按鈕位置。
  noStroke(); // 關閉按鈕外框線。
  fill(buttonColor); // 套用按鈕背景色。
  rect(button.x, button.y, button.w, button.h, Math.min(14, button.h * 0.22)); // 依最新版面繪製下一題按鈕。
  fill(buttonTextColor); // 套用按鈕文字顏色。
  const buttonLabel = enabled ? (currentQuestion === quizQuestions.length - 1 ? "查看結果" : "下一題") : "請先選擇答案"; // 依作答狀態決定按鈕文字。
  drawFittedText(buttonLabel, button.x + button.w / 2, button.y + button.h * 0.08, Math.max(1, button.w - layout.gap * 2), Math.max(1, button.h * 0.84), layout.buttonSize, Math.max(9, layout.buttonSize * 0.55), CENTER, CENTER, BOLD); // 以按鈕中心繪製可自動縮放的按鈕文字。
} // 結束下一題按鈕繪製函式。

function drawResultScreen() { // 定義結果頁繪製函式。
  if (!layout.result) { // 判斷結果頁版面資料是否尚未建立。
    calculateResultLayout(); // 在需要時立即建立結果頁版面資料。
  } // 結束結果版面存在判斷。
  const result = layout.result; // 取得最新結果頁版面資料。
  noStroke(); // 關閉結果卡片外框線。
  fill("#24344e"); // 設定結果卡片底色。
  rect(result.cardX, result.cardY, result.cardW, result.cardH, Math.min(24, result.cardH * 0.08)); // 依視窗尺寸繪製結果卡片。
  fill("#63d5c8"); // 設定結果標題顏色。
  drawFittedText("測驗完成！", result.centerX, result.cardY + result.cardH * 0.08, result.cardW * 0.9, result.cardH * 0.16, constrain(30 * layout.scale, 18, 34), 13, CENTER, CENTER, BOLD); // 繪製可自動縮放的結果標題。
  fill("#f8fafc"); // 設定分數文字顏色。
  drawFittedText(`${score} / ${quizQuestions.length}`, result.centerX, result.cardY + result.cardH * 0.25, result.cardW * 0.9, result.cardH * 0.24, constrain(58 * layout.scale, 26, 64), 20, CENTER, CENTER, BOLD); // 繪製答對題數。
  fill("#c5d2e5"); // 設定鼓勵文字顏色。
  drawFittedText(getResultMessage(), result.centerX, result.cardY + result.cardH * 0.54, result.cardW * 0.84, result.cardH * 0.18, constrain(19 * layout.scale, 12, 21), 10, CENTER, CENTER, NORMAL); // 繪製結果鼓勵文字。
  fill("#63d5c8"); // 設定重新開始按鈕底色。
  rect(result.restart.x, result.restart.y, result.restart.w, result.restart.h, Math.min(14, result.restart.h * 0.22)); // 繪製重新開始按鈕。
  fill("#102238"); // 設定重新開始按鈕文字顏色。
  drawFittedText("重新開始測驗", result.restart.x + result.restart.w / 2, result.restart.y + result.restart.h * 0.08, Math.max(1, result.restart.w - layout.gap * 2), Math.max(1, result.restart.h * 0.84), constrain(19 * layout.scale, 12, 21), 10, CENTER, CENTER, BOLD); // 繪製可自動縮放的重新開始文字。
} // 結束結果頁繪製函式。

function getResultMessage() { // 定義依分數產生鼓勵文字的函式。
  if (score === quizQuestions.length) { // 判斷是否獲得滿分。
    return "太棒了！你已經熟悉這些 p5.js 簡易指令。"; // 回傳滿分鼓勵文字。
  } // 結束滿分判斷。
  if (score >= 3) { // 判斷是否答對三題以上。
    return "表現很好！再練習幾次就能更加熟練。"; // 回傳中高分鼓勵文字。
  } // 結束中高分判斷。
  return "繼續加油！重新挑戰並熟悉 p5.js 指令吧。"; // 回傳需要加強練習的鼓勵文字。
} // 結束結果文字函式。

function drawFittedText(value, x, y, boxW, boxH, startSize, minimumSize, horizontalAlign, verticalAlign, styleValue) { // 定義可換行且會自動縮小文字的共用繪製函式。
  push(); // 儲存目前文字繪製狀態。
  textAlign(horizontalAlign, verticalAlign); // 套用呼叫端指定的水平與垂直對齊方式。
  textStyle(styleValue); // 套用呼叫端指定的文字樣式。
  textWrap(WORD); // 使用 p5.js WORD 換行模式避免長文字超出文字框。
  let fittedSize = Math.max(minimumSize, startSize); // 建立不小於最小值的初始文字大小。
  textSize(fittedSize); // 套用初始文字大小以測量換行高度。
  while (fittedSize > minimumSize && estimateTextHeight(value, boxW, fittedSize) > boxH) { // 持續縮小文字直到內容能放入指定文字框。
    fittedSize -= 0.5; // 每次縮小半個像素以提高不同裝置的適配精度。
    textSize(fittedSize); // 套用縮小後的文字大小。
  } // 結束自動縮小文字迴圈。
  text(value, x, y, Math.max(1, boxW), Math.max(1, boxH)); // 在限制好的文字框中繪製內容。
  pop(); // 還原文字繪製狀態。
} // 結束自動縮放文字函式。

function estimateTextHeight(value, boxW, fontSize) { // 定義估算換行文字高度的函式。
  textSize(fontSize); // 套用要測量的字型大小。
  const lines = getWrappedLineCount(String(value), Math.max(1, boxW)); // 依文字寬度取得估計行數。
  return lines * fontSize * 1.25; // 回傳行數乘以行高的估計高度。
} // 結束文字高度估算函式。

function getWrappedLineCount(value, boxW) { // 定義支援中英文混合文字的換行行數估算函式。
  const paragraphs = value.split("\\n"); // 依換行符號分割文字段落。
  let lineCount = 0; // 建立目前累計行數。
  paragraphs.forEach((paragraph) => { // 逐一處理每個文字段落。
    let currentLine = ""; // 建立目前正在測量的文字行。
    Array.from(paragraph).forEach((character) => { // 逐字測量中英文與符號寬度。
      const trialLine = currentLine + character; // 暫時加入下一個字元以測量寬度。
      if (currentLine.length > 0 && textWidth(trialLine) > boxW) { // 判斷加入字元後是否超出文字框寬度。
        lineCount += 1; // 將目前文字行計入總行數。
        currentLine = character; // 將超出的字元放到下一行。
      } else { // 文字仍可放在目前行時執行以下內容。
        currentLine = trialLine; // 更新目前文字行內容。
      } // 結束文字寬度判斷。
    }); // 結束逐字測量。
    lineCount += 1; // 將段落最後一行計入總行數。
  }); // 結束逐一處理文字段落。
  return Math.max(1, lineCount); // 至少回傳一行文字。
} // 結束換行行數估算函式。

function mousePressed() { // 定義滑鼠按下事件處理函式。
  calculateLayout(); // 在判定按鈕前取得最新視窗版面資料。
  handlePointer(mouseX, mouseY); // 將最新滑鼠座標交給共用輸入函式。
} // 結束滑鼠按下事件函式。

function touchStarted() { // 定義觸控開始事件處理函式。
  calculateLayout(); // 在判定觸控按鈕前取得最新視窗版面資料。
  if (touches.length > 0) { // 確認至少存在一個觸控點。
    handlePointer(touches[0].x, touches[0].y); // 將第一個觸控點交給共用輸入函式。
  } // 結束觸控點存在判斷。
  return false; // 阻止瀏覽器預設捲動或縮放行為。
} // 結束觸控事件函式。

function handlePointer(pointerX, pointerY) { // 定義滑鼠與觸控共用的互動處理函式。
  calculateLayout(); // 再次確保互動判定使用最新 layout 資料。
  const now = millis(); // 取得目前 p5.js 執行時間。
  if (now - lastInputTime < 250) { // 判斷是否收到短時間內的重複輸入。
    return; // 忽略重複輸入，避免一次觸控推進兩題。
  } // 結束重複輸入判斷。
  lastInputTime = now; // 記錄這次有效輸入時間。
  if (quizFinished) { // 判斷目前是否位於結果頁。
    if (isInside(pointerX, pointerY, layout.result.restart)) { // 判斷是否點擊最新重新開始按鈕。
      restartQuiz(); // 重設測驗並重新開始。
    } // 結束重新開始按鈕判斷。
    return; // 結果頁不再處理其他點擊。
  } // 結束結果頁判斷。
  if (selectedOption === -1) { // 只有尚未作答時才允許選取選項。
    for (let index = 0; index < layout.options.length; index += 1) { // 逐一檢查四個選項的最新範圍。
      if (isInside(pointerX, pointerY, layout.options[index])) { // 判斷輸入位置是否位於目前選項內。
        selectedOption = index; // 記錄使用者選取的選項。
        if (selectedOption === quizQuestions[currentQuestion].answer) { // 判斷選取答案是否正確。
          score += 1; // 答對時增加一題分數。
        } // 結束答對分數判斷。
        break; // 命中選項後停止檢查，避免重複處理。
      } // 結束選項命中判斷。
    } // 結束逐一檢查選項。
    return; // 作答後等待使用者按下一題。
  } // 結束尚未作答判斷。
  if (isInside(pointerX, pointerY, layout.next)) { // 判斷是否點擊最新下一題按鈕。
    goToNextQuestion(); // 前往下一題或結果頁。
  } // 結束下一題按鈕判斷。
} // 結束共用互動處理函式。

function isInside(pointerX, pointerY, box) { // 定義矩形範圍命中判斷函式。
  if (!box) { // 判斷矩形資料是否不存在。
    return false; // 沒有矩形資料時回傳未命中。
  } // 結束矩形存在判斷。
  return pointerX >= box.x && pointerX <= box.x + box.w && pointerY >= box.y && pointerY <= box.y + box.h; // 回傳座標是否位於矩形內。
} // 結束矩形命中判斷函式。

function goToNextQuestion() { // 定義前往下一題或顯示結果的函式。
  if (currentQuestion >= quizQuestions.length - 1) { // 判斷目前是否為最後一題。
    quizFinished = true; // 將畫面切換為結果頁。
    calculateLayout(); // 立即建立最新結果頁版面。
    return; // 結束函式以避免題目索引超出範圍。
  } // 結束最後一題判斷。
  currentQuestion += 1; // 將題目索引增加一題。
  selectedOption = -1; // 清除上一題選取狀態以防止同題重複作答。
  calculateLayout(); // 題號變更後立即重新計算題目版面。
} // 結束前往下一題函式。

function restartQuiz() { // 定義重新開始測驗的函式。
  currentQuestion = 0; // 將題目索引重設為第一題。
  selectedOption = -1; // 清除目前選項選取狀態。
  score = 0; // 將答對題數重設為零。
  quizFinished = false; // 將畫面切換回答題頁。
  calculateLayout(); // 重新開始後立即計算題目版面。
} // 結束重新開始函式。

function windowResized() { // 定義瀏覽器視窗尺寸改變時執行的函式。
  resizeCanvas(Math.max(1, windowWidth), Math.max(1, windowHeight)); // 將畫布調整為不超出瀏覽器視窗的新尺寸。
  applyPageStyles({ elt: document.querySelector("canvas") }); // 重新套用頁面與畫布的防捲動樣式。
  calculateLayout(); // 視窗改變後重新計算全部 layout，而不只重設畫布。
} // 結束視窗尺寸改變函式。

```
:::


---

## 學習3：設定嵌入 Google 字型，網頁文字採用這些字型

https://cfchen58.synology.me/115/week4/stage3/

**這個階段的目標：** 從 Google Fonts 嵌入繁體中文字型，並讓畫布上的題目與選項文字使用這些字型。
**這個階段會修改的檔案：** index.html、sketch.js

### 執行截圖

（把截圖拖曳到這裡，或貼上圖片連結）

![學習3截圖](請貼上截圖)

### 第一次問 AI

```tex!
（逐字貼上你第一次問 AI 的提示詞）
```

### 第二次問 AI

```tex!
（逐字貼上你第二次問 AI 的提示詞）
```

### 第三次問 AI

```tex!
（逐字貼上你第三次問 AI 的提示詞）
```

### 程式碼內容

:::info
:::spoiler 點開貼上學習3的程式碼
```javascript=
//學習3程式碼所在

```
:::


---

## 學習4：設定題庫並抽題顯示題目網頁（CSV 檔案）

https://cfchen58.synology.me/115/week4/stage4/

**這個階段的目標：** 把題目移到 questions.csv，網站讀取題庫後每次隨機抽出 5 題。
**這個階段會修改的檔案：** index.html、sketch.js、questions.csv

### 執行截圖

（把截圖拖曳到這裡，或貼上圖片連結）

![學習4截圖](請貼上截圖)

### 第一次問 AI

```tex!
（逐字貼上你第一次問 AI 的提示詞）
```

### 第二次問 AI

```tex!
（逐字貼上你第二次問 AI 的提示詞）
```

### 第三次問 AI

```tex!
（逐字貼上你第三次問 AI 的提示詞）
```

### 程式碼內容

:::info
:::spoiler 點開貼上學習4的程式碼
```javascript=
//學習4程式碼所在

```
:::


---

## 學習5：利用 Google Sheets 當題庫

https://cfchen58.synology.me/115/week4/stage5/

**這個階段的目標：** 把題庫放在 Google 試算表，網站直接讀取，老師改試算表，網站題目就跟著更新。
**這個階段會修改的檔案：** index.html、sketch.js（questions.csv 當備用題庫）

### 執行截圖

（把截圖拖曳到這裡，或貼上圖片連結）

![學習5截圖](請貼上截圖)

### 第一次問 AI

```tex!
（逐字貼上你第一次問 AI 的提示詞）
```

### 第二次問 AI

```tex!
（逐字貼上你第二次問 AI 的提示詞）
```

### 第三次問 AI

```tex!
（逐字貼上你第三次問 AI 的提示詞）
```

### 程式碼內容

:::info
:::spoiler 點開貼上學習5的程式碼
```javascript=
//學習5程式碼所在

```
:::


---

## 我的心得

這五個學習中，哪一個最困難？你是怎麼解決的？（請寫出實際發生的事）

＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿

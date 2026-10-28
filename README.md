const quizData = [
  {
    question: "下列说法中，正确的是？",
    options: [
      "冰融化是化学变化，因为有液体形成",
      "燃烧是化学变化，因为有新物质生成",
      "铁丝弯曲是化学变化，因为形状变了",
      "纸张被撕烂是化学变化，因为分子变小了"
    ],
    answer: 1
  },
  {
    question: "下列物质中，属于酸的是？",
    options: ["NaOH", "HCl", "NaCl", "CaCO3"],
    answer: 1
  },
  {
    question: "实验中，过滤操作的目的是？",
    options: [
      "把溶液蒸干",
      "分离固体和液体",
      "使反应更快",
      "提高溶液的浓度"
    ],
    answer: 1
  }
];

const questionNumber = document.getElementById("question-number");
const questionText = document.getElementById("question-text");
const optionsContainer = document.getElementById("options");
const result = document.getElementById("result");
const checkAnswerBtn = document.getElementById("check-answer");

let currentIndex = 0;
let selectedAnswer = null;

function renderQuestion() {
  const current = quizData[currentIndex];
  questionNumber.textContent = `题目 ${currentIndex + 1}`;
  questionText.textContent = current.question;
  optionsContainer.innerHTML = "";
  selectedAnswer = null;
  result.textContent = "";
  result.className = "result";

  current.options.forEach((option, index) => {
    const button = document.createElement("button");
    button.type = "button";
    button.className = "option-btn";
    button.textContent = option;
    button.addEventListener("click", () => {
      selectedAnswer = index;
      document.querySelectorAll(".option-btn").forEach((btn) => {
        btn.classList.remove("selected");
      });
      button.classList.add("selected");
    });
    optionsContainer.appendChild(button);
  });
}

checkAnswerBtn.addEventListener("click", () => {
  if (selectedAnswer === null) {
    result.textContent = "请选择一个答案后再检查。";
    result.className = "result error";
    return;
  }

  const current = quizData[currentIndex];
  const isCorrect = selectedAnswer === current.answer;

  if (isCorrect) {
    result.textContent = "正确！继续保持这个好状态。";
    result.className = "result success";
  } else {
    result.textContent = `不对哦，正确答案是：${current.options[current.answer]}`;
    result.className = "result error";
  }

  setTimeout(() => {
    currentIndex = (currentIndex + 1) % quizData.length;
    renderQuestion();
  }, 1400);
});

renderQuestion();

const tabButtons = document.querySelectorAll(".tab-btn");
const tabPanels = document.querySelectorAll(".tab-panel");

tabButtons.forEach((button) => {
  button.addEventListener("click", () => {
    const target = button.dataset.tab;

    tabButtons.forEach((btn) => {
      btn.classList.toggle("active", btn === button);
      btn.setAttribute("aria-selected", String(btn === button));
    });

    tabPanels.forEach((panel) => {
      panel.classList.toggle("active", panel.id === target);
    });
  });
});

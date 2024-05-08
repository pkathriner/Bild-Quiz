# Bild-Quiz
Basis Quiz mit Construct mit Bilddaten für Education Games

let quizData = [];
let currentQuestionIndex = 0;
let score = 0;  // Für die Punktzahl
let totalQuestions = 0;  // Gesamtanzahl der Fragen

window.onload = function() {
  loadQuizData();
};


setupUI()
loadQuizData()
displayQuestion(index)
checkAnswer(selectedIndex)
nextQuestion()
showResults()



function loadQuizData() {
  fetch('https://raw.githubusercontent.com/pkathriner/Bild-Quiz/main/Quiz.json')
    .then(response => response.json())
    .then(data => {
      quizData = data;
      totalQuestions = quizData.length;  // Anzahl der Fragen festlegen
      setupUI();
      displayQuestion(currentQuestionIndex);  // Zeigt die erste Frage beim Start
    })
    .catch(error => console.error('Fehler beim Laden der Quizdaten:', error));
}

function setupUI() {
  document.body.innerHTML = '';  // Löscht den aktuellen Inhalt (falls notwendig)
  
  const questionText = document.createElement('div');
  questionText.id = 'questionText';
  questionText.style.position = 'absolute';
  questionText.style.left = '810px';  // Zentriert bei 1920px Breite
  questionText.style.top = '100px';
  questionText.style.width = '300px';
  questionText.style.height = '50px';
  questionText.style.color = 'black';
  document.body.appendChild(questionText);
  
  const questionImage = document.createElement('img');
  questionImage.id = 'questionImage';
  questionImage.style.position = 'absolute';
  questionImage.style.left = '810px';
  questionImage.style.top = '160px';
  questionImage.style.width = '300px';
  questionImage.style.height = '200px';
  document.body.appendChild(questionImage);

  for (let i = 0; i < 5; i++) {
    const answerButton = document.createElement('button');
    answerButton.id = `answerOption${i}`;
    answerButton.style.position = 'absolute';
    answerButton.style.left = '810px';
    answerButton.style.top = `${360 + i * 50}px`;
    answerButton.style.width = '300px';
    answerButton.style.height = '40px';
    answerButton.onclick = () => checkAnswer(i);
    document.body.appendChild(answerButton);
  }

  const nextButton = document.createElement('button');
  nextButton.id = 'nextButton';
  nextButton.textContent = 'Nächste Frage';
  nextButton.style.position = 'absolute';
  nextButton.style.left = '810px';
  nextButton.style.top = '660px';
  nextButton.style.width = '200px';
  nextButton.style.height = '50px';
  nextButton.onclick = nextQuestion;
  nextButton.style.display = 'none';  // Verstecke den Button zunächst
  document.body.appendChild(nextButton);
}

function displayQuestion(index) {
  const question = quizData[index];
  const questionText = document.getElementById('questionText');
  const questionImage = document.getElementById('questionImage');

  questionText.textContent = question.question;
  questionImage.src = question.image_url;

  for (let i = 0; i < question.options.length; i++) {
    const answerButton = document.getElementById(`answerOption${i}`);
    answerButton.textContent = question.options[i];
    answerButton.style.display = 'block';
  }

  for (let i = question.options.length; i < 5; i++) {
const answerButton = document.getElementById(`answerOption${i}`);
if (answerButton) {
answerButton.style.display = 'none'; // Verstecke Buttons, die nicht benötigt werden
}
}

const nextButton = document.getElementById('nextButton');
nextButton.style.display = 'none'; // Verstecke den "Nächste Frage" Button
}

function checkAnswer(selectedIndex) {
const question = quizData[currentQuestionIndex];
if (selectedIndex === question.correct) {
score++; // Punktzahl erhöhen, wenn die Antwort richtig ist
}
const nextButton = document.getElementById('nextButton');
nextButton.style.display = 'block'; // Zeige den "Nächste Frage" Button an
}

function nextQuestion() {
currentQuestionIndex++;
if (currentQuestionIndex < quizData.length) {
displayQuestion(currentQuestionIndex); // Lade die nächste Frage
} else {
showResults(); // Ergebnisse anzeigen, wenn das Quiz vorbei ist
}
}

function showResults() {
const percentage = (score / totalQuestions) * 100;
let feedback = '';
if (percentage < 50) {
feedback = 'Du solltest noch etwas üben.';
} else if (percentage >= 50 && percentage <= 80) {
feedback = 'Du bist schon ziemlich fit.';
} else if (percentage > 80) {
feedback = 'Du bist ein Ranger.';
}

document.body.innerHTML = `<div style="position: absolute; left: 810px; top: 360px; width: 300px;"> Du hast ${score} von ${totalQuestions} Punkten erreicht (${percentage}%).<br>${feedback} </div>` ;
}



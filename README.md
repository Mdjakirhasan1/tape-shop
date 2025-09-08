# tape-shop
tap shop
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Tap Game Leaderboard</title>
<style>
  body {
    margin: 0;
    padding: 0;
    font-family: Arial, sans-serif;
    background: linear-gradient(135deg, #ff9a9e, #fad0c4, #a18cd1);
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
    text-align: center;
  }
  h1 { color: #fff; margin-bottom: 10px; }
  #tapButton {
    padding: 25px 60px;
    font-size: 30px;
    background: #ff6f61;
    color: white;
    border: none;
    border-radius: 20px;
    cursor: pointer;
    box-shadow: 0 5px #c0392b;
    transition: all 0.1s ease-in-out;
  }
  #tapButton:active {
    transform: translateY(5px);
    box-shadow: 0 2px #c0392b;
  }
  #score, #timer, #level, #highscore {
    margin-top: 15px;
    font-size: 22px;
    color: #fff;
  }
</style>
</head>
<body>

<h1>🔥 Tap Game Leaderboard 🔥</h1>
<button id="tapButton">Tap Me!</button>
<div id="score">Score: 0</div>
<div id="timer">Time Left: 10s</div>
<div id="level">Level: 1</div>
<div id="highscore">Highscore: 0</div>

<audio id="clickSound" src="https://www.soundjay.com/button/beep-07.mp3"></audio>

<script>
  let score = 0;
  let timeLeft = 10;
  let level = 1;
  const maxLevels = 3;
  let gameOver = false;

  const scoreDisplay = document.getElementById('score');
  const timerDisplay = document.getElementById('timer');
  const levelDisplay = document.getElementById('level');
  const highscoreDisplay = document.getElementById('highscore');
  const button = document.getElementById('tapButton');
  const clickSound = document.getElementById('clickSound');

  // Get highscore from localStorage
  let highscore = localStorage.getItem('tapGameHighscore') || 0;
  highscoreDisplay.textContent = "Highscore: " + highscore;

  function startLevel(levelTime) {
    timeLeft = levelTime;
    timerDisplay.textContent = "Time Left: " + timeLeft + "s";
    gameOver = false;
    button.disabled = false;

    const timer = setInterval(() => {
      if(timeLeft > 0){
        timeLeft--;
        timerDisplay.textContent = "Time Left: " + timeLeft + "s";
      } else {
        clearInterval(timer);
        gameOver = true;
        button.disabled = true;

        // Update highscore if necessary
        if(score > highscore){
          highscore = score;
          localStorage.setItem('tapGameHighscore', highscore);
          highscoreDisplay.textContent = "Highscore: " + highscore;
        }

        if(level < maxLevels){
          alert(`⏱️ Level ${level} over! Score: ${score}\nNext Level Unlocked!`);
          level++;
          levelDisplay.textContent = "Level: " + level;
          startLevel(10 - (level-1)*2); // Next level shorter time
        } else {
          alert(`🏆 Game Over! Final Score: ${score}`);
        }
      }
    }, 1000);
  }

  button.addEventListener('click', () => {
    if(!gameOver){
      score++;
      scoreDisplay.textContent = "Score: " + score;
      clickSound.currentTime = 0;
      clickSound.play();
      // Button color animation
      button.style.background = `hsl(${Math.random()*360}, 70%, 60%)`;
    }
  });

  // Start the first level
  startLevel(10);
</script>

</body>
</html>

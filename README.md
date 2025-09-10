# Axiom.Brewery.game
<!DOCTYPE html>
<html lang="cs">
    <script src="https://code.jquery.com/jquery-1.12.4.min.js"></script>
<body>
    <div></div>
</body>
<script>
    $('div').html("<div>1</div>");
</script>
<head>
<meta charset="UTF-8">
<title>Axiom Halloween Game</title>
<style>
body {
  margin: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  color: #111; /* černý text */
  text-shadow: 2px 2px 8px #ff6600, 0 0 6px #b35400;
  font-family: 'Trebuchet MS', 'Creepster', 'Permanent Marker', cursive, sans-serif;
  background: 
    repeating-linear-gradient(135deg, #b35400 0px, #ffb347 100px),
    url('https://axiombrewery.cz/wp-content/uploads/2020/09/logo-axiom-brewery-white.png') 40px 40px/80px 80px repeat,
    url('https://em-content.zobj.net/source/apple/354/jack-o-lantern_1f383.png') 140px 140px/80px 80px repeat;
}
h1 { color: #b35400; margin-bottom: 5px; }
#controls { margin: 10px; }
button {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  background: #b35400;
  color: #fff;
  font-size: 16px;
  cursor: pointer;
}
button:hover { background: #d35400; }
#gameArea {
  position: relative;
  width: 700px;
  height: 450px;
  border: 3px solid #b35400;
  overflow: hidden;
  border-radius: 10px;
  background: linear-gradient(to bottom, #1a0a2a 60%, #2a0a1a 100%);
  box-shadow: 0 0 60px 10px #000 inset;
}

/* Jehličnaté stromy jako pozadí */
#gameArea::before {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 180px;
  background: url('https://pngimg.com/uploads/christmas_tree/christmas_tree_PNG18894.png') repeat-x left bottom;
  background-size: 120px 180px;
  z-index: 1;
  opacity: 0.7;
  pointer-events: none;
}

/* Měsíc v pravém horním rohu */
#gameArea::after {
  content: '';
  position: absolute;
  top: 25px;
  right: 25px;
  width: 90px;
  height: 90px;
  background: radial-gradient(circle, #ffffcc 60%, #ffaa00 100%);
  border-radius: 50%;
  z-index: 2;
  box-shadow: 0 0 40px 10px #ffaa00;
  pointer-events: none;
}

/* Mlžný efekt */
#fog {
  position: absolute;
  top: 10; left: 10;
  width: 100%; height: 100%;
  pointer-events: none;
  z-index: 5;
  background: url('https://pngimg.com/uploads/fog/fog_PNG19.png');
  opacity: 0.60;
  mix-blend-mode: lighten;
  animation: fogMove 10s linear infinite alternate;
}
@keyframes fogMove {
  0% { background-position: 0 0; }
  100% { background-position: 200px 0; }
}

/* Chodící dýně uprostřed */
#walking-pumpkin {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  font-size: 60px;
  z-index: 5;
  animation: pumpkinWalk 2.5s infinite linear;
  color: #b35400; /* tmavší dýňová barva */
}
@keyframes pumpkinWalk {
  0%   { transform: translate(-50%, -50%) scaleX(1); }
  25%  { transform: translate(-45%, -50%) scaleX(-1); }
  50%  { transform: translate(-50%, -50%) scaleX(1); }
  75%  { transform: translate(-55%, -50%) scaleX(-1); }
  100% { transform: translate(-50%, -50%) scaleX(1); }
}

.pumpkin { background: purple; color: #b35400; } /* tmavší dýňová barva */

.bat {
  position: absolute;
  width: 40px;
  height: 40px;
  background: #333;
  clip-path: polygon(50% 0%, 100% 100%, 0% 100%);
  animation: fly 4s infinite;
}

@keyframes fly {
  0% { transform: translateY(0) rotate(0); }
  50% { transform: translateY(-20px) rotate(-20deg); }
  100% { transform: translateY(0) rotate(0); }
}

.item {
  position: absolute;
  width: 70px;
  height: 70px;
  font-size: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  box-shadow: 0 0 10px #000;
  cursor: pointer;
  user-select: none;
  transition: transform 0.1s;
}
.item:active {
  transform: scale(0.9);
}
#score {
  font-size: 2em;
  font-weight: bold;
  color: #b35400;
  padding: 5px 20px;
  background: rgba(0,0,0,0.3);
  border-radius: 8px;
  margin-left: 10px;
}

#gameArea::before,
#gameArea::after {
  pointer-events: none;
}
h1, p, #score, #highscore, #timer, button, .item {
  color: #111 !important; /* černý text pro všechny hlavní prvky */
  text-shadow: 2px 2px 8px #ff6600, 0 0 6px #b35400;
  font-family: 'Trebuchet MS', 'Creepster', 'Permanent Marker', cursive, sans-serif;
  letter-spacing: 2px;
}
</style>
</head>
<body>
  
  <h1>Axiom Brewery 🎃 Halloween</h1>
  <p> Nasbírej co nejvíce pivních sklenic </p>
<p>
  Skóre: <span id="score">0</span>
  &nbsp;|&nbsp;
  Nejlepší skóre: <span id="highscore">0</span>
  &nbsp;|&nbsp;
  Čas: <span id="timer">120</span> s
</p>
<div id="controls">
  <button id="startBtn">Start</button>
</div>
<!-- Hrací pole s halloweenskou tématikou -->
<div id="gameArea" style="position:relative; width:700px; height:450px; border:3px solid #b35400; overflow:hidden; border-radius:10px; background:linear-gradient(to bottom, #1a0a2a 60%, #2a0a1a 100%); box-shadow:0 0 60px 10px #000 inset;">
  <!-- Mlha -->
  <div id="fog" style="position:absolute;top:0;left:0;width:100%;height:100%;pointer-events:none;z-index:5;background:url('https://pngimg.com/uploads/fog/fog_PNG19.png');opacity:0.60;mix-blend-mode:lighten;animation:fogMove 10s linear infinite alternate;"></div>
  <!-- Jehličnaté stromy -->
  <div style="position:absolute;bottom:0;left:0;width:100%;height:180px;background:url('https://pngimg.com/uploads/christmas_tree/christmas_tree_PNG18894.png') repeat-x left bottom;background-size:120px 180px;z-index:1;opacity:0.7;pointer-events:none;"></div>
  <!-- Měsíc -->
  <div style="position:absolute;top:25px;right:25px;width:90px;height:90px;background:radial-gradient(circle,#ffffcc 60%,#ffaa00 100%);border-radius:50%;z-index:2;box-shadow:0 0 40px 10px #ffaa00;pointer-events:none;"></div>
  <!-- Chodící dýně -->
  <div id="walking-pumpkin" style="position:absolute;left:50%;top:50%;transform:translate(-50%,-50%);font-size:60px;z-index:5;animation:pumpkinWalk 2.5s infinite linear;color:#b35400;">🎃</div>
  <!-- Netopýři -->
  <div id="bats"></div>
</div>

<!-- Animace pro mlhu a dýni -->
<style>
@keyframes fogMove {
  0% { background-position: 0 0; }
  100% { background-position: 200px 0; }
}
@keyframes pumpkinWalk {
  0%   { transform: translate(-50%, -50%) scaleX(1); }
  25%  { transform: translate(-45%, -50%) scaleX(-1); }
  50%  { transform: translate(-50%, -50%) scaleX(1); }
  75%  { transform: translate(-55%, -50%) scaleX(-1); }
  100% { transform: translate(-50%, -50%) scaleX(1); }
}
.bat-img {
  position: absolute;
  width: 60px;
  height: auto;
  pointer-events: none;
  z-index: 4;
  animation: batFly 6s linear infinite;
}
@keyframes batFly {
  0%   { transform: translateY(0) scaleX(1); opacity: 0.8; }
  20%  { transform: translateY(-40px) scaleX(-1); opacity: 1; }
  40%  { transform: translateY(20px) scaleX(1); opacity: 0.9; }
  60%  { transform: translateY(-30px) scaleX(-1); opacity: 1; }
  80%  { transform: translateY(10px) scaleX(1); opacity: 0.8; }
  100% { transform: translateY(0) scaleX(-1); opacity: 0.8; }
}
</style>

<!-- Generování netopýrů do hracího pole -->
<script>
function createBats(count = 8) {
  const batsContainer = document.getElementById("bats");
  batsContainer.innerHTML = "";
  const gameArea = document.getElementById("gameArea");
  for (let i = 0; i < count; i++) {
    const bat = document.createElement("img");
    bat.src = "https://pngimg.com/uploads/bat/bat_PNG12.png";
    bat.className = "bat-img";
    bat.style.top = Math.random() * (gameArea.clientHeight - 60) + "px";
    bat.style.left = Math.random() * (gameArea.clientWidth - 60) + "px";
    bat.style.animationDelay = (Math.random() * 6) + "s";
    batsContainer.appendChild(bat);
  }
}
window.addEventListener("DOMContentLoaded", () => {
  createBats(8);
});
</script>

<script>
const gameArea = document.getElementById("gameArea");
const scoreDisplay = document.getElementById("score");
const highscoreDisplay = document.getElementById("highscore");
const startBtn = document.getElementById("startBtn");
const timerDisplay = document.getElementById("timer");

let score = 0;
let highscore = localStorage.getItem("highscore") ? Number(localStorage.getItem("highscore")) : 0;
highscoreDisplay.textContent = highscore;
let gameInterval;
let spawnTime = 1000;
let timerInterval;
let timeLeft = 120;

// ✅ ZDE DEFINUJEME RŮZNÉ DRUHY PIVA (můžeš později nahradit obrázky/logy)
const beers = [
  { name: "Axiom Lager", color: "orange", emoji: "🍺" },
  { name: "Axiom IPA", color: "yellow", emoji: "🍻" },
  { name: "Axiom Stout", color: "brown", emoji: "🍺" },
  { name: "Axiom Pale Ale", color: "gold", emoji: "🍺" }
];

function spawnItem() {
  // Odstraň předchozí ikonu (pokud existuje)
  document.querySelectorAll('.item').forEach(el => el.remove());

  const item = document.createElement("div");
  item.classList.add("item");

  // Náhodně vybereme typ objektu
  const types = [
    { type: "beer", emoji: "🍺", color: "orange" },
    { type: "beer", emoji: "🍻", color: "yellow" },
    { type: "beer", emoji: "🍺", color: "brown" },
    { type: "beer", emoji: "🍺", color: "gold" },
    { type: "pumpkin", emoji: "🎃", color: "purple" },
    { type: "ghost", emoji: "👻", color: "#e0e0ff" }
  ];
  const chosen = types[Math.floor(Math.random() * types.length)];

  item.dataset.type = chosen.type;
  item.textContent = chosen.emoji;
  item.style.background = chosen.color;
  if (chosen.type === "ghost") {
    item.style.color = "#333";
    item.style.boxShadow = "0 0 20px #fff";
  }

  const x = Math.random() * (gameArea.clientWidth - 70);
  const y = Math.random() * (gameArea.clientHeight - 70);
  item.style.left = `${x}px`;
  item.style.top = `${y}px`;

  item.addEventListener("click", () => {
    if (item.dataset.type === "beer") {
      score++;
      scoreDisplay.textContent = score;
    } else if (item.dataset.type === "pumpkin") {
      score--;
      scoreDisplay.textContent = score;
      if (score < 0) endGame("negative");
    } else if (item.dataset.type === "ghost") {
      endGame("ghost");
    }
    if (gameArea.contains(item)) gameArea.removeChild(item);
  });

  gameArea.appendChild(item);

  // Nastav čas podle skóre
  let timeout = 1000;
  if (score > 9 && score <= 20) timeout = 800;
  if (score > 20) timeout = 500;

  setTimeout(() => {
    if (gameArea.contains(item)) gameArea.removeChild(item);
  }, timeout);
}

function spawnLoop() {
  spawnItem();
  gameInterval = setTimeout(spawnLoop, 1000); // vždy 1s mezi ikonami
}

function endGame(reason) {
  if (score > highscore) {
    highscore = score;
    localStorage.setItem("highscore", highscore);
    highscoreDisplay.textContent = highscore;
  }
  if (gameInterval) clearTimeout(gameInterval);
  if (timerInterval) clearInterval(timerInterval);

  document.querySelectorAll('.item').forEach(el => el.remove());

  let message = "";
  if (reason === "ghost") {
    message = "Jsi vystrašený k smrti.";
  } else if (reason === "timeout") {
    message = `Konec hry! Čas vypršel.\nTvé skóre: ${score}`;
  } else {
    message = `Konec hry! Skóre je záporné.\nTvé skóre: ${score}`;
  }

  // Vytvoř nabídku Hrát znovu
  const overlay = document.createElement("div");
  overlay.style.position = "fixed";
  overlay.style.top = "0";
  overlay.style.left = "0";
  overlay.style.width = "100vw";
  overlay.style.height = "100vh";
  overlay.style.background = "rgba(0,0,0,0.85)";
  overlay.style.display = "flex";
  overlay.style.flexDirection = "column";
  overlay.style.alignItems = "center";
  overlay.style.justifyContent = "center";
  overlay.style.zIndex = "9999";

  const msg = document.createElement("div");
  msg.style.color = "#ff6600";
  msg.style.fontSize = "2em";
  msg.style.marginBottom = "30px";
  msg.style.textAlign = "center";
  msg.textContent = message;

  const againBtn = document.createElement("button");
  againBtn.textContent = "Hrát znovu";
  againBtn.style.padding = "15px 40px";
  againBtn.style.fontSize = "1.2em";
  againBtn.style.background = "#ff6600";
  againBtn.style.color = "#fff";
  againBtn.style.border = "none";
  againBtn.style.borderRadius = "8px";
  againBtn.style.cursor = "pointer";
  againBtn.style.boxShadow = "0 0 10px #000";

  againBtn.onclick = () => {
    overlay.remove();
    startGame();
  };

  overlay.appendChild(msg);
  overlay.appendChild(againBtn);
  document.body.appendChild(overlay);
}

function startGame() {
  score = 0;
  scoreDisplay.textContent = score;
  spawnTime = 1000;
  timeLeft = 120;
  timerDisplay.textContent = timeLeft;
  if (gameInterval) clearTimeout(gameInterval);
  if (timerInterval) clearInterval(timerInterval);
  spawnLoop();

  // Ukončení hry po 2 minutách
  timerInterval = setInterval(() => {
    timeLeft--;
    timerDisplay.textContent = timeLeft;
    if (timeLeft <= 0) endGame("timeout");
  }, 1000);

  // Skryj tlačítko start během hry
  startBtn.style.display = "none";
}

function createBats(count = 8) {
  const batsContainer = document.getElementById("bats");
  batsContainer.innerHTML = "";
  const gameArea = document.getElementById("gameArea");
  for (let i = 0; i < count; i++) {
    const bat = document.createElement("img");
    bat.src = "https://pngimg.com/uploads/bat/bat_PNG12.png";
    bat.className = "bat-img";
    bat.style.top = Math.random() * (gameArea.clientHeight - 60) + "px";
    bat.style.left = Math.random() * (gameArea.clientWidth - 60) + "px";
    bat.style.animationDelay = (Math.random() * 6) + "s";
    batsContainer.appendChild(bat);
  }
}

// Vytvoř netopýry při načtení stránky
window.addEventListener("DOMContentLoaded", () => {
  createBats(8);
});

startBtn.addEventListener("click", startGame);
</script>
</body>
</html>

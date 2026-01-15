<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Happy Makara Sankranti 🪁</title>
<style>
  body {
    margin: 0;
    background: #87CEEB;
    overflow: hidden;
  }
  canvas {
    display: block;
    margin: auto;
    background: #87CEEB;
  }
</style>
</head>
<body>

<canvas id="canvas" width="900" height="600"></canvas>

<script>
const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");

const WIDTH = canvas.width;
const HEIGHT = canvas.height;

// ---------------- UTIL ----------------
function drawCircle(x, y, r, color) {
  ctx.fillStyle = color;
  ctx.beginPath();
  ctx.arc(x, y, r, 0, Math.PI * 2);
  ctx.fill();
}

// ---------------- LAND ----------------
function drawLand() {
  ctx.fillStyle = "#228B22";
  ctx.fillRect(0, HEIGHT - 140, WIDTH, 140);
}

// ---------------- SUN ----------------
function drawSun() {
  drawCircle(780, 100, 35, "gold");
  ctx.strokeStyle = "orange";
  ctx.lineWidth = 2;
  for (let a = 0; a < 360; a += 20) {
    let r1 = 35, r2 = 55;
    let x1 = 780 + Math.cos(a * Math.PI / 180) * r1;
    let y1 = 100 + Math.sin(a * Math.PI / 180) * r1;
    let x2 = 780 + Math.cos(a * Math.PI / 180) * r2;
    let y2 = 100 + Math.sin(a * Math.PI / 180) * r2;
    ctx.beginPath();
    ctx.moveTo(x1, y1);
    ctx.lineTo(x2, y2);
    ctx.stroke();
  }
}

// ---------------- CLOUD ----------------
function drawCloud(x, y) {
  drawCircle(x, y, 18, "white");
  drawCircle(x + 25, y - 10, 22, "white");
  drawCircle(x + 50, y, 18, "white");
}

// ---------------- KITE ----------------
function drawKite(x, y, color) {
  ctx.fillStyle = color;
  ctx.beginPath();
  ctx.moveTo(x, y);
  ctx.lineTo(x + 20, y + 20);
  ctx.lineTo(x, y + 40);
  ctx.lineTo(x - 20, y + 20);
  ctx.closePath();
  ctx.fill();

  // Tail
  ctx.strokeStyle = "black";
  for (let i = 0; i < 4; i++) {
    ctx.beginPath();
    ctx.arc(x, y + 40 + i * 10, 2, 0, Math.PI * 2);
    ctx.stroke();
  }
}

// ---------------- TEXT ----------------
function drawText() {
  ctx.fillStyle = "darkred";
  ctx.font = "bold 26px Arial";
  ctx.textAlign = "center";
  ctx.fillText("Happy Makara Sankranti 🪁", WIDTH / 2, 40);
}

// ---------------- DATA ----------------
const kiteColors = [
  "red","blue","purple","orange","magenta",
  "green","cyan","yellow","pink","brown",
  "navy","gold"
];

const kites = [];
const KITE_COUNT = 25;

for (let i = 0; i < KITE_COUNT; i++) {
  kites.push({
    x: Math.random() * WIDTH,
    baseY: 80 + Math.random() * 200,
    speed: 1 + Math.random() * 2.5,
    phase: Math.random() * Math.PI * 2,
    color: kiteColors[Math.floor(Math.random() * kiteColors.length)]
  });
}

const clouds = [
  { x: -200, y: 150, speed: 0.8 },
  { x: -400, y: 200, speed: 1.0 },
  { x: -600, y: 170, speed: 0.6 },
  { x: -800, y: 230, speed: 1.2 },
  { x: -1000, y: 180, speed: 0.9 }
];

// ---------------- ANIMATION ----------------
function animate(time) {
  ctx.clearRect(0, 0, WIDTH, HEIGHT);

  drawLand();
  drawSun();

  // Clouds
  clouds.forEach(c => {
    drawCloud(c.x, c.y);
    c.x += c.speed;
    if (c.x > WIDTH + 100) c.x = -200;
  });

  // Kites
  kites.forEach(k => {
    let y = k.baseY + Math.sin((k.x * 0.03) + k.phase) * 20;
    drawKite(k.x, y, k.color);
    k.x += k.speed;
    if (k.x > WIDTH + 50) k.x = -50;
  });

  drawText();

  requestAnimationFrame(animate);
}

animate();
</script>

</body>
</html>

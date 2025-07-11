<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>메모리 게임</title>
  <style>
    body {
      font-family: sans-serif;
      text-align: center;
      background: #282c34;
      color: white;
      margin: 0;
      padding: 20px;
    }
    #game-board {
      display: grid;
      grid-template-columns: repeat(10, 60px);
      grid-gap: 10px;
      justify-content: center;
      margin-top: 20px;
    }
    .card {
      width: 60px;
      height: 80px;
      background: #ccc;
      color: transparent;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 24px;
      cursor: pointer;
      border-radius: 8px;
      user-select: none;
    }
    .card.revealed {
      background: #61dafb;
      color: #000;
    }
    #score {
      font-size: 20px;
      margin-top: 10px;
    }
    canvas {
      position: fixed;
      top: 0;
      left: 0;
      pointer-events: none;
    }
  </style>
</head>
<body>
  <h1>🎴 메모리 게임</h1>
  <div id="score">점수: 0</div>
  <div id="game-board"></div>
  <canvas id="fireworks"></canvas>

  <script>
    const board = document.getElementById('game-board');
    const scoreDisplay = document.getElementById('score');
    const totalCards = 50;
    const matchCount = 5;
    const typeCount = totalCards / matchCount;
    const cardValues = [];

    let revealedCards = [];
    let score = 0;
    let matched = [];

    // 카드 값 생성
    for (let i = 0; i < typeCount; i++) {
      for (let j = 0; j < matchCount; j++) {
        cardValues.push(i);
      }
    }

    // 카드 섞기
    cardValues.sort(() => Math.random() - 0.5);

    // 카드 생성
    cardValues.forEach((value, index) => {
      const card = document.createElement('div');
      card.classList.add('card');
      card.dataset.index = index;
      card.dataset.value = value;
      card.innerText = value;
      board.appendChild(card);
    });

    function resetRevealed() {
      revealedCards.forEach(card => card.classList.remove('revealed'));
      revealedCards = [];
    }

    function checkMatch() {
      const first = revealedCards[0];
      const second = revealedCards[1];
      if (first.dataset.value === second.dataset.value) {
        matched.push(first.dataset.value);
        score += 5;
        scoreDisplay.innerText = `점수: ${score}`;
        revealedCards = [];

        // 모두 맞췄을 때
        if (matched.length === typeCount) {
          launchFireworks();
        }
      } else {
        setTimeout(resetRevealed, 1000);
      }
    }

    board.addEventListener('click', e => {
      const card = e.target;
      if (!card.classList.contains('card') || card.classList.contains('revealed') || revealedCards.length >= 2) return;
      card.classList.add('revealed');
      revealedCards.push(card);

      if (revealedCards.length === 2) {
        checkMatch();
      }
    });

    // 🎆 폭죽 애니메이션
    function launchFireworks() {
      const canvas = document.getElementById('fireworks');
      const ctx = canvas.getContext('2d');
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;

      let particles = [];

      function createFirework() {
        const x = Math.random() * canvas.width;
        const y = Math.random() * canvas.height / 2;
        for (let i = 0; i < 50; i++) {
          particles.push({
            x, y,
            angle: Math.random() * Math.PI * 2,
            speed: Math.random() * 4 + 1,
            radius: Math.random() * 3 + 1,
            alpha: 1
          });
        }
      }

      function animate() {
        ctx.fillStyle = 'rgba(0, 0, 0, 0.2)';
        ctx.fillRect(0, 0, canvas.width, canvas.height);
        particles.forEach((p, i) => {
          p.x += Math.cos(p.angle) * p.speed;
          p.y += Math.sin(p.angle) * p.speed;
          p.alpha -= 0.01;
          if (p.alpha <= 0) particles.splice(i, 1);
          ctx.beginPath();
          ctx.arc(p.x, p.y, p.radius, 0, Math.PI * 2);
          ctx.fillStyle = `rgba(255, 255, 255, ${p.alpha})`;
          ctx.fill();
        });
        if (particles.length > 0) requestAnimationFrame(animate);
      }

      for (let i = 0; i < 5; i++) {
        setTimeout(createFirework, i * 400);
      }
      animate();
    }
  </script>
</body>
</html>

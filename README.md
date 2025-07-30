<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>3D Car Racing Game</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { overflow: hidden; background: #222; }

    #gameArea {
      position: relative;
      width: 100vw;
      height: 100vh;
      perspective: 1000px;
      overflow: hidden;
    }

    .road {
      position: absolute;
      top: 0;
      left: 50%;
      transform: translateX(-50%);
      width: 300px;
      height: 2000px;
      background: repeating-linear-gradient(
        to bottom,
        #444,
        #444 40px,
        #333 40px,
        #333 80px
      );
      transform-style: preserve-3d;
      animation: moveRoad 2s linear infinite;
    }

    @keyframes moveRoad {
      0% { transform: translateX(-50%) translateY(0); }
      100% { transform: translateX(-50%) translateY(-80px); }
    }

    .car {
      position: absolute;
      bottom: 100px;
      left: 50%;
      transform: translateX(-50%);
      width: 50px;
      height: 100px;
      background: red;
      border-radius: 10px;
      z-index: 10;
    }

    .obstacle {
      position: absolute;
      width: 50px;
      height: 100px;
      background: yellow;
      top: -100px;
      z-index: 5;
    }
  </style>
</head>
<body>

<div id="gameArea">
  <div class="road"></div>
  <div class="car" id="playerCar"></div>
</div>

<script>
  const gameArea = document.getElementById('gameArea');
  const playerCar = document.getElementById('playerCar');
  let carX = window.innerWidth / 2;

  // Move car left/right
  document.addEventListener('keydown', (e) => {
    if (e.key === 'ArrowLeft') {
      carX -= 20;
    } else if (e.key === 'ArrowRight') {
      carX += 20;
    }
    playerCar.style.left = `${carX}px`;
  });

  // Create obstacles
  function createObstacle() {
    const obs = document.createElement('div');
    obs.classList.add('obstacle');
    obs.style.left = `${Math.random() * (window.innerWidth - 60)}px`;
    gameArea.appendChild(obs);

    let pos = -100;
    const move = setInterval(() => {
      pos += 5;
      obs.style.top = `${pos}px`;

      // Collision detection
      const obsRect = obs.getBoundingClientRect();
      const carRect = playerCar.getBoundingClientRect();

      if (
        obsRect.top < carRect.bottom &&
        obsRect.bottom > carRect.top &&
        obsRect.left < carRect.right &&
        obsRect.right > carRect.left
      ) {
        alert("Game Over!");
        clearInterval(move);
        window.location.reload();
      }

      // Remove obstacle if out of view
      if (pos > window.innerHeight) {
        obs.remove();
        clearInterval(move);
      }
    }, 20);
  }

  // Repeat obstacles
  setInterval(createObstacle, 1500);
</script>

</body>
</html>

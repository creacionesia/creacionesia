<div id="gameWrapper" style="max-width:500px;margin:10px auto;text-align:center;background:#1a1a2e;padding:10px;">
  <h1 style="font-family:Arial,sans-serif;font-size:1.5em;color:#00ff9f;margin:0 0 5px;">El Juego de los Gafos</h1>
  <canvas id="gameCanvas" width="300" height="300" style="border:2px solid #e94560;background:#0f3460;"></canvas>
  <div id="scoreDisplay" style="font-family:Arial,sans-serif;font-size:1em;color:#00ff9f;margin:5px 0;">Puntaje: 0</div>
  <div id="gameOverScreen" style="display:none;position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);background:#1a1a2e;padding:10px;color:#00ff9f;border:2px solid #e94560;">
    <h2 style="font-family:Arial,sans-serif;font-size:1.2em;color:#e94560;margin:0 0 10px;">¡Perdiste por gafa!</h2>
    <button id="restartButton" style="background:#e94560;color:#fff;border:none;padding:5px 20px;font-size:1em;">Reiniciar</button>
  </div>
<script type="text/javascript">
<![CDATA[
(function() {
  function initGame() {
    var canvas = document.getElementById('gameCanvas');
    if (!canvas || !canvas.getContext) {
      console.log('ERROR: Canvas no encontrado o no soportado');
      return;
    }
    var ctx = canvas.getContext('2d');
    var restartButton = document.getElementById('restartButton');
    var gameOverScreen = document.getElementById('gameOverScreen');
    var scoreDisplay = document.getElementById('scoreDisplay');

    console.log('1. Juego inicializado');

    var gridSize = 20;
    var tileCount = canvas.width / gridSize;
    var snake = [{ x: 10, y: 10 }];
    var food = { x: 15, y: 15 };
    var dx = 0;
    var dy = 0;
    var score = 0;
    var gameLoop = null;

    ctx.fillStyle = '#0f3460';
    ctx.fillRect(0, 0, canvas.width, canvas.height);
    ctx.fillStyle = '#fff';
    ctx.font = '12px Arial';
    ctx.fillText('Cargando juego...', 10, 20);

    function drawGame() {
      ctx.fillStyle = '#0f3460';
      ctx.fillRect(0, 0, canvas.width, canvas.height);
      ctx.fillStyle = '#e94560';
      ctx.fillRect(food.x * gridSize, food.y * gridSize, gridSize - 2, gridSize - 2);
      var head = { x: snake[0].x + dx, y: snake[0].y + dy };
      snake.unshift(head);
      if (head.x === food.x && head.y === food.y) {
        score += 10;
        scoreDisplay.textContent = 'Puntaje: ' + score;
        generateFood();
      } else {
        snake.pop();
      }
      snake.forEach(function(segment) {
        ctx.fillStyle = '#00ff9f';
        ctx.fillRect(segment.x * gridSize, segment.y * gridSize, gridSize - 2, gridSize - 2);
      });
      if (head.x < 0 || head.x >= tileCount || head.y < 0 || head.y >= tileCount || snake.slice(1).some(function(segment) { return segment.x === head.x && segment.y === head.y; })) {
        gameOver();
      }
    }

    function generateFood() {
      food.x = Math.floor(Math.random() * tileCount);
      food.y = Math.floor(Math.random() * tileCount);
      if (snake.some(function(segment) { return segment.x === food.x && segment.y === food.y; })) {
        generateFood();
      }
    }

    function gameOver() {
      if (gameLoop) {
        clearInterval(gameLoop);
        gameLoop = null;
      }
      gameOverScreen.style.display = 'block';
      console.log('3. Game Over');
    }

    function resetGame() {
      snake = [{ x: 10, y: 10 }];
      food = { x: 15, y: 15 };
      dx = 0;
      dy = 0;
      score = 0;
      scoreDisplay.textContent = 'Puntaje: ' + score;
      gameOverScreen.style.display = 'none';
      ctx.fillStyle = '#0f3460';
      ctx.fillRect(0, 0, canvas.width, canvas.height);
      if (!gameLoop) {
        gameLoop = setInterval(drawGame, 100);
      }
      console.log('2. Juego comenzado');
    }

    if (restartButton) {
      restartButton.addEventListener('click', resetGame);
    } else {
      console.log('ERROR: restartButton no encontrado');
    }

    document.addEventListener('keydown', function(e) {
      switch (e.key) {
        case 'ArrowUp':
          if (dy != 1) { dx = 0; dy = -1; }
          break;
        case 'ArrowDown':
          if (dy != -1) { dx = 0; dy = 1; }
          break;
        case 'ArrowLeft':
          if (dx != 1) { dx = -1; dy = 0; }
          break;
        case 'ArrowRight':
          if (dx != -1) { dx = 1; dy = 0; }
          break;
      }
    });

    resetGame();
  }

  setTimeout(function() {
    initGame();
  }, 1000);
})();
]]>
</script>
</div>

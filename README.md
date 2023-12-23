<!DOCTYPE html>
<html>
<head>
 <title>2048</title>
 <style>
    .container {
      width: 400px;
      height: 400px;
      margin: 0 auto;
      border: 2px solid #333;
      display: grid;
      grid-template-columns: repeat(4, 100px);
      grid-template-rows: repeat(4, 100px);
    }
    .tile {
      width: 100px;
      height: 100px;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 36px;
      color: #333;
      border: 1px solid #999;
      background-color: #eee;
    }
 </style>
</head>
<body>
 <div class="container"></div>
 <script>
    const container = document.querySelector('.container');

    const board = [
      [0, 0, 0, 0],
      [0, 0, 0, 0],
      [0, 0, 0, 0],
      [0, 0, 0, 0]
    ];

    const score = document.createElement('p');
    score.textContent = 'Score: 0';
    score.id = 'score';
    document.body.appendChild(score);

    function drawBoard() {
      for (let i = 0; i < 4; i++) {
        for (let j = 0; j < 4; j++) {
          const tile = document.createElement('div');
          tile.textContent = board[i][j] || '';
          tile.className = 'tile';
          container.appendChild(tile);
        }
      }
    }

    function addTile() {
      const emptyCells = [];

      for (let i = 0; i < 4; i++) {
        for (let j = 0; j < 4; j++) {
          if (board[i][j] === 0) {
            emptyCells.push({ x: i, y: j });
          }
        }
      }

      if (emptyCells.length > 0) {
        const randomCell = emptyCells[Math.floor(Math.random() * emptyCells.length)];
        board[randomCell.x][randomCell.y] = Math.random() > 0.875 ? 4 : 2;
      }
    }

    function moveBoard(direction) {
      let hasChanged = false;

      if (direction === 'up') {
        for (let i = 0; i < 4; i++) {
          for (let j = 0; j < 4; j++) {
            if (board[j][i]) {
              let newJ = j;

              while (newJ > 0 && board[newJ - 1][i] === 0) {
                newJ--;
              }

              if (newJ !== j) {
                if (board[newJ][i] === board[j][i]) {
                 board[newJ][i] *= 2;
                 board[j][i] = 0;
                 score.textContent = `Score: ${parseInt(score.textContent.split(' ')[1]) + board[newJ][i]}`;
                } else {
                 board[newJ][i] = board[j][i];
                 board[j][i] = 0;
                }

                hasChanged = true;
              }
            }
          }
        }
      }

      // Add more conditions for other directions ('right', 'down', 'left')

      if (hasChanged) {
        addTile();
      }
    }

    function startGame() {
      addTile();
      addTile();
      drawBoard();
    }

    startGame();
 </script>
</body>
</html>

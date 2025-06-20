# tic-toc
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Tic-Tac-Toe Game</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@600&display=swap');
  * {
    box-sizing: border-box;
  }
  body {
    background: linear-gradient(135deg, #667eea, #764ba2);
    font-family: 'Montserrat', sans-serif;
    margin: 0;
    padding: 2rem;
    display: flex;
    flex-direction: column;
    align-items: center;
    color: #f0f0f0;
    min-height: 100vh;
  }
  h1 {
    font-size: 3rem;
    margin-bottom: 0.2rem;
    text-shadow: 0 2px 5px rgba(0,0,0,0.3);
  }
  #status {
    font-size: 1.25rem;
    margin-bottom: 1.5rem;
    min-height: 1.5rem;
    font-weight: 600;
    text-align: center;
  }
  .board {
    display: grid;
    grid-template-columns: repeat(3, 100px);
    grid-template-rows: repeat(3, 100px);
    gap: 15px;
    background: rgba(255, 255, 255, 0.1);
    padding: 15px;
    border-radius: 12px;
    box-shadow: 0 8px 25px rgba(0,0,0,0.3);
  }
  .cell {
    background: rgba(255, 255, 255, 0.15);
    border-radius: 10px;
    cursor: pointer;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 3.5rem;
    font-weight: 700;
    color: #f0f0f0;
    user-select: none;
    transition: background-color 0.3s ease, transform 0.2s ease;
    box-shadow: inset 0 0 10px rgba(255,255,255,0.2);
  }
  .cell:hover {
    background-color: rgba(255, 255, 255, 0.3);
    transform: scale(1.05);
  }
  .cell.taken {
    cursor: default;
    background-color: rgba(255, 255, 255, 0.25);
    box-shadow: inset 0 0 15px rgba(255,255,255,0.6);
    transition: none;
  }
  button {
    margin-top: 2rem;
    background-color: #764ba2;
    border: none;
    padding: 0.75rem 2rem;
    font-size: 1.1rem;
    font-weight: 600;
    color: #f0f0f0;
    border-radius: 8px;
    cursor: pointer;
    box-shadow: 0 6px 15px rgba(118, 75, 162, 0.7);
    transition: background-color 0.3s ease;
  }
  button:hover {
    background-color: #5a357d;
  }
</style>
</head>
<body>
<h1>Tic-Tac-Toe</h1>
<div id="status">Player X's turn</div>
<div class="board" id="board">
  <div class="cell" data-index="0" tabindex="0"></div>
  <div class="cell" data-index="1" tabindex="0"></div>
  <div class="cell" data-index="2" tabindex="0"></div>
  <div class="cell" data-index="3" tabindex="0"></div>
  <div class="cell" data-index="4" tabindex="0"></div>
  <div class="cell" data-index="5" tabindex="0"></div>
  <div class="cell" data-index="6" tabindex="0"></div>
  <div class="cell" data-index="7" tabindex="0"></div>
  <div class="cell" data-index="8" tabindex="0"></div>
</div>
<button id="resetBtn">Restart Game</button>

<script>
  (function() {
    const boardEl = document.getElementById('board');
    const cells = boardEl.querySelectorAll('.cell');
    const statusEl = document.getElementById('status');
    const resetBtn = document.getElementById('resetBtn');

    let boardState = Array(9).fill(null);
    let currentPlayer = 'X';
    let gameActive = true;

    const winningCombinations = [
      [0,1,2],
      [3,4,5],
      [6,7,8],
      [0,3,6],
      [1,4,7],
      [2,5,8],
      [0,4,8],
      [2,4,6]
    ];

    function handleCellClick(e) {
      const index = +e.target.getAttribute('data-index');
      if (!gameActive || boardState[index] !== null) {
        return;
      }
      markCell(e.target, index);
      if (checkWin(currentPlayer)) {
        statusEl.textContent = `Player ${currentPlayer} wins! 🎉`;
        gameActive = false;
        highlightWinningCells(currentPlayer);
        return;
      }
      if (boardState.every(cell => cell !== null)) {
        statusEl.textContent = "It's a draw! 🤝";
        gameActive = false;
        return;
      }
      togglePlayer();
      updateStatus();
    }

    function markCell(cell, index) {
      boardState[index] = currentPlayer;
      cell.textContent = currentPlayer;
      cell.classList.add('taken');
    }

    function togglePlayer() {
      currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
    }

    function updateStatus() {
      statusEl.textContent = `Player ${currentPlayer}'s turn`;
    }

    function checkWin(player) {
      return winningCombinations.some(combination => {
        return combination.every(index => boardState[index] === player);
      });
    }

    function highlightWinningCells(player) {
      winningCombinations.forEach(combination => {
        if (combination.every(index => boardState[index] === player)) {
          combination.forEach(index => {
            cells[index].style.backgroundColor = '#b57edc';
            cells[index].style.color = '#4a235a';
            cells[index].style.fontWeight = '900';
            cells[index].style.textShadow = '0 0 5px #7d55aa';
          });
        }
      });
    }

    function resetGame() {
      boardState.fill(null);
      cells.forEach(cell => {
        cell.textContent = '';
        cell.classList.remove('taken');
        cell.style.backgroundColor = '';
        cell.style.color = '';
        cell.style.fontWeight = '';
        cell.style.textShadow = '';
      });
      currentPlayer = 'X';
      gameActive = true;
      updateStatus();
    }

    cells.forEach(cell => cell.addEventListener('click', handleCellClick));
    resetBtn.addEventListener('click', resetGame);
  })();
</script>
</body>
</html>

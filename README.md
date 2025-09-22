<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Games Dashboard</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      font-family: 'Segoe UI', Arial, sans-serif;
      margin: 0;
      padding: 0;
      background: linear-gradient(120deg, #ffecd2 0%, #fcb69f 100%);
      min-height: 100vh;
    }
    header {
      background: #ff6f61;
      color: white;
      padding: 1.5em 0;
      text-align: center;
      font-size: 2.2em;
      letter-spacing: 2px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.07);
    }
    main {
      max-width: 540px;
      margin: 2em auto;
      background: white;
      border-radius: 16px;
      box-shadow: 0 8px 32px rgba(33, 33, 33, 0.11);
      padding: 2em;
    }
    .game-select {
      display: flex;
      justify-content: space-between;
      flex-wrap: wrap;
      gap: 1em;
      margin: 2em 0;
    }
    .game-btn {
      border: none;
      background: #ffe0b2;
      color: #ff6f61;
      font-size: 1.1em;
      padding: 1em 2em;
      border-radius: 8px;
      cursor: pointer;
      box-shadow: 0 2px 8px rgba(0,0,0,0.05);
      transition: background 0.2s, transform 0.15s;
      min-width: 140px;
      margin-bottom: 0.3em;
    }
    .game-btn.active, .game-btn:hover {
      background: #ff6f61;
      color: white;
      transform: scale(1.04);
    }
    .game-area {
      margin-top: 2em;
      min-height: 220px;
    }
    .hidden {
      display: none;
    }
    /* Tic-Tac-Toe styles */
    .ttt-board {
      display: grid;
      grid-template-columns: repeat(3, 60px);
      grid-gap: 10px;
      justify-content: center;
      margin-top: 1em;
    }
    .ttt-cell {
      width: 60px;
      height: 60px;
      background: #ffe0b2;
      border-radius: 6px;
      font-size: 2em;
      color: #ff6f61;
      text-align: center;
      line-height: 60px;
      cursor: pointer;
      box-shadow: 0 2px 8px rgba(0,0,0,0.07);
      user-select: none;
      transition: background 0.2s;
    }
    .ttt-cell:hover {
      background: #fff3e0;
    }
    /* Number Guess styles */
    .ng-input {
      width: 70px;
      font-size: 1.1em;
      padding: 0.4em;
      border-radius: 6px;
      border: 1px solid #ffb199;
      margin-right: 0.5em;
    }
    .ng-btn {
      background: #ff6f61;
      color: white;
      border: none;
      border-radius: 6px;
      padding: 0.5em 1.2em;
      font-size: 1em;
      cursor: pointer;
      margin-top: 0.3em;
      transition: background 0.2s;
    }
    .ng-btn:hover {
      background: #d84315;
    }
    /* Rock Paper Scissors styles */
    .rps-btn {
      background: #ffb199;
      color: #ff6f61;
      border: none;
      border-radius: 6px;
      font-size: 1.1em;
      padding: 1em 1.4em;
      cursor: pointer;
      margin-right: 1em;
      margin-bottom: 0.5em;
      transition: background 0.2s;
    }
    .rps-btn:hover {
      background: #ff6f61;
      color: white;
    }
    .rps-result {
      margin-top: 1em;
      font-size: 1.1em;
      color: #ff6f61;
    }
    @media (max-width: 600px) {
      main {
        padding: 1em;
      }
      .game-btn {
        font-size: 1em;
        padding: 0.8em 1em;
        min-width: 80px;
      }
      .ttt-board {
        grid-template-columns: repeat(3, 45px);
        grid-gap: 6px;
      }
      .ttt-cell {
        width: 45px;
        height: 45px;
        font-size: 1.3em;
        line-height: 45px;
      }
    }
  </style>
</head>
<body>
  <header>
    🎮 Games Dashboard
  </header>
  <main>
    <label style="font-size:1.1em;">Choose a game:</label>
    <div class="game-select">
      <button class="game-btn" data-game="ttt">Tic-Tac-Toe</button>
      <button class="game-btn" data-game="guess">Number Guess</button>
      <button class="game-btn" data-game="rps">Rock Paper Scissors</button>
    </div>
    <div id="game-area" class="game-area">
      <!-- Game content will appear here -->
    </div>
  </main>
  <script>
    // Game selectors
    const gameBtns = document.querySelectorAll('.game-btn');
    const gameArea = document.getElementById('game-area');

    // Remove active class from all game buttons
    function clearActive() {
      gameBtns.forEach(btn => btn.classList.remove('active'));
    }

    // Clear game area
    function clearGameArea() {
      gameArea.innerHTML = '';
    }

    // Tic-Tac-Toe implementation
    function startTicTacToe() {
      clearGameArea();
      let board = Array(9).fill('');
      let currentPlayer = 'X';
      let gameOver = false;

      function checkWinner(b) {
        const wins = [
          [0,1,2],[3,4,5],[6,7,8], // rows
          [0,3,6],[1,4,7],[2,5,8], // cols
          [0,4,8],[2,4,6]          // diags
        ];
        for (let w of wins) {
          const [a,b,c] = w;
          if (board[a] && board[a] === board[b] && board[b] === board[c]) return board[a];
        }
        return board.every(cell => cell) ? 'draw' : null;
      }

      const tttTitle = document.createElement('div');
      tttTitle.style.fontSize = "1.1em";
      tttTitle.style.color = "#ff6f61";
      tttTitle.innerText = "Tic-Tac-Toe: Player X's turn";
      gameArea.appendChild(tttTitle);

      const tttBoard = document.createElement('div');
      tttBoard.className = "ttt-board";
      gameArea.appendChild(tttBoard);

      function renderBoard() {
        tttBoard.innerHTML = '';
        board.forEach((cell, idx) => {
          const cellDiv = document.createElement('div');
          cellDiv.className = "ttt-cell";
          cellDiv.innerText = cell;
          cellDiv.onclick = () => {
            if (!cell && !gameOver) {
              board[idx] = currentPlayer;
              const winner = checkWinner(board);
              if (winner) {
                gameOver = true;
                if (winner === 'draw') {
                  tttTitle.innerText = "It's a draw!";
                } else {
                  tttTitle.innerText = `Player ${winner} wins!`;
                }
              } else {
                currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
                tttTitle.innerText = `Tic-Tac-Toe: Player ${currentPlayer}'s turn`;
              }
              renderBoard();
            }
          };
          tttBoard.appendChild(cellDiv);
        });
      }
      renderBoard();

      // Reset button
      const resetBtn = document.createElement('button');
      resetBtn.className = "game-btn";
      resetBtn.style.marginTop = "1.5em";
      resetBtn.innerText = "Restart Game";
      resetBtn.onclick = startTicTacToe;
      gameArea.appendChild(resetBtn);
    }

    // Number Guess implementation
    function startNumberGuess() {
      clearGameArea();
      const maxNum = 50;
      const secret = Math.floor(Math.random()*maxNum)+1;
      let attempts = 0;
      let finished = false;

      const ngTitle = document.createElement('div');
      ngTitle.style.fontSize = "1.1em";
      ngTitle.style.color = "#ff6f61";
      ngTitle.innerText = `Guess the number (1-${maxNum})`;
      gameArea.appendChild(ngTitle);

      const ngForm = document.createElement('form');
      ngForm.onsubmit = e => { e.preventDefault(); };

      const ngInput = document.createElement('input');
      ngInput.type = "number";
      ngInput.min = 1;
      ngInput.max = maxNum;
      ngInput.className = "ng-input";
      ngInput.placeholder = "?";
      ngForm.appendChild(ngInput);

      const ngBtn = document.createElement('button');
      ngBtn.className = "ng-btn";
      ngBtn.innerText = "Guess!";
      ngForm.appendChild(ngBtn);

      gameArea.appendChild(ngForm);

      const ngStatus = document.createElement('div');
      ngStatus.style.marginTop = "0.8em";
      ngStatus.style.fontSize = "1.1em";
      ngStatus.style.color = "#ff6f61";
      gameArea.appendChild(ngStatus);

      ngBtn.onclick = () => {
        if (finished) return;
        const val = parseInt(ngInput.value);
        if (!val || val < 1 || val > maxNum) {
          ngStatus.innerText = "Please enter a valid number!";
          return;
        }
        attempts++;
        if (val < secret) {
          ngStatus.innerText = "Too low!";
        } else if (val > secret) {
          ngStatus.innerText = "Too high!";
        } else {
          ngStatus.innerText = `Correct! You guessed it in ${attempts} attempts.`;
          finished = true;
        }
        ngInput.value = "";
        ngInput.focus();
      };

      // Reset button
      const resetBtn = document.createElement('button');
      resetBtn.className = "game-btn";
      resetBtn.style.marginTop = "1.5em";
      resetBtn.innerText = "Play Again";
      resetBtn.onclick = startNumberGuess;
      gameArea.appendChild(resetBtn);
    }

    // Rock Paper Scissors implementation
    function startRPS() {
      clearGameArea();
      const choices = ["Rock", "Paper", "Scissors"];
      const rpsTitle = document.createElement('div');
      rpsTitle.style.fontSize = "1.1em";
      rpsTitle.style.color = "#ff6f61";
      rpsTitle.innerText = "Rock Paper Scissors";
      gameArea.appendChild(rpsTitle);

      const rpsBtnsDiv = document.createElement('div');
      choices.forEach(c => {
        const btn = document.createElement('button');
        btn.className = "rps-btn";
        btn.innerText = c;
        btn.onclick = () => playRPS(c);
        rpsBtnsDiv.appendChild(btn);
      });
      gameArea.appendChild(rpsBtnsDiv);

      const resultDiv = document.createElement('div');
      resultDiv.className = "rps-result";
      gameArea.appendChild(resultDiv);

      function playRPS(userChoice) {
        const compChoice = choices[Math.floor(Math.random()*3)];
        let resMsg = `You chose ${userChoice}. Computer chose ${compChoice}. `;
        if (userChoice === compChoice) {
          resMsg += "It's a draw!";
        } else if (
          (userChoice === "Rock" && compChoice === "Scissors") ||
          (userChoice === "Paper" && compChoice === "Rock") ||
          (userChoice === "Scissors" && compChoice === "Paper")
        ) {
          resMsg += "You win!";
        } else {
          resMsg += "Computer wins!";
        }
        resultDiv.innerText = resMsg;
      }

      // Reset button
      const resetBtn = document.createElement('button');
      resetBtn.className = "game-btn";
      resetBtn.style.marginTop = "1.5em";
      resetBtn.innerText = "Play Again";
      resetBtn.onclick = startRPS;
      gameArea.appendChild(resetBtn);
    }

    // Game selection logic
    gameBtns.forEach(btn => {
      btn.addEventListener('click', () => {
        clearActive();
        btn.classList.add('active');
        switch(btn.dataset.game) {
          case 'ttt': startTicTacToe(); break;
          case 'guess': startNumberGuess(); break;
          case 'rps': startRPS(); break;
        }
      });
    });

    // Default: show Tic-Tac-Toe
    startTicTacToe();
    gameBtns[0].classList.add('active');
  </script>
</body>
</html>

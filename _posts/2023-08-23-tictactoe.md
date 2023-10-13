---
toc: true
comments: true
layout: post
title: Tic-Tac-Toe
description: A simple Tic Tac Toe game to test blog functionality
type: tangibles
courses: {csa: {week: 1}}
---
<html>
<head>
  <style>
  .board-container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 50vh;
  }
  .board {
    display: grid;
    grid-template-columns: repeat(3, 100px);
    grid-gap: 2px;
  }
  .cell {
    width: 100px;
    height: 100px;
    border: 1px solid black;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 24px;
  }
</style>
</head>
<body>
<div class="board-container">
  <div class="board" id="board">
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
  </div>
</div>
  <script>
    const board = document.getElementById('board');
    const cells = document.querySelectorAll('.cell');
    let currentPlayer = 'X';
    let gameOver = false;

    function checkWin() {
      const winningCombos = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8], // Rows
        [0, 3, 6], [1, 4, 7], [2, 5, 8], // Columns
        [0, 4, 8], [2, 4, 6]            // Diagonals
      ];

      for (const combo of winningCombos) {
        const [a, b, c] = combo;
        if (cells[a].textContent && cells[a].textContent === cells[b].textContent && cells[a].textContent === cells[c].textContent) {
          gameOver = true;
          cells[a].style.backgroundColor = 'lightgreen';
          cells[b].style.backgroundColor = 'lightgreen';
          cells[c].style.backgroundColor = 'lightgreen';
          break;
        }
      }

      if (!gameOver && Array.from(cells).every(cell => cell.textContent)) {
        gameOver = true;
        cells.forEach(cell => {
          cell.style.backgroundColor = 'lightcoral';
        });
      }
    }

    function handleCellClick(event) {
      const cell = event.target;
      if (!cell.textContent && !gameOver) {
        cell.textContent = currentPlayer;
        checkWin();
        currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
      }
    }

    cells.forEach(cell => {
      cell.addEventListener('click', handleCellClick);
    });
  </script>
</body>
</html>

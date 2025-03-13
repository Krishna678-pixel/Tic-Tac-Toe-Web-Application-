# Tic-Tac-Toe Web Application.
<!DOCTYPE html> 
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
  <h1>tic-tac-toe</h1>
    <div id="tic-tac-toe">
        <div class="cell" data-cell></div>
        </div>
    <button id="restartButton">Restart</button>
</body>
</html>

#tic-tac-toe {
    display: grid;
    grid-template-columns: repeat(3, 100px);
    grid-template-rows: repeat(3, 100px);
    gap: 5px;
    margin: 20px auto;
    width: 310px;
}

.cell {
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #fff;
    border: 2px solid #000;
    font-size: 2em;
    cursor: pointer;
}


const cells = document.querySelectorAll('[data-cell]');
const board = document.getElementById('tic-tac-toe');
const restartButton = document.getElementById('restartButton');
let isCircleTurn;

const startGame = () => {
    isCircleTurn = false;
    cells.forEach(cell => {
        cell.classList.remove('circle');
        cell.classList.remove('x');
        cell.removeEventListener('click', handleClick);
        cell.addEventListener('click', handleClick, { once: true });
    });
};

const handleClick = (e) => {
    const cell = e.target;
    const currentClass = isCircleTurn ? 'circle' : 'x';
    placeMark(cell, currentClass);
    if (checkWin(currentClass)) {
        alert(`${currentClass.toUpperCase()} Wins!`);
    } else if (isDraw()) {
        alert('Draw!');
    } else {
        swapTurns();
    }
};

const placeMark = (cell, currentClass) => {
    cell.classList.add(currentClass);
};

const swapTurns = () => {
    isCircleTurn = !isCircleTurn;
};

const checkWin = (currentClass) => {
    const WINNING_COMBINATIONS = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6]
    ];
    return WINNING_COMBINATIONS.some(combination => {
        return combination.every(index => {
            return cells[index].classList.contains(currentClass);
        });
    });
};

const isDraw = () => {
    return [...cells].every(cell => {
        return cell.classList.contains('x') || cell.classList.contains('circle');
    });
};

restartButton.addEventListener('click', startGame);

startGame();

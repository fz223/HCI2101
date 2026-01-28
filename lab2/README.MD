
‏<html lang="ar" dir="rtl">
‏<head>
‏<meta charset="UTF-8">
‏<title>لعبة XO</title>
‏<meta name="viewport" content="width=device-width, initial-scale=1.0">

‏<style>
‏    body {
‏        margin: 0;
‏        font-family: "Cairo", Tahoma, Arial, sans-serif;
‏        background: linear-gradient(135deg, #0f2027, #203a43, #2c5364);
‏        color: #fff;
‏        min-height: 100vh;
‏        display: flex;
‏        flex-direction: column;
    }

‏    header {
‏        text-align: center;
‏        padding: 25px 10px;
    }

‏    header h1 {
‏        margin: 0;
‏        font-size: 2.5rem;
    }

‏    header p {
‏        margin-top: 8px;
‏        opacity: 0.85;
    }

‏    main {
‏        flex: 1;
‏        display: flex;
‏        flex-direction: column;
‏        align-items: center;
‏        justify-content: center;
    }

‏    .modes {
‏        margin-bottom: 20px;
    }

‏    .modes button {
‏        background: #ffffff;
‏        color: #203a43;
‏        border: none;
‏        padding: 10px 18px;
‏        margin: 5px;
‏        border-radius: 10px;
‏        font-size: 1rem;
‏        cursor: pointer;
‏        transition: 0.3s;
    }

‏    .modes button:hover {
‏        transform: scale(1.05);
‏        background: #f2f2f2;
    }

‏    .status {
‏        margin-bottom: 15px;
‏        font-size: 1.1rem;
    }

‏    .board {
‏        display: grid;
‏        grid-template-columns: repeat(3, 100px);
‏        grid-template-rows: repeat(3, 100px);
‏        gap: 10px;
    }

‏    .cell {
‏        background: rgba(255,255,255,0.15);
‏        border-radius: 15px;
‏        font-size: 2.8rem;
‏        display: flex;
‏        align-items: center;
‏        justify-content: center;
‏        cursor: pointer;
‏        transition: 0.25s;
    }

‏    .cell:hover {
‏        background: rgba(255,255,255,0.3);
    }

‏    .cell.disabled {
‏        pointer-events: none;
‏        opacity: 0.6;
    }

‏    .reset {
‏        margin-top: 20px;
‏        background: #ffcc00;
‏        color: #333;
‏        border: none;
‏        padding: 10px 22px;
‏        border-radius: 12px;
‏        font-size: 1rem;
‏        cursor: pointer;
    }

‏    footer {
‏        background: rgba(0,0,0,0.35);
‏        text-align: center;
‏        padding: 12px;
‏        font-size: 0.9rem;
    }

‏    @media (max-width: 480px) {
‏        .board {
‏            grid-template-columns: repeat(3, 80px);
‏            grid-template-rows: repeat(3, 80px);
        }
    }
‏</style>
‏</head>

‏<body>

‏<header>
‏    <h1>🎮 لعبة XO</h1>
‏    <p>لعبة إكس أو بتصميم عصري</p>
‏</header>

‏<main>

‏    <div class="modes">
‏        <button onclick="startGame('single')">لاعب واحد 🤖</button>
‏        <button onclick="startGame('two')">لاعبين 👥</button>
‏    </div>

‏    <div class="status" id="status">اختر وضع اللعب للبدء</div>

‏    <div class="board" id="board"></div>

‏    <button class="reset" onclick="resetGame()">إعادة اللعب 🔄</button>

‏</main>

‏<footer>
   
‏    Student Name: [Fahad] | ID: [446000926] | Supervised by: Mohammed Jabali
‏</footer>

‏<script>
‏    const boardEl = document.getElementById("board");
‏    const statusEl = document.getElementById("status");

‏    let board = Array(9).fill("");
‏    let currentPlayer = "X";
‏    let mode = null;
‏    let active = false;

‏    const winPatterns = [
        [0,1,2],[3,4,5],[6,7,8],
        [0,3,6],[1,4,7],[2,5,8],
        [0,4,8],[2,4,6]
    ];

‏    function startGame(selectedMode) {
‏        mode = selectedMode;
‏        resetGame();
‏        active = true;
‏        statusEl.textContent = "دور اللاعب: " + currentPlayer;
    }

‏    function createBoard() {
‏        boardEl.innerHTML = "";
‏        board.forEach((_, i) => {
‏            const cell = document.createElement("div");
‏            cell.className = "cell";
‏            cell.onclick = () => play(i);
‏            boardEl.appendChild(cell);
        });
    }

‏    function play(index) {
‏        if (!active || board[index]) return;

‏        board[index] = currentPlayer;
‏        updateBoard();

‏        if (checkWin()) {
‏            statusEl.
textContent = "🎉 اللاعب " + currentPlayer + " فاز!";
‏            active = false;
‏            disableBoard();
‏            return;
        }

‏        if (!board.includes("")) {
‏            statusEl.textContent = "😐 تعادل!";
‏            active = false;
‏            return;
        }

‏        currentPlayer = currentPlayer === "X" ? "O" : "X";
‏        statusEl.textContent = "دور اللاعب: " + currentPlayer;

‏        if (mode === "single" && currentPlayer === "O") {
‏            setTimeout(computerMove, 500);
        }
    }

‏    function computerMove() {
‏        const empty = board
‏            .map((v,i) => v === "" ? i : null)
‏            .filter(v => v !== null);

‏        const choice = empty[Math.floor(Math.random() * empty.length)];
‏        play(choice);
    }

‏    function checkWin() {
‏        return winPatterns.some(p =>
‏            p.every(i => board[i] === currentPlayer)
        );
    }

‏    function updateBoard() {
‏        document.querySelectorAll(".cell").forEach((c,i) => {
‏            c.textContent = board[i];
        });
    }

‏    function disableBoard() {
‏        document.querySelectorAll(".cell").forEach(c =>
‏            c.classList.add("disabled")
        );
    }

‏    function resetGame() {
‏        board = Array(9).fill("");
‏        currentPlayer = "X";
‏        active = false;
‏        statusEl.textContent = "اختر وضع اللعب للبدء";
‏        createBoard();
    }

‏    createBoard();
‏</script>

‏</body>
‏</html>

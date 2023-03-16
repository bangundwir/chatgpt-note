Model: GPT-4

> Buatakan game snake di html,css,javascript Train AI to play Snake menjadi beberapa file

Membuat game Snake di HTML, CSS, dan JavaScript melibatkan beberapa langkah, dan di sini, kita akan membaginya menjadi beberapa file agar lebih mudah diatur. Anda akan memerlukan tiga file: index.html, style.css, dan script.js.

1.  Buat file index.html:

html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake Game</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div id="game-board"></div>
    <script src="script.js"></script>
</body>
</html>
```

2.  Buat file style.css:

css

```css
body {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    background-color: #222;
    margin: 0;
    font-family: Arial, sans-serif;
}

#game-board {
    display: grid;
    grid-template-rows: repeat(20, 20px);
    grid-template-columns: repeat(20, 20px);
    gap: 1px;
    background-color: #333;
}

.snake {
    background-color: lime;
    border-radius: 2px;
}

.food {
    background-color: red;
    border-radius: 50%;
}
```

3.  Buat file script.js:

javascript

```javascript
const board = document.getElementById('game-board');
const boardSize = 20;
let snake = [{x: 10, y: 10}];
let food = {x: 5, y: 5};
let dx = 0, dy = 0;

function createBoard() {
    for (let i = 0; i < boardSize * boardSize; i++) {
        const cell = document.createElement('div');
        board.appendChild(cell);
    }
}

function drawSnakeAndFood() {
    const cells = board.children;
    for (let i = 0; i < cells.length; i++) {
        const cell = cells[i];
        const x = i % boardSize;
        const y = Math.floor(i / boardSize);

        if (snake.some(s => s.x === x && s.y === y)) {
            cell.classList.add('snake');
        } else if (food.x === x && food.y === y) {
            cell.classList.add('food');
        } else {
            cell.className = '';
        }
    }
}

function moveSnake() {
    const newHead = {x: snake[0].x + dx, y: snake[0].y + dy};

    if (newHead.x < 0 || newHead.x >= boardSize || newHead.y < 0 || newHead.y >= boardSize || snake.some(s => s.x === newHead.x && s.y === newHead.y)) {
        // Game over, reset snake
        snake = [{x: 10, y: 10}];
        dx = 0;
        dy = 0;
    } else {
        snake.unshift(newHead);

        if (newHead.x === food.x && newHead.y === food.y) {
            // Snake ate the food, generate new food
            food.x = Math.floor(Math.random() * boardSize);
            food.y = Math.floor(Math.random() * boardSize);
        } else {
            // Remove tail
            snake.pop();
        }
    }
}

function handleKeyPress(event) {
    if (event.key === 'ArrowUp' && dy === 0) {
        dx = 0
```

> Lanjutkan kode tersebut

Saya akan melanjutkan kode script.js yang terpotong sebelumnya.

javascript

        ```javascript
dy = -1;
    } else if (event.key === 'ArrowDown' && dy === 0) {
        dx = 0;
        dy = 1;
    } else if (event.key === 'ArrowLeft' && dx === 0) {
        dx = -1;
        dy = 0;
    } else if (event.key === 'ArrowRight' && dx === 0) {
        dx = 1;
        dy = 0;
    }
}

function gameLoop() {
    moveSnake();
    drawSnakeAndFood();
    setTimeout(gameLoop, 100);
}

createBoard();
drawSnakeAndFood();
gameLoop();
document.addEventListener('keydown', handleKeyPress);
```

Sekarang Anda memiliki kode JavaScript yang lengkap untuk game Snake. Kode ini akan menangani gerakan ular, memeriksa tabrakan dengan dinding atau dirinya sendiri, menghasilkan makanan baru saat ular memakannya, dan mengontrol kecepatan game.

Pastikan untuk menyimpan file ini sebagai script.js dan meletakkannya di folder yang sama dengan index.html dan style.css yang telah Anda buat sebelumnya. Buka index.html di browser Anda, dan Anda harus dapat melihat dan bermain game Snake.

> Tambahkan fiture agar bisa di mainkan ai di file yang baru

Untuk menambahkan fitur agar AI bisa memainkan game Snake, kita akan membuat file baru bernama ai.js. Kita akan menggunakan algoritme sederhana yang memungkinkan AI mengikuti makanan. Ingatlah bahwa AI ini tidak sempurna dan ada banyak cara untuk meningkatkannya.

1.  Buat file ai.js:

javascript

```javascript
function getNextDirection(snake, food) {
    const head = snake[0];

    if (head.x < food.x && !snake.some(s => s.x === head.x + 1 && s.y === head.y)) {
        // Food is to the right
        return { dx: 1, dy: 0 };
    } else if (head.x > food.x && !snake.some(s => s.x === head.x - 1 && s.y === head.y)) {
        // Food is to the left
        return { dx: -1, dy: 0 };
    } else if (head.y < food.y && !snake.some(s => s.x === head.x && s.y === head.y + 1)) {
        // Food is below
        return { dx: 0, dy: 1 };
    } else if (head.y > food.y && !snake.some(s => s.x === head.x && s.y === head.y - 1)) {
        // Food is above
        return { dx: 0, dy: -1 };
    }

    // Fallback if food is blocked by the snake's body
    const possibleMoves = [
        { dx: 1, dy: 0 },
        { dx: -1, dy: 0 },
        { dx: 0, dy: 1 },
        { dx: 0, dy: -1 },
    ];

    return possibleMoves.find(move => !snake.some(s => s.x === head.x + move.dx && s.y === head.y + move.dy));
}
```

2.  Modifikasi script.js untuk menggabungkan ai.js:

Tambahkan baris berikut tepat di bawah baris `<script src="script.js"></script>` dalam file index.html:

html

```html
<script src="ai.js"></script>
```

Kemudian, ubah fungsi gameLoop() dalam script.js untuk memasukkan kode AI:

javascript

```javascript
function gameLoop() {
    const aiDirection = getNextDirection(snake, food);
    dx = aiDirection.dx;
    dy = aiDirection.dy;

    moveSnake();
    drawSnakeAndFood();
    setTimeout(gameLoop, 100);
}
```

Sekarang AI akan mengontrol ular dalam game Snake. Buka kembali index.html di browser Anda dan lihat AI memainkan game tersebut. Ingatlah bahwa AI ini sederhana dan tidak akan selalu berhasil memecahkan game. Ada banyak cara untuk meningkatkan AI, seperti dengan mengimplementasikan algoritme yang lebih canggih atau menggunakan machine learning.

> Tulis semua script.js

Berikut ini adalah file script.js yang telah dimodifikasi untuk menggabungkan ai.js:

javascript

```javascript
const board = document.getElementById('game-board');
const boardSize = 20;
let snake = [{x: 10, y: 10}];
let food = {x: 5, y: 5};
let dx = 0, dy = 0;

function createBoard() {
    for (let i = 0; i < boardSize * boardSize; i++) {
        const cell = document.createElement('div');
        board.appendChild(cell);
    }
}

function drawSnakeAndFood() {
    const cells = board.children;
    for (let i = 0; i < cells.length; i++) {
        const cell = cells[i];
        const x = i % boardSize;
        const y = Math.floor(i / boardSize);

        if (snake.some(s => s.x === x && s.y === y)) {
            cell.classList.add('snake');
        } else if (food.x === x && food.y === y) {
            cell.classList.add('food');
        } else {
            cell.className = '';
        }
    }
}

function moveSnake() {
    const newHead = {x: snake[0].x + dx, y: snake[0].y + dy};

    if (newHead.x < 0 || newHead.x >= boardSize || newHead.y < 0 || newHead.y >= boardSize || snake.some(s => s.x === newHead.x && s.y === newHead.y)) {
        // Game over, reset snake
        snake = [{x: 10, y: 10}];
        dx = 0;
        dy = 0;
    } else {
        snake.unshift(newHead);

        if (newHead.x === food.x && newHead.y === food.y) {
            // Snake ate the food, generate new food
            food.x = Math.floor(Math.random() * boardSize);
            food.y = Math.floor(Math.random() * boardSize);
        } else {
            // Remove tail
            snake.pop();
        }
    }
}

function gameLoop() {
    const aiDirection = getNextDirection(snake, food);
    dx = aiDirection.dx;
    dy = aiDirection.dy;

    moveSnake();
    drawSnakeAndFood();
    setTimeout(gameLoop, 100);
}

createBoard();
drawSnakeAndFood();
gameLoop();
```

Jangan lupa untuk memasukkan `<script src="ai.js"></script>` di file index.html setelah `<script src="script.js"></script>` agar fungsi getNextDirection() dalam ai.js dapat digunakan dalam script.js.

> Tulis semua index.html nya

Berikut ini adalah file index.html yang telah dimodifikasi untuk menggabungkan ai.js:

html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake Game</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div id="game-board"></div>
    <script src="script.js"></script>
    <script src="ai.js"></script>
</body>
</html>
```

Dalam file ini, kami menambahkan baris `<script src="ai.js"></script>` setelah `<script src="script.js"></script>` untuk memasukkan file ai.js yang telah Anda buat sebelumnya. Sekarang AI akan mengontrol ular dalam game Snake. Buka kembali index.html di browser Anda dan lihat AI memainkan game tersebut.

> Tambakan fiture start dan restart ada score nya juga

Untuk menambahkan fitur start, restart, dan skor, kita perlu memodifikasi file index.html, style.css, dan script.js.

1.  Modifikasi file index.html:

Tambahkan tombol start, restart, dan elemen skor ke dalam file index.html di bawah elemen `<div id="game-board"></div>`.

html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake Game</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div id="game-board"></div>
    <button id="start-btn">Start</button>
    <button id="restart-btn" disabled>Restart</button>
    <div id="score">Score: 0</div>
    <script src="script.js"></script>
    <script src="ai.js"></script>
</body>
</html>
```

2.  Modifikasi file style.css:

Tambahkan gaya untuk tombol start, restart, dan elemen skor.

css

```css
/* ... Existing styles ... */

button {
    background-color: #4CAF50;
    border: none;
    color: white;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin: 4px 2px;
    cursor: pointer;
    padding: 10px 24px;
    border-radius: 4px;
}

button:disabled {
    background-color: #ccc;
    cursor: not-allowed;
}

#score {
    color: white;
    font-size: 20px;
    margin-top: 10px;
}
```

3.  Modifikasi file script.js:

Tambahkan fitur start, restart, dan skor ke dalam file script.js.

javascript

```javascript
// ... Existing code ...

let score = 0;
let gameInterval;
const startBtn = document.getElementById('start-btn');
const restartBtn = document.getElementById('restart-btn');
const scoreElement = document.getElementById('score');

function updateScore() {
    scoreElement.innerText = `Score: ${score}`;
}

function startGame() {
    startBtn.disabled = true;
    restartBtn.disabled = false;
    gameInterval = setInterval(gameLoop, 100);
}

function restartGame() {
    clearInterval(gameInterval);
    snake = [{x: 10, y: 10}];
    food = {x: 5, y: 5};
    dx = 0;
    dy = 0;
    score = 0;
    updateScore();
    drawSnakeAndFood();
    startBtn.disabled = false;
    restartBtn.disabled = true;
}

startBtn.addEventListener('click', startGame);
restartBtn.addEventListener('click', restartGame);

// ... Existing code ...

function moveSnake() {
    // ... Existing code ...

    if (newHead.x === food.x && newHead.y === food.y) {
        // Snake ate the food, generate new food
        food.x = Math.floor(Math.random() * boardSize);
        food.y = Math.floor(Math.random() * boardSize);
        score++;
        updateScore();
    } else {
        // Remove tail
        snake.pop();
    }
}

// Replace gameLoop function with the following code
function gameLoop() {
    const aiDirection = getNextDirection(snake, food);
    dx = aiDirection.dx;
    dy = aiDirection.dy;

    moveSnake();
    drawSnakeAndFood();
}

createBoard();
drawSnakeAndFood();
updateScore();
```

Sekarang Anda memiliki fitur

> Tulis semua script.js.

Berikut adalah file script.js yang telah dimodifikasi untuk menambahkan fitur start, restart, dan skor:

javascript

```javascript
const board = document.getElementById('game-board');
const boardSize = 20;
let snake = [{x: 10, y: 10}];
let food = {x: 5, y: 5};
let dx = 0, dy = 0;
let score = 0;
let gameInterval;
const startBtn = document.getElementById('start-btn');
const restartBtn = document.getElementById('restart-btn');
const scoreElement = document.getElementById('score');

function createBoard() {
    for (let i = 0; i < boardSize * boardSize; i++) {
        const cell = document.createElement('div');
        board.appendChild(cell);
    }
}

function drawSnakeAndFood() {
    const cells = board.children;
    for (let i = 0; i < cells.length; i++) {
        const cell = cells[i];
        const x = i % boardSize;
        const y = Math.floor(i / boardSize);

        if (snake.some(s => s.x === x && s.y === y)) {
            cell.classList.add('snake');
        } else if (food.x === x && food.y === y) {
            cell.classList.add('food');
        } else {
            cell.className = '';
        }
    }
}

function updateScore() {
    scoreElement.innerText = `Score: ${score}`;
}

function startGame() {
    startBtn.disabled = true;
    restartBtn.disabled = false;
    gameInterval = setInterval(gameLoop, 100);
}

function restartGame() {
    clearInterval(gameInterval);
    snake = [{x: 10, y: 10}];
    food = {x: 5, y: 5};
    dx = 0;
    dy = 0;
    score = 0;
    updateScore();
    drawSnakeAndFood();
    startBtn.disabled = false;
    restartBtn.disabled = true;
}

startBtn.addEventListener('click', startGame);
restartBtn.addEventListener('click', restartGame);

function moveSnake() {
    const newHead = {x: snake[0].x + dx, y: snake[0].y + dy};

    if (newHead.x < 0 || newHead.x >= boardSize || newHead.y < 0 || newHead.y >= boardSize || snake.some(s => s.x === newHead.x && s.y === newHead.y)) {
        // Game over, reset snake
        restartGame();
    } else {
        snake.unshift(newHead);

        if (newHead.x === food.x && newHead.y === food.y) {
            // Snake ate the food, generate new food
            food.x = Math.floor(Math.random() * boardSize);
            food.y = Math.floor(Math.random() * boardSize);
            score++;
            updateScore();
        } else {
            // Remove tail
            snake.pop();
        }
    }
}

function gameLoop() {
    const aiDirection = getNextDirection(snake, food);
    dx = aiDirection.dx;
    dy = aiDirection.dy;

    moveSnake();
    drawSnakeAndFood();
}

createBoard();
drawSnakeAndFood();
updateScore();
```

Dalam file ini, kami menambahkan fitur start, restart, dan skor, serta mengubah logika gameLoop agar game hanya berjalan saat tombol start ditekan dan dihentikan saat tombol restart ditekan. Sekarang Anda memiliki fitur yang diminta. Pastikan semua file (index.html, style.css, script.js, dan ai.js) telah diperbarui dan berada dalam folder yang sama. Buka index.html

> Buatakan history score tertinggi di local storage nya di berikan peringkan di file yang terspisah

Untuk membuat fitur history skor tertinggi yang disimpan di localStorage dan menampilkan peringkat, kita akan membuat file baru bernama highscore.js.

1.  Buat file highscore.js:

javascript

```javascript
const highscoreList = document.getElementById('highscore-list');
const highscoreCount = 5;

function getHighscores() {
    const highscores = localStorage.getItem('highscores');
    return highscores ? JSON.parse(highscores) : [];
}

function setHighscores(highscores) {
    localStorage.setItem('highscores', JSON.stringify(highscores));
}

function updateHighscore(newScore) {
    const highscores = getHighscores();
    highscores.push(newScore);
    highscores.sort((a, b) => b - a);
    highscores.length = Math.min(highscores.length, highscoreCount);
    setHighscores(highscores);
    return highscores;
}

function displayHighscores() {
    const highscores = getHighscores();
    highscoreList.innerHTML = '';

    highscores.forEach((score, index) => {
        const listItem = document.createElement('li');
        listItem.innerText = `${index + 1}. ${score}`;
        highscoreList.appendChild(listItem);
    });
}
```

2.  Modifikasi index.html untuk menggabungkan highscore.js:

Tambahkan elemen daftar untuk menampilkan highscore di bawah elemen skor dan tambahkan baris `<script src="highscore.js"></script>` di bawah baris `<script src="ai.js"></script>` dalam file index.html:

html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake Game</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div id="game-board"></div>
    <button id="start-btn">Start</button>
    <button id="restart-btn" disabled>Restart</button>
    <div id="score">Score: 0</div>
    <ul id="highscore-list"></ul>
    <script src="script.js"></script>
    <script src="ai.js"></script>
    <script src="highscore.js"></script>
</body>
</html>
```

3.  Modifikasi file style.css:

Tambahkan gaya untuk daftar highscore.

css

```css
/* ... Existing styles ... */

#highscore-list {
    color: white;
    font-size: 18px;
    list-style-type: none;
    padding: 0;
    margin-top: 10px;
}

#highscore-list li {
    margin-bottom: 5px;
}
```

4.  Modifikasi file script.js:

Panggil `updateHighscore(score)` setelah skor berubah dan `displayHighscores()` setelah permainan dimulai dan dihentikan.

javascript

```javascript
// ... Existing code ...

function restartGame() {
    // ... Existing code ...

    displayHighscores();
}

// ... Existing code ...

function move
```

> Tulis full script.js:

Berikut adalah file script.js yang telah dimodifikasi untuk menambahkan fitur highscore dengan localStorage:

javascript

```javascript
const board = document.getElementById('game-board');
const boardSize = 20;
let snake = [{x: 10, y: 10}];
let food = {x: 5, y: 5};
let dx = 0, dy = 0;
let score = 0;
let gameInterval;
const startBtn = document.getElementById('start-btn');
const restartBtn = document.getElementById('restart-btn');
const scoreElement = document.getElementById('score');

function createBoard() {
    for (let i = 0; i < boardSize * boardSize; i++) {
        const cell = document.createElement('div');
        board.appendChild(cell);
    }
}

function drawSnakeAndFood() {
    const cells = board.children;
    for (let i = 0; i < cells.length; i++) {
        const cell = cells[i];
        const x = i % boardSize;
        const y = Math.floor(i / boardSize);

        if (snake.some(s => s.x === x && s.y === y)) {
            cell.classList.add('snake');
        } else if (food.x === x && food.y === y) {
            cell.classList.add('food');
        } else {
            cell.className = '';
        }
    }
}

function updateScore() {
    scoreElement.innerText = `Score: ${score}`;
}

function startGame() {
    startBtn.disabled = true;
    restartBtn.disabled = false;
    gameInterval = setInterval(gameLoop, 100);
}

function restartGame() {
    clearInterval(gameInterval);
    snake = [{x: 10, y: 10}];
    food = {x: 5, y: 5};
    dx = 0;
    dy = 0;
    updateHighscore(score);
    score = 0;
    updateScore();
    drawSnakeAndFood();
    startBtn.disabled = false;
    restartBtn.disabled = true;
    displayHighscores();
}

startBtn.addEventListener('click', startGame);
restartBtn.addEventListener('click', restartGame);

function moveSnake() {
    const newHead = {x: snake[0].x + dx, y: snake[0].y + dy};

    if (newHead.x < 0 || newHead.x >= boardSize || newHead.y < 0 || newHead.y >= boardSize || snake.some(s => s.x === newHead.x && s.y === newHead.y)) {
        // Game over, reset snake
        restartGame();
    } else {
        snake.unshift(newHead);

        if (newHead.x === food.x && newHead.y === food.y) {
            // Snake ate the food, generate new food
            food.x = Math.floor(Math.random() * boardSize);
            food.y = Math.floor(Math.random() * boardSize);
            score++;
            updateScore();
        } else {
            // Remove tail
            snake.pop();
        }
    }
}

function gameLoop() {
    const aiDirection = getNextDirection(snake, food);
    dx = aiDirection.dx;
    dy = aiDirection.dy;

    moveSnake();
    drawSnakeAndFood();
}

createBoard();
drawSnakeAndFood();
updateScore();
```

Dengan menggunakan kode ini, skor tertinggi akan disimpan dalam localStorage, dan peringkat skor tertinggi akan ditampilkan setelah permainan berakhir. Pastikan semua file yang diperlukan (index.html, style.css, script.js, ai.js, dan highscore.js) telah diperbarui dan berada dalam folder yang sama. Buka index.html di browser Anda untuk melihat hasilnya.
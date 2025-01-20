<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Memory Match Game</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f2f2f2;
        }

        .game-container {
            display: grid;
            grid-template-columns: repeat(4, 100px);
            grid-gap: 10px;
        }

        .card {
            width: 100px;
            height: 100px;
            background-color: #4CAF50;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            font-size: 32px;
            color: transparent;
            border-radius: 8px;
            transition: 0.3s;
        }

        .flipped {
            background-color: #fff;
            color: black;
        }

        .matched {
            background-color: #3e8e41;
            color: white;
        }

        .card-container {
            display: flex;
            justify-content: center;
            align-items: center;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <div class="game-container" id="gameBoard"></div>
    <div class="card-container">
        <button onclick="startGame()">Start Game</button>
    </div>

    <script src="game.js"></script>
</body>
</html>
let board = [];
let flippedCards = [];
let matchedPairs = 0;
let isGameOver = false;

const cardValues = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H'];
const doubledCardValues = [...cardValues, ...cardValues]; // Double the values to create pairs

const gameBoard = document.getElementById('gameBoard');

// Shuffle the cards randomly
function shuffleCards() {
    for (let i = doubledCardValues.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [doubledCardValues[i], doubledCardValues[j]] = [doubledCardValues[j], doubledCardValues[i]];
    }
}

// Create the game board
function createBoard() {
    gameBoard.innerHTML = ''; // Clear previous board

    shuffledValues = [...doubledCardValues];
    shuffleCards();

    shuffledValues.forEach((value, index) => {
        const card = document.createElement('div');
        card.classList.add('card');
        card.setAttribute('data-value', value);
        card.setAttribute('data-index', index);
        card.addEventListener('click', flipCard);
        gameBoard.appendChild(card);
    });
}

// Flip a card
function flipCard(event) {
    if (isGameOver) return;
    
    const card = event.target;
    if (flippedCards.length === 2 || card.classList.contains('flipped') || card.classList.contains('matched')) {
        return; // Ignore click if 2 cards are flipped already or it's already matched/flipped
    }

    card.classList.add('flipped');
    card.textContent = card.getAttribute('data-value');
    flippedCards.push(card);

    // Check if two cards are flipped
    if (flippedCards.length === 2) {
        setTimeout(checkMatch, 1000);
    }
}

// Check if the two flipped cards match
function checkMatch() {
    const [card1, card2] = flippedCards;

    if (card1.getAttribute('data-value') === card2.getAttribute('data-value')) {
        card1.classList.add('matched');
        card2.classList.add('matched');
        matchedPairs++;

        if (matchedPairs === cardValues.length) {
            setTimeout(() => alert('You win!'), 500);
            isGameOver = true;
        }
    } else {
        card1.classList.remove('flipped');
        card2.classList.remove('flipped');
        card1.textContent = '';
        card2.textContent = '';
    }

    flippedCards = [];
}

// Start a new game
function startGame() {
    isGameOver = false;
    matchedPairs = 0;
    flippedCards = [];
    createBoard();
}

startGame();

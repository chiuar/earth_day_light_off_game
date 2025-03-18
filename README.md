<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Earth Day Light Off Game</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #f0f8ff;
            margin: 0;
        }

        .game-container {
            text-align: center;
            background-color: #fff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }

        #scoreboard {
            margin-bottom: 20px;
        }

        #game-grid {
            display: grid;
            grid-template-columns: repeat(6, 1fr);
            gap: 10px;
            margin: 0 auto;
        }

        .light-bulb {
            width: 50px;
            height: 50px;
            background-color: yellow;
            border-radius: 50%;
            cursor: pointer;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 18px;
            color: black;
            transition: background-color 0.3s ease;
        }

        .light-bulb.off {
            background-color: gray;
        }

        #start-button, #play-again-button {
            margin-top: 20px;
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
        }

        /* Full-screen end message */
        #end-message {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(255, 255, 255, 0.9);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            font-size: 24px;
            color: green;
            display: none;
            z-index: 1000;
        }

        #end-message p {
            margin: 10px 0;
        }

        #end-message .first-line {
            font-size: 28px; /* Larger font size for the first line */
        }

        #end-message .second-line {
            font-size: 24px; /* Slightly smaller font size for the second line */
        }

        #end-message button {
            padding: 15px 30px;
            font-size: 18px;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <div class="game-container">
        <h1>Earth Day Light Off Game</h1>
        <div id="scoreboard">
            <p>Score: <span id="score">0</span></p>
            <p>Time Left: <span id="timer">15</span> seconds</p>
        </div>
        <div id="game-grid"></div>
        <button id="start-button">Start Game</button>
    </div>
    <div id="end-message">
        <p class="first-line">Turn off lights and electronics when not in use.</p>
        <p class="second-line">Every small action counts for a sustainable future!</p>
        <button id="play-again-button">Play Again</button>
    </div>
    <script>
        document.addEventListener("DOMContentLoaded", () => {
            const gameGrid = document.getElementById("game-grid");
            const scoreElement = document.getElementById("score");
            const timerElement = document.getElementById("timer");
            const startButton = document.getElementById("start-button");
            const playAgainButton = document.getElementById("play-again-button");
            const endMessage = document.getElementById("end-message");

            let score = 0;
            let timeLeft = 15;
            let gameStarted = false;

            const createLightBulbs = () => {
                for (let i = 0; i < 24; i++) {
                    const bulb = document.createElement("div");
                    bulb.classList.add("light-bulb");
                    bulb.addEventListener("click", () => {
                        if (!bulb.classList.contains("off")) {
                            bulb.classList.add("off");
                            score += 20;
                            scoreElement.textContent = score;
                            turnOnRandomLight();
                        }
                    });
                    gameGrid.appendChild(bulb);
                }
            };

            const turnOnRandomLight = () => {
                const bulbs = document.querySelectorAll(".light-bulb");
                const offBulbs = Array.from(bulbs).filter(bulb => bulb.classList.contains("off"));
                const onBulbs = bulbs.length - offBulbs.length;

                if (onBulbs === 0) { // If all bulbs are off, turn on exactly one light
                    const randomIndex = Math.floor(Math.random() * bulbs.length);
                    bulbs[randomIndex].classList.remove("off");
                } else if (onBulbs === 1) { // If only one light is on, do nothing
                    return;
                } else { // Otherwise, turn on another light
                    const randomIndex = Math.floor(Math.random() * offBulbs.length);
                    offBulbs[randomIndex].classList.remove("off");
                }
            };

            const startGame = () => {
                if (gameStarted) return;
                gameStarted = true;

                createLightBulbs();
                const bulbs = document.querySelectorAll(".light-bulb");
                bulbs.forEach((bulb) => {
                    if (Math.random() > 0.5) {
                        bulb.classList.add("off");
                    }
                });

                // Ensure at least one light is on at the start
                turnOnRandomLight();

                startButton.style.display = "none";

                const countdown = setInterval(() => {
                    timeLeft--;
                    timerElement.textContent = timeLeft;
                    if (timeLeft <= 0) {
                        clearInterval(countdown);
                        endMessage.style.display = "flex";
                        gameStarted = false;
                    }
                }, 1000);
            };

            startButton.addEventListener("click", startGame);
            playAgainButton.addEventListener("click", () => {
                location.reload();
            });
        });
    </script>
</body>
</html>

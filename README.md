<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jogo da Cobrinha</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #333;
            font-family: Arial, sans-serif;
        }
        canvas {
            background-color: #111;
            border: 1px solid #555;
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas" width="400" height="400"></canvas>

    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");

        const boxSize = 20; // Tamanho de cada quadrado (célula)
        const canvasSize = 400;
        let snake = [{ x: boxSize * 5, y: boxSize * 5 }];
        let direction = "RIGHT";
        let food = generateFood();
        let score = 0;

        // Controle do teclado
        document.addEventListener("keydown", changeDirection);

        function changeDirection(event) {
            const key = event.keyCode;
            if (key === 37 && direction !== "RIGHT") direction = "LEFT";
            else if (key === 38 && direction !== "DOWN") direction = "UP";
            else if (key === 39 && direction !== "LEFT") direction = "RIGHT";
            else if (key === 40 && direction !== "UP") direction = "DOWN";
        }

        function generateFood() {
            return {
                x: Math.floor(Math.random() * (canvas.width / boxSize)) * boxSize,
                y: Math.floor(Math.random() * (canvas.height / boxSize)) * boxSize,
            };
        }

        function drawFood() {
            ctx.fillStyle = "red";
            ctx.fillRect(food.x, food.y, boxSize, boxSize);
        }

        function drawSnake() {
            ctx.fillStyle = "lime";
            for (let i = 0; i < snake.length; i++) {
                ctx.fillRect(snake[i].x, snake[i].y, boxSize, boxSize);
            }
        }

        function updateSnakePosition() {
            let head = { x: snake[0].x, y: snake[0].y };

            if (direction === "LEFT") head.x -= boxSize;
            else if (direction === "UP") head.y -= boxSize;
            else if (direction === "RIGHT") head.x += boxSize;
            else if (direction === "DOWN") head.y += boxSize;

            snake.unshift(head);

            // Se a cobra come a comida
            if (head.x === food.x && head.y === food.y) {
                score++;
                food = generateFood();
            } else {
                snake.pop(); // Remove a cauda
            }
        }

        function checkCollision() {
            // Verifica colisão com paredes
            if (snake[0].x < 0 || snake[0].x >= canvas.width || snake[0].y < 0 || snake[0].y >= canvas.height) {
                return true;
            }

            // Verifica colisão com o próprio corpo
            for (let i = 1; i < snake.length; i++) {
                if (snake[0].x === snake[i].x && snake[0].y === snake[i].y) {
                    return true;
                }
            }
            return false;
        }

        function gameLoop() {
            if (checkCollision()) {
                alert("Fim de Jogo! Sua pontuação: " + score);
                document.location.reload();
                return;
            }

            ctx.clearRect(0, 0, canvas.width, canvas.height);

            drawFood();
            drawSnake();
            updateSnakePosition();
        }

        setInterval(gameLoop, 100); // Controla a velocidade do jogo
    </script>
</body>
</html>

<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Goli Maar Game</title>
    <style>
        /* Full screen aur styling ka code */
        body {
            margin: 0;
            padding: 0;
            overflow: hidden;
            background-color: #2c3e50;
            font-family: Arial, sans-serif;
            touch-action: none; /* Mobile par zoom/scroll band karne ke liye */
        }
        canvas {
            display: block;
        }
        /* Buttons ki styling */
        .controls {
            position: absolute;
            bottom: 30px;
            width: 100%;
            display: flex;
            justify-content: space-around;
            pointer-events: none; /* Taki canvas par bhi touch kaam kare */
        }
        .btn {
            pointer-events: auto;
            padding: 20px 30px;
            font-size: 20px;
            font-weight: bold;
            color: white;
            border: none;
            border-radius: 50px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.3);
            user-select: none;
            cursor: pointer;
        }
        .btn:active {
            transform: scale(0.95);
        }
        #btn-move {
            background-color: #3498db; /* Neela rang */
        }
        #btn-shoot {
            background-color: #e74c3c; /* Laal rang */
        }
        #score-board {
            position: absolute;
            top: 20px;
            left: 20px;
            color: white;
            font-size: 24px;
            font-weight: bold;
        }
    </style>
</head>
<body>

    <div id="score-board">Score: <span id="score">0</span></div>
    <canvas id="gameCanvas"></canvas>
    
    <div class="controls">
        <button id="btn-move" class="btn">🚀 Aage Badho</button>
        <button id="btn-shoot" class="btn">🔫 Goli Maaro</button>
    </div>

    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");

        // Full screen set karna
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;

        let score = 0;
        let isMoving = false;

        // Player (Aapka Character)
        const player = {
            x: 50,
            y: canvas.height / 2 - 25,
            width: 50,
            height: 50,
            color: '#2ecc71',
            speed: 5
        };

        // Goli aur Dushman (Bullets and Enemies)
        const bullets = [];
        const enemies = [];
        let enemySpawnTimer = 0;

        // Buttons ke elements
        const btnMove = document.getElementById('btn-move');
        const btnShoot = document.getElementById('btn-shoot');

        // Aage Badhne ka logic (Touch aur Mouse ke liye)
        const startMoving = () => isMoving = true;
        const stopMoving = () => isMoving = false;
        
        btnMove.addEventListener('mousedown', startMoving);
        btnMove.addEventListener('mouseup', stopMoving);
        btnMove.addEventListener('touchstart', (e) => { e.preventDefault(); startMoving(); });
        btnMove.addEventListener('touchend', (e) => { e.preventDefault(); stopMoving(); });

        // Goli Maarne ka logic
        const shoot = () => {
            bullets.push({
                x: player.x + player.width,
                y: player.y + player.height / 2 - 5,
                width: 20,
                height: 10,
                color: '#f1c40f',
                speed: 10
            });
        };
        
        btnShoot.addEventListener('mousedown', shoot);
        btnShoot.addEventListener('touchstart', (e) => { e.preventDefault(); shoot(); });

        // Dushman banane ka function
        function spawnEnemy() {
            const size = Math.random() * 30 + 20;
            enemies.push({
                x: canvas.width,
                y: Math.random() * (canvas.height - size - 100) + 50, // Button ke area se upar
                width: size,
                height: size,
                color: '#e67e22',
                speed: Math.random() * 3 + 2
            });
        }

        // Game Loop (Jo game ko chalata hai)
        function update() {
            // Player movement
            if (isMoving && player.x < canvas.width - player.width) {
                player.x += player.speed;
            } else if (!isMoving && player.x > 50) {
                player.x -= 2; // Piche wapas aane ke liye thoda sa
            }

            // Goli ki movement
            for (let i = 0; i < bullets.length; i++) {
                bullets[i].x += bullets[i].speed;
                // Screen se bahar jaane par goli hata do
                if (bullets[i].x > canvas.width) {
                    bullets.splice(i, 1);
                    i--;
                }
            }

            // Dushman ki movement
            enemySpawnTimer++;
            if (enemySpawnTimer > 60) { // Har thodi der mein naya dushman
                spawnEnemy();
                enemySpawnTimer = 0;
            }

            for (let i = 0; i < enemies.length; i++) {
                enemies[i].x -= enemies[i].speed;

                // Collision Logic (Goli dushman ko lagi ya nahi)
                for (let j = 0; j < bullets.length; j++) {
                    if (
                        bullets[j].x < enemies[i].x + enemies[i].width &&
                        bullets[j].x + bullets[j].width > enemies[i].x &&
                        bullets[j].y < enemies[i].y + enemies[i].height &&
                        bullets[j].height + bullets[j].y > enemies[i].y
                    ) {
                        // Dushman mar gaya
                        enemies.splice(i, 1);
                        bullets.splice(j, 1);
                        score += 10;
                        document.getElementById('score').innerText = score;
                        i--; // Loop adjust karne ke liye
                        break;
                    }
                }
                
                // Screen se bahar nikalne par dushman hatao
                if (enemies[i] && enemies[i].x + enemies[i].width < 0) {
                    enemies.splice(i, 1);
                    i--;
                }
            }
        }

        // Screen par draw karne ka function
        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height); // Screen saaf karo

            // Draw Player (Aapka dibba)
            ctx.fillStyle = player.color;
            ctx.fillRect(player.x, player.y, player.width, player.height);

            // Draw Bullets (Goliyan)
            for (let bullet of bullets) {
                ctx.fillStyle = bullet.color;
                ctx.fillRect(bullet.x, bullet.y, bullet.width, bullet.height);
            }

            // Draw Enemies (Dushman)
            for (let enemy of enemies) {
                ctx.fillStyle = enemy.color;
                ctx.fillRect(enemy.x, enemy.y, enemy.width, enemy.height);
            }
        }

        // Main game loop chalana
        function gameLoop() {
            update();
            draw();
            requestAnimationFrame(gameLoop);
        }

        // Window resize hone par adjust karna
        window.addEventListener('resize', () => {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
            player.y = canvas.height / 2 - 25;
        });

        // Game Start
        gameLoop();
    </script>
</body>
</html>

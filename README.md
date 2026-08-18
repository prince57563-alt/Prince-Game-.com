<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>एरोप्लेन गेम - Full Screen</title>
    <style>
        body, html {
            margin: 0;
            padding: 0;
            width: 100%;
            height: 100%;
            overflow: hidden;
            background-color: #87CEEB;
            font-family: 'Arial', sans-serif;
            touch-action: none;
        }

        #orientation-warning {
            display: none;
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            background: #2c3e50;
            color: white;
            z-index: 1000;
            text-align: center;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            font-size: 24px;
            font-weight: bold;
        }

        @media screen and (orientation: portrait) {
            #orientation-warning { display: flex; }
            #game-container { display: none; }
        }

        #game-container {
            position: relative;
            width: 100%;
            height: 100%;
        }

        canvas {
            display: block;
            width: 100%;
            height: 100%;
        }

        #ui-layer {
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            pointer-events: none;
        }

        #score-board {
            position: absolute;
            top: 20px;
            left: 20px;
            font-size: 30px;
            font-weight: bold;
            color: #fff;
            text-shadow: 2px 2px 4px #000;
        }

        .btn {
            padding: 15px 40px;
            font-size: 24px;
            font-weight: bold;
            background-color: #ff4757;
            color: white;
            border: none;
            border-radius: 10px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.3);
            pointer-events: auto;
            cursor: pointer;
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }
    </style>
</head>
<body>

    <div id="orientation-warning">
        <div style="font-size: 50px;">🔄</div>
        <p>गेम खेलने के लिए मोबाइल को<br>आड़ा (Landscape) पकड़ें!</p>
    </div>

    <div id="game-container">
        <canvas id="gameCanvas"></canvas>
        <div id="ui-layer">
            <div id="score-board">Score: <span id="score">0</span></div>
            <button id="startBtn" class="btn">🚀 Start Game (Full Screen)</button>
        </div>
    </div>

    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");
        const startBtn = document.getElementById("startBtn");
        const scoreElement = document.getElementById("score");

        let frames = 0;
        let score = 0;
        let gameRunning = false;
        let pipes = [];

        function resize() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        }
        window.addEventListener('resize', resize);
        resize();

        // --- फुल स्क्रीन करने का फंक्शन ---
        function toggleFullScreen() {
            let elem = document.documentElement;
            if (!document.fullscreenElement && !document.mozFullScreenElement && !document.webkitFullscreenElement && !document.msFullscreenElement) {
                if (elem.requestFullscreen) { elem.requestFullscreen(); }
                else if (elem.webkitRequestFullscreen) { elem.webkitRequestFullscreen(); }
                else if (elem.msRequestFullscreen) { elem.msRequestFullscreen(); }
            }
        }

        // --- साउंड सिस्टम ---
        const AudioContext = window.AudioContext || window.webkitAudioContext;
        let audioCtx;

        function initAudio() {
            if (!audioCtx) audioCtx = new AudioContext();
            if(audioCtx.state === 'suspended') audioCtx.resume();
        }

        function playJumpSound() {
            if(!audioCtx) return;
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            osc.type = "sine";
            osc.frequency.setValueAtTime(300, audioCtx.currentTime);
            osc.frequency.exponentialRampToValueAtTime(600, audioCtx.currentTime + 0.1);
            gain.gain.setValueAtTime(0.5, audioCtx.currentTime);
            gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.1);
            osc.connect(gain); gain.connect(audioCtx.destination);
            osc.start(); osc.stop(audioCtx.currentTime + 0.1);
        }

        function playCrashSound() {
            if(!audioCtx) return;
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            osc.type = "sawtooth";
            osc.frequency.setValueAtTime(150, audioCtx.currentTime);
            osc.frequency.exponentialRampToValueAtTime(40, audioCtx.currentTime + 0.3);
            gain.gain.setValueAtTime(0.5, audioCtx.currentTime);
            gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.3);
            osc.connect(gain); gain.connect(audioCtx.destination);
            osc.start(); osc.stop(audioCtx.currentTime + 0.3);
        }

        // --- प्लेन की सेटिंग ---
        const plane = {
            x: 100,
            y: canvas.height / 2,
            width: 60,
            height: 40,
            gravity: 0.25,
            lift: -6,
            velocity: 0,
            draw: function() {
                ctx.font = "50px Arial";
                ctx.fillText("✈️", this.x, this.y);
            },
            update: function() {
                this.velocity += this.gravity;
                this.y += this.velocity;
                if (this.y + this.height >= canvas.height || this.y <= 30) { gameOver(); }
            },
            flap: function() {
                this.velocity = this.lift;
                playJumpSound();
            }
        };

        // --- रुकावटें (पहाड़/बिल्डिंग) ---
        function drawPipes() {
            for (let i = 0; i < pipes.length; i++) {
                let p = pipes[i];
                p.x -= 4; 

                ctx.fillStyle = "#2ecc71"; 
                ctx.fillRect(p.x, 0, p.width, p.top);
                ctx.fillRect(p.x, canvas.height - p.bottom, p.width, p.bottom);

                // टक्कर चेक करना
                if (plane.x + 40 > p.x && plane.x < p.x + p.width) {
                    if (plane.y - 30 < p.top || plane.y + 10 > canvas.height - p.bottom) { gameOver(); }
                }

                // स्कोर
                if (p.x === 96) {
                    score++;
                    scoreElement.innerText = score;
                }

                if (p.x + p.width < 0) {
                    pipes.shift();
                    i--;
                }
            }

            if (frames % 100 === 0) {
                let gap = 200;
                let minHeight = 50;
                let maxHeight = canvas.height - gap - minHeight;
                let topHeight = Math.floor(Math.random() * (maxHeight - minHeight + 1) + minHeight);
                
                pipes.push({ x: canvas.width, width: 60, top: topHeight, bottom: canvas.height - (topHeight + gap) });
            }
        }

        // --- गेम लूप ---
        function loop() {
            if (!gameRunning) return;
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            plane.draw();
            plane.update();
            drawPipes();

            frames++;
            requestAnimationFrame(loop);
        }

        // --- गेम ओवर ---
        function gameOver() {
            gameRunning = false;
            playCrashSound();
            startBtn.style.display = "block";
            startBtn.innerText = "🔄 Play Again";
        }

        // --- स्टार्ट और कंट्रोल्स ---
        function startGame() {
            toggleFullScreen(); // बटन दबाते ही फुल स्क्रीन होगा
            initAudio(); 
            plane.y = canvas.height / 2;
            plane.velocity = 0;
            pipes = [];
            score = 0;
            frames = 0;
            scoreElement.innerText = score;
            startBtn.style.display = "none";
            gameRunning = true;
            loop();
        }

        window.addEventListener("mousedown", () => { if(gameRunning) plane.flap(); });
        window.addEventListener("touchstart", (e) => {
            if(gameRunning) {
                e.preventDefault();
                plane.flap();
            }
        });

        startBtn.addEventListener("click", startGame);

    </script>
</body>
</html>

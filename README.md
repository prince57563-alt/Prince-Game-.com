<!DOCTYPE html>
<html lang="hi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Neon Space Shooter</title>
  <style>
    /* गेम को फुल-स्क्रीन बनाने के लिए CSS */
    body { 
      margin: 0; 
      overflow: hidden; 
      background-color: #0f172a; 
      color: white; 
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
    }
    canvas { 
      display: block; 
    }
    #ui { 
      position: absolute; 
      top: 15px; 
      left: 15px; 
      pointer-events: none; 
    }
    #score { 
      font-size: 28px; 
      font-weight: bold; 
      color: #38bdf8; 
      text-shadow: 0 0 10px #38bdf8;
    }
    #game-over { 
      display: none; 
      position: absolute; 
      top: 50%; 
      left: 50%; 
      transform: translate(-50%, -50%); 
      text-align: center; 
    }
    #game-over h1 { 
      color: #ef4444; 
      font-size: 56px; 
      margin-bottom: 20px; 
      text-shadow: 0 0 20px #ef4444;
    }
    button { 
      padding: 12px 30px; 
      font-size: 22px; 
      background: #38bdf8; 
      border: none; 
      border-radius: 8px; 
      cursor: pointer; 
      color: #0f172a; 
      font-weight: bold;
      transition: 0.2s;
    }
    button:hover { 
      background: #7dd3fc; 
      transform: scale(1.1);
    }
  </style>
</head>
<body>

  <div id="ui">
    <div id="score">Score: 0</div>
  </div>
  
  <div id="game-over">
    <h1>GAME OVER</h1>
    <button onclick="location.reload()">फिर से खेलें</button>
  </div>
  
  <canvas id="gameCanvas"></canvas>

  <script>
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');
    
    // कैनवास को फुल स्क्रीन सेट करना
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;

    let score = 0;
    let gameOver = false;

    // प्लेयर (शिप) की सेटिंग
    const player = {
      x: canvas.width / 2,
      y: canvas.height - 80,
      width: 50,
      height: 50,
      color: '#38bdf8'
    };

    const bullets = [];
    const enemies = [];
    let frameCount = 0;

    // माउस या टच से प्लेयर को चलाना
    window.addEventListener('mousemove', (e) => {
      player.x = e.clientX - player.width / 2;
    });
    window.addEventListener('touchmove', (e) => {
      player.x = e.touches[0].clientX - player.width / 2;
    });

    // गेम का मुख्य लूप
    function loop() {
      if (gameOver) {
        document.getElementById('game-over').style.display = 'block';
        return;
      }

      // बैकग्राउंड में मोशन ट्रेल इफेक्ट
      ctx.fillStyle = 'rgba(15, 23, 42, 0.4)'; 
      ctx.fillRect(0, 0, canvas.width, canvas.height);

      // प्लेयर को ड्रा करना (त्रिभुज आकार का शिप)
      ctx.fillStyle = player.color;
      ctx.shadowBlur = 15;
      ctx.shadowColor = player.color;
      ctx.beginPath();
      ctx.moveTo(player.x + player.width / 2, player.y);
      ctx.lineTo(player.x + player.width, player.y + player.height);
      ctx.lineTo(player.x, player.y + player.height);
      ctx.fill();
      ctx.shadowBlur = 0; // बाकी चीज़ों से ग्लो हटाना

      // ऑटोमैटिक गन फायरिंग (हर 10 फ्रेम में एक गोली)
      if (frameCount % 10 === 0) {
        bullets.push({
          x: player.x + player.width / 2 - 4,
          y: player.y,
          width: 8,
          height: 20,
          color: '#facc15',
          speed: 12
        });
      }

      // गोलियों को चलाना
      for (let i = 0; i < bullets.length; i++) {
        let b = bullets[i];
        b.y -= b.speed;
        ctx.fillStyle = b.color;
        ctx.fillRect(b.x, b.y, b.width, b.height);

        // स्क्रीन से बाहर जाने पर गोली हटाना
        if (b.y < 0) { bullets.splice(i, 1); i--; }
      }

      // नए दुश्मन बनाना
      if (frameCount % 30 === 0) {
        let size = Math.random() * 30 + 25;
        enemies.push({
          x: Math.random() * (canvas.width - size),
          y: -50,
          width: size,
          height: size,
          color: '#ef4444',
          speed: Math.random() * 3 + 3
        });
      }

      // दुश्मनों को नीचे लाना और टकराव जाँचना
      for (let i = 0; i < enemies.length; i++) {
        let e = enemies[i];
        e.y += e.speed;
        ctx.fillStyle = e.color;
        ctx.fillRect(e.x, e.y, e.width, e.height);

        // अगर दुश्मन प्लेयर से टकरा जाए (Game Over)
        if (player.x < e.x + e.width && player.x + player.width > e.x &&
            player.y < e.y + e.height && player.y + player.height > e.y) {
          gameOver = true;
        }

        // अगर गोली दुश्मन को लग जाए (Score++)
        for (let j = 0; j < bullets.length; j++) {
          let b = bullets[j];
          if (b.x < e.x + e.width && b.x + b.width > e.x &&
              b.y < e.y + e.height && b.y + b.height > e.y) {
            
            enemies.splice(i, 1); // दुश्मन मरा
            bullets.splice(j, 1); // गोली गायब
            score += 10;
            document.getElementById('score').innerText = `Score: ${score}`;
            i--;
            break;
          }
        }

        // स्क्रीन के नीचे से निकलने वाले दुश्मनों को हटाना
        if (e && e.y > canvas.height) { enemies.splice(i, 1); i--; }
      }

      frameCount++;
      requestAnimationFrame(loop);
    }

    // स्क्रीन साइज़ बदलने पर गेम को एडजस्ट करना
    window.addEventListener('resize', () => {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
      player.y = canvas.height - 80;
    });

    // गेम शुरू करें
    loop();
  </script>
</body>
</html>

<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>एरोप्लेन गेम</title>
    <style>
        body, html {
            margin: 0;
            padding: 0;
            width: 100%;
            height: 100%;
            overflow: hidden;
            background-color: #87CEEB; /* आसमान का रंग */
            font-family: 'Arial', sans-serif;
            touch-action: none; /* मोबाइल पर जूम और स्क्रॉल रोके */
        }

        /* मोबाइल सीधा होने पर यह चेतावनी दिखेगी */
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

    <!-- रोटेट स्क्रीन वार्निंग -->
    <div id="orientation-warning">
        <div style="font-size: 50px;">🔄</div>
        <p>गेम खेलने के लिए मोबाइल को<br>आड़ा (Landscape) पकड़ें!</p>
    </div>

    <!-- गेम एरिया -->
    <div id="game-container">
        <canvas id="gameCanvas"></canvas>
        <div id="ui-layer">
            <div id="score-board">Score: <span id="score">0</span></div>
            <button id="startBtn" class="btn">🚀 Start Game</button>
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

        // स्क्रीन साइज सेट करें
        function resize() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        }
        window.addEventListener('resize', resize);
        resize();

        // --- साउंड सिस्टम (बिना किसी ऑडियो फाइल के) ---
        const AudioContext = window.AudioContext || window.webkitAudioContext;
        let audioCtx;

        function initAudio() {
            if (!audioCtx) {
                audioCtx = new AudioContext();
            }
            if(audioCtx.state === 'suspended') {
                audioCtx.resume();
            }
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
            osc.connect(gain);
            gain.connect(audioCtx.destination);
            osc.start();
            osc.stop(audioCtx.currentTime + 0.1);
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
            osc.connect(gain);
            gain.connect(audioCtx.destination);
            osc.start();
            osc.stop(audioCtx.currentTime + 0.3);
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
                
                // जमीन या आसमान से टकराना
                if (this.y + this.height >= canvas.height || this.y <= 30) {
                    gameOver();
                }
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
                p.x -= 4; // स्पीड

                // खंभे बनाना
                ctx.fillStyle = "#2ecc71"; 
                ctx.fillRect(p.x, 0, p.width, p.top);
                ctx.fillRect(p.x, canvas.height - p.bottom, p.width, p.bottom);

                // टक्कर चेक करना (Collision Detection)
                if (plane.x + 40 > p.x && plane.x < p.x + p.width) {
                    // चूँकि इमोजी ऊपर से ड्रॉ होता है, थोड़ा मार्जिन रखा है
                    if (plane.y - 30 < p.top || plane.y + 10 > canvas.height - p.bottom) {
                        gameOver();
                    }
                }

                // स्कोर बढ़ाना
                if (p.x === 96) {
                    score++;
                    scoreElement.innerText = score;
                }

                // स्क्रीन से बाहर जाने पर हटा दें
                if (p.x + p.width < 0) {
                    pipes.shift();
                    i--;
                }
            }

            // नए खंभे जोड़ना
            if (frames % 100 === 0) {
                let gap = 200; // प्लेन के निकलने की जगह
                let minHeight = 50;
                let maxHeight = canvas.height - gap - minHeight;
                let topHeight = Math.floor(Math.random() * (maxHeight - minHeight + 1) + minHeight);
                
                pipes.push({
                    x: canvas.width,
                    width: 60,
                    top: topHeight,
                    bottom: canvas.height - (topHeight + gap)
                });
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
            initAudio(); // साउंड शुरू करें
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

        // माउस क्लिक और मोबाइल टच
        window.addEventListener("mousedown", () => {
            if(gameRunning) plane.flap();
        });
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

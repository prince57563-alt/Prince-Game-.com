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

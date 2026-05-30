<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>✈️ Game Pesawat Tempur - Tembak Musuh!</title>
    <style>
        :root {
            --bg: #0a0a1a;
            --panel: #111130;
            --accent: #ff4757;
            --gold: #ffa502;
            --blue: #3742fa;
            --green: #2ed573;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: var(--bg);
            background-image:
                radial-gradient(ellipse at center, #1a1a3e 0%, #0a0a1a 70%),
                radial-gradient(circle at 20% 80%, rgba(255, 71, 87, 0.08) 0%, transparent 50%),
                radial-gradient(circle at 80% 20%, rgba(55, 66, 250, 0.08) 0%, transparent 50%);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            font-family: 'Segoe UI', 'Arial', sans-serif;
            user-select: none;
            -webkit-tap-highlight-color: transparent;
            overflow-y: auto;
            padding: 10px;
        }

        .game-wrapper {
            background: var(--panel);
            border-radius: 24px;
            padding: 20px;
            box-shadow:
                0 20px 60px rgba(0, 0, 0, 0.6),
                0 0 0 2px rgba(255, 255, 255, 0.05),
                inset 0 1px 0 rgba(255, 255, 255, 0.03);
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 16px;
            max-width: 520px;
            width: 100%;
        }

        .game-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            width: 100%;
            padding: 0 10px;
            flex-wrap: wrap;
            gap: 8px;
        }

        .game-title {
            font-size: 1.6em;
            font-weight: 900;
            letter-spacing: 1px;
            background: linear-gradient(135deg, #ffa502, #ff6348, #ff4757);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            text-shadow: none;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .game-title span {
            font-size: 1.3em;
            -webkit-text-fill-color: initial;
            animation: bounce-icon 0.6s ease-in-out infinite alternate;
        }
        @keyframes bounce-icon {
            from {
                transform: translateY(0);
            }
            to {
                transform: translateY(-8px);
            }
        }

        .score-panel {
            display: flex;
            gap: 16px;
            align-items: center;
            flex-wrap: wrap;
        }
        .info-badge {
            background: rgba(255, 255, 255, 0.04);
            border: 1px solid rgba(255, 255, 255, 0.12);
            border-radius: 30px;
            padding: 8px 16px;
            color: #ddd;
            font-weight: 700;
            font-size: 0.9em;
            display: flex;
            align-items: center;
            gap: 6px;
            letter-spacing: 0.5px;
        }
        .info-badge .icon {
            font-size: 1.2em;
        }
        .info-badge.score-badge {
            border-color: var(--gold);
            color: var(--gold);
            background: rgba(255, 165, 2, 0.08);
        }
        .info-badge.lives-badge {
            border-color: var(--accent);
            color: var(--accent);
            background: rgba(255, 71, 87, 0.08);
        }
        .info-badge.level-badge {
            border-color: var(--green);
            color: var(--green);
            background: rgba(46, 213, 115, 0.08);
        }

        canvas {
            border-radius: 16px;
            background: radial-gradient(ellipse at center bottom, #0d0d2b 0%, #060615 100%);
            box-shadow:
                inset 0 -40px 80px rgba(0, 0, 0, 0.5),
                0 8px 32px rgba(0, 0, 0, 0.5);
            display: block;
            width: 100%;
            max-width: 460px;
            height: auto;
            aspect-ratio: 460 / 600;
            cursor: none;
            border: 1px solid rgba(255, 255, 255, 0.08);
        }

        .btn-row {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
            justify-content: center;
            width: 100%;
        }

        .btn {
            padding: 10px 20px;
            border-radius: 30px;
            border: none;
            font-weight: 700;
            font-size: 0.9em;
            cursor: pointer;
            letter-spacing: 0.5px;
            transition: all 0.2s ease;
            font-family: inherit;
            position: relative;
            overflow: hidden;
        }
        .btn:active {
            transform: scale(0.94);
        }
        .btn-start {
            background: var(--green);
            color: #000;
            box-shadow: 0 6px 20px rgba(46, 213, 115, 0.35);
        }
        .btn-start:hover {
            box-shadow: 0 8px 28px rgba(46, 213, 115, 0.5);
            transform: translateY(-2px);
        }
        .btn-pause {
            background: var(--gold);
            color: #000;
            box-shadow: 0 6px 20px rgba(255, 165, 2, 0.35);
        }
        .btn-pause:hover {
            box-shadow: 0 8px 28px rgba(255, 165, 2, 0.5);
            transform: translateY(-2px);
        }
        .btn-restart {
            background: #fff;
            color: #000;
            box-shadow: 0 6px 20px rgba(255, 255, 255, 0.25);
        }
        .btn-restart:hover {
            box-shadow: 0 8px 28px rgba(255, 255, 255, 0.4);
            transform: translateY(-2px);
        }
        .btn-fire {
            background: var(--accent);
            color: #fff;
            font-size: 1.1em;
            padding: 14px 32px;
            letter-spacing: 1px;
            box-shadow: 0 8px 28px rgba(255, 71, 87, 0.5);
            animation: pulse-fire 1.5s ease-in-out infinite;
        }
        @keyframes pulse-fire {
            0%,
            100% {
                box-shadow: 0 8px 28px rgba(255, 71, 87, 0.5);
            }
            50% {
                box-shadow: 0 12px 40px rgba(255, 71, 87, 0.75);
            }
        }
        .btn-fire:hover {
            transform: translateY(-3px);
            box-shadow: 0 14px 36px rgba(255, 71, 87, 0.7);
        }
        .btn-fire:active {
            transform: scale(0.9);
            box-shadow: 0 4px 14px rgba(255, 71, 87, 0.6);
        }

        .dpad {
            display: grid;
            grid-template-columns: 60px 60px 60px;
            grid-template-rows: 60px 60px 60px;
            gap: 4px;
            justify-content: center;
        }
        .dpad-btn {
            width: 56px;
            height: 56px;
            border-radius: 16px;
            border: 2px solid rgba(255, 255, 255, 0.2);
            background: rgba(255, 255, 255, 0.06);
            color: #fff;
            font-size: 1.6em;
            cursor: pointer;
            transition: all 0.15s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            -webkit-tap-highlight-color: transparent;
            touch-action: manipulation;
            user-select: none;
        }
        .dpad-btn:active {
            background: rgba(255, 255, 255, 0.2);
            border-color: #fff;
            transform: scale(0.88);
            box-shadow: 0 0 20px rgba(255, 255, 255, 0.3);
        }
        .dpad-empty {
            width: 56px;
            height: 56px;
        }
        .dpad-center {
            background: rgba(255, 255, 255, 0.03);
            border-radius: 50%;
            font-size: 0.7em;
            color: rgba(255, 255, 255, 0.3);
            cursor: default;
            border: 1px dashed rgba(255, 255, 255, 0.1);
        }

        .mobile-controls {
            display: flex;
            align-items: center;
            gap: 20px;
            flex-wrap: wrap;
            justify-content: center;
        }

        .instruction {
            color: rgba(255, 255, 255, 0.5);
            font-size: 0.75em;
            text-align: center;
            letter-spacing: 0.5px;
            margin-top: -4px;
        }

        @media (max-width: 480px) {
            .game-wrapper {
                padding: 10px;
                gap: 10px;
                border-radius: 16px;
            }
            canvas {
                border-radius: 10px;
                max-width: 100%;
            }
            .btn {
                padding: 8px 14px;
                font-size: 0.78em;
            }
            .dpad-btn {
                width: 42px;
                height: 42px;
                font-size: 1.2em;
                border-radius: 12px;
            }
            .dpad {
                grid-template-columns: 46px 46px 46px;
                grid-template-rows: 46px 46px 46px;
                gap: 3px;
            }
            .dpad-empty {
                width: 42px;
                height: 42px;
            }
            .game-title {
                font-size: 1.2em;
            }
            .btn-fire {
                padding: 10px 22px;
                font-size: 0.9em;
            }
        }
    </style>
</head>
<body>

    <div class="game-wrapper">
        <!-- Header -->
        <div class="game-header">
            <div class="game-title">
                <span>✈️</span> PESAWAT TEMPUR
            </div>
            <div class="score-panel">
                <div class="info-badge score-badge">
                    <span class="icon">⭐</span> Skor: <strong id="scoreDisplay">0</strong>
                </div>
                <div class="info-badge lives-badge">
                    <span class="icon">❤️</span> Nyawa: <strong id="livesDisplay">5</strong>
                </div>
                <div class="info-badge level-badge">
                    <span class="icon">📈</span> Lvl: <strong id="levelDisplay">1</strong>
                </div>
            </div>
        </div>

        <!-- Canvas Game -->
        <canvas id="gameCanvas" width="460" height="600"></canvas>

        <!-- Instruksi -->
        <div class="instruction">
            🎮 <b>Keyboard:</b> ← → ↑ ↓ atau WASD untuk gerak & <b>SPASI</b> untuk tembak &nbsp;|&nbsp; 📱 <b>Mobile:</b> Gunakan tombol di bawah
        </div>

        <!-- Tombol Aksi -->
        <div class="btn-row">
            <button class="btn btn-start" id="btnStart" title="Mulai Game">▶ Mulai</button>
            <button class="btn btn-pause" id="btnPause" title="Jeda / Lanjutkan">⏯️ Jeda</button>
            <button class="btn btn-restart" id="btnRestart" title="Ulangi Game">🔄 Ulang</button>
        </div>

        <!-- Kontrol Mobile: D-Pad + Tombol Tembak -->
        <div class="mobile-controls" id="mobileControls">
            <!-- D-Pad -->
            <div class="dpad">
                <div class="dpad-empty"></div>
                <button class="dpad-btn" id="btnUp" title="Atas">⬆️</button>
                <div class="dpad-empty"></div>

                <button class="dpad-btn" id="btnLeft" title="Kiri">⬅️</button>
                <div class="dpad-btn dpad-center">🎯</div>
                <button class="dpad-btn" id="btnRight" title="Kanan">➡️</button>

                <div class="dpad-empty"></div>
                <button class="dpad-btn" id="btnDown" title="Bawah">⬇️</button>
                <div class="dpad-empty"></div>
            </div>

            <!-- Tombol Tembak Besar -->
            <button class="btn btn-fire" id="btnFire" title="TEMBAK!">💥 TEMBAK!</button>
        </div>
    </div>

    <script>
        (function() {
            // ============ ELEMEN ============
            const canvas = document.getElementById('gameCanvas');
            const ctx = canvas.getContext('2d');
            const scoreDisplay = document.getElementById('scoreDisplay');
            const livesDisplay = document.getElementById('livesDisplay');
            const levelDisplay = document.getElementById('levelDisplay');

            // ============ DIMENSI ============
            const W = canvas.width;
            const H = canvas.height;

            // ============ STATE GAME ============
            let gameState = 'idle'; // 'idle' | 'playing' | 'paused' | 'gameover'
            let score = 0;
            let lives = 5;
            let level = 1;
            let frameCount = 0;

            // ============ PLAYER ============
            const player = {
                x: W / 2,
                y: H - 80,
                w: 44,
                h: 50,
                speed: 5,
                cooldown: 0,
                cooldownMax: 14, // frame antar tembakan
            };

            // ============ ARRAYS ============
            let bullets = [];
            let enemies = [];
            let explosions = [];
            let stars = [];

            // ============ INPUT STATE ============
            const keys = {
                ArrowLeft: false,
                ArrowRight: false,
                ArrowUp: false,
                ArrowDown: false,
                KeyA: false,
                KeyD: false,
                KeyW: false,
                KeyS: false,
                Space: false,
            };
            let mouseFire = false;

            // ============ BINTANG LATAR ============
            function initStars() {
                stars = [];
                for (let i = 0; i < 80; i++) {
                    stars.push({
                        x: Math.random() * W,
                        y: Math.random() * H,
                        r: Math.random() * 1.6 + 0.4,
                        speed: Math.random() * 1.5 + 0.3,
                        twinkle: Math.random() * Math.PI * 2,
                        twinkleSpeed: Math.random() * 0.04 + 0.01,
                    });
                }
            }
            initStars();

            // ============ FUNGSI BANTU ============
            function spawnEnemy() {
                const size = 30 + Math.random() * 16;
                const x = size / 2 + Math.random() * (W - size);
                const speedY = 1.2 + Math.random() * 2.0 + level * 0.3;
                const speedX = (Math.random() - 0.5) * 1.8;
                enemies.push({
                    x: x,
                    y: -size,
                    w: size,
                    h: size,
                    speedX: speedX,
                    speedY: speedY,
                    hp: level >= 4 ? 2 : 1, // mulai level 4, musuh perlu 2 tembakan
                    maxHp: level >= 4 ? 2 : 1,
                    color: level >= 5 ? '#ff4757' : level >= 3 ? '#ffa502' : '#eccc68',
                });
            }

            function spawnExplosion(x, y, size = 1) {
                explosions.push({
                    x: x,
                    y: y,
                    radius: 4,
                    maxRadius: 18 * size,
                    life: 1,
                    decay: 0.03 + Math.random() * 0.04,
                });
            }

            function resetGame() {
                player.x = W / 2;
                player.y = H - 80;
                player.cooldown = 0;
                bullets = [];
                enemies = [];
                explosions = [];
                score = 0;
                lives = 5;
                level = 1;
                frameCount = 0;
                updateUI();
            }

            function updateUI() {
                scoreDisplay.textContent = score;
                livesDisplay.textContent = lives;
                levelDisplay.textContent = level;
            }

            // ============ GAME LOOP ============
            function update() {
                if (gameState !== 'playing') return;

                frameCount++;

                // Update bintang
                stars.forEach(s => {
                    s.y += s.speed;
                    s.twinkle += s.twinkleSpeed;
                    if (s.y > H + 5) {
                        s.y = -5;
                        s.x = Math.random() * W;
                    }
                });

                // Spawn musuh
                const spawnRate = Math.max(20, 55 - level * 4);
                if (frameCount % spawnRate === 0) {
                    spawnEnemy();
                }
                // Tambahan spawn untuk level tinggi
                if (level >= 3 && frameCount % (spawnRate * 3) === 0) {
                    spawnEnemy();
                }

                // Update player
                let moveX = 0;
                let moveY = 0;
                if (keys.ArrowLeft || keys.KeyA) moveX = -1;
                if (keys.ArrowRight || keys.KeyD) moveX = 1;
                if (keys.ArrowUp || keys.KeyW) moveY = -1;
                if (keys.ArrowDown || keys.KeyS) moveY = 1;

                // Normalisasi diagonal
                if (moveX !== 0 && moveY !== 0) {
                    moveX *= 0.707;
                    moveY *= 0.707;
                }

                player.x += moveX * player.speed;
                player.y += moveY * player.speed;

                // Batasi player
                player.x = Math.max(player.w / 2, Math.min(W - player.w / 2, player.x));
                player.y = Math.max(player.h / 2, Math.min(H - player.h / 2, player.y));

                // Cooldown tembak
                if (player.cooldown > 0) player.cooldown--;

                // Tembak (keyboard space atau mouseFire)
                if ((keys.Space || mouseFire) && player.cooldown <= 0) {
                    bullets.push({
                        x: player.x,
                        y: player.y - player.h / 2 - 6,
                        w: 4,
                        h: 14,
                        speed: 8,
                        color: '#ffdd59',
                    });
                    // Tembakan ganda di level tinggi
                    if (level >= 3) {
                        bullets.push({
                            x: player.x - 10,
                            y: player.y - player.h / 2 - 2,
                            w: 3,
                            h: 10,
                            speed: 7.5,
                            color: '#ffa502',
                        });
                        bullets.push({
                            x: player.x + 10,
                            y: player.y - player.h / 2 - 2,
                            w: 3,
                            h: 10,
                            speed: 7.5,
                            color: '#ffa502',
                        });
                    }
                    player.cooldown = player.cooldownMax;
                }

                // Update peluru
                for (let i = bullets.length - 1; i >= 0; i--) {
                    bullets[i].y -= bullets[i].speed;
                    if (bullets[i].y < -20) {
                        bullets.splice(i, 1);
                    }
                }

                // Update musuh
                for (let i = enemies.length - 1; i >= 0; i--) {
                    const e = enemies[i];
                    e.x += e.speedX;
                    e.y += e.speedY;

                    // Pantul di tepi
                    if (e.x - e.w / 2 < 0 || e.x + e.w / 2 > W) {
                        e.speedX *= -1;
                        e.x = Math.max(e.w / 2, Math.min(W - e.w / 2, e.x));
                    }

                    // Musuh lewat bawah
                    if (e.y > H + 60) {
                        enemies.splice(i, 1);
                        lives--;
                        updateUI();
                        if (lives <= 0) {
                            gameState = 'gameover';
                            spawnExplosion(player.x, player.y, 2.5);
                        }
                        continue;
                    }

                    // Cek tabrakan dengan player
                    const dxP = e.x - player.x;
                    const dyP = e.y - player.y;
                    const distP = Math.sqrt(dxP * dxP + dyP * dyP);
                    const collisionDistP = (e.w / 2) + (player.w / 2) - 6;
                    if (distP < collisionDistP) {
                        enemies.splice(i, 1);
                        lives--;
                        spawnExplosion(player.x, player.y, 1.8);
                        updateUI();
                        if (lives <= 0) {
                            gameState = 'gameover';
                            spawnExplosion(player.x, player.y, 2.5);
                        }
                        continue;
                    }

                    // Cek tabrakan dengan peluru
                    let hit = false;
                    for (let j = bullets.length - 1; j >= 0; j--) {
                        const b = bullets[j];
                        const dxB = e.x - b.x;
                        const dyB = e.y - b.y;
                        const distB = Math.sqrt(dxB * dxB + dyB * dyB);
                        if (distB < (e.w / 2) + (b.w / 2) + 2) {
                            bullets.splice(j, 1);
                            e.hp--;
                            if (e.hp <= 0) {
                                hit = true;
                                break;
                            }
                        }
                    }
                    if (hit) {
                        spawnExplosion(e.x, e.y, 1.2);
                        enemies.splice(i, 1);
                        score += 10;
                        updateUI();
                        // Naik level setiap 150 poin
                        const newLevel = Math.floor(score / 150) + 1;
                        if (newLevel > level && newLevel <= 10) {
                            level = newLevel;
                            // Bonus nyawa saat naik level
                            lives = Math.min(lives + 1, 8);
                            updateUI();
                        }
                    }
                }

                // Update ledakan
                for (let i = explosions.length - 1; i >= 0; i--) {
                    explosions[i].life -= explosions[i].decay;
                    explosions[i].radius = explosions[i].maxRadius * (1 - explosions[i].life);
                    if (explosions[i].life <= 0) {
                        explosions.splice(i, 1);
                    }
                }

                updateUI();
            }

            function draw() {
                ctx.clearRect(0, 0, W, H);

                // Background gradient
                const bgGrad = ctx.createLinearGradient(0, 0, 0, H);
                bgGrad.addColorStop(0, '#0a0a2e');
                bgGrad.addColorStop(0.5, '#0d0d30');
                bgGrad.addColorStop(1, '#060618');
                ctx.fillStyle = bgGrad;
                ctx.fillRect(0, 0, W, H);

                // Bintang
                stars.forEach(s => {
                    const alpha = 0.4 + 0.6 * Math.abs(Math.sin(s.twinkle));
                    ctx.fillStyle = `rgba(255,255,255,${alpha})`;
                    ctx.beginPath();
                    ctx.arc(s.x, s.y, s.r, 0, Math.PI * 2);
                    ctx.fill();
                });

                // Grid subtle
                ctx.strokeStyle = 'rgba(255,255,255,0.015)';
                ctx.lineWidth = 1;
                const gridSize = 40;
                for (let gx = 0; gx < W; gx += gridSize) {
                    ctx.beginPath();
                    ctx.moveTo(gx, 0);
                    ctx.lineTo(gx, H);
                    ctx.stroke();
                }
                for (let gy = 0; gy < H; gy += gridSize) {
                    ctx.beginPath();
                    ctx.moveTo(0, gy);
                    ctx.lineTo(W, gy);
                    ctx.stroke();
                }

                // Peluru
                bullets.forEach(b => {
                    // Glow
                    const glowGrad = ctx.createRadialGradient(b.x, b.y, 1, b.x, b.y, 12);
                    glowGrad.addColorStop(0, b.color);
                    glowGrad.addColorStop(1, 'transparent');
                    ctx.fillStyle = glowGrad;
                    ctx.beginPath();
                    ctx.arc(b.x, b.y, 12, 0, Math.PI * 2);
                    ctx.fill();

                    // Inti
                    ctx.fillStyle = '#fff';
                    ctx.shadowColor = b.color;
                    ctx.shadowBlur = 8;
                    ctx.fillRect(b.x - b.w / 2, b.y - b.h / 2, b.w, b.h);
                    ctx.shadowBlur = 0;
                });

                // Musuh
                enemies.forEach(e => {
                    ctx.save();
                    ctx.translate(e.x, e.y);

                    // Glow
                    const glowGrad = ctx.createRadialGradient(0, 0, e.w * 0.3, 0, 0, e.w * 0.9);
                    glowGrad.addColorStop(0, e.color);
                    glowGrad.addColorStop(1, 'transparent');
                    ctx.fillStyle = glowGrad;
                    ctx.beginPath();
                    ctx.arc(0, 0, e.w * 0.9, 0, Math.PI * 2);
                    ctx.fill();

                    // Badan musuh (bentuk pesawat musuh / diamond agresif)
                    ctx.fillStyle = e.color;
                    ctx.shadowColor = e.color;
                    ctx.shadowBlur = 10;
                    ctx.beginPath();
                    ctx.moveTo(0, -e.h / 2); // atas
                    ctx.lineTo(e.w / 2, e.h / 4); // kanan
                    ctx.lineTo(e.w / 4, e.h / 2); // kanan bawah
                    ctx.lineTo(-e.w / 4, e.h / 2); // kiri bawah
                    ctx.lineTo(-e.w / 2, e.h / 4); // kiri
                    ctx.closePath();
                    ctx.fill();
                    ctx.shadowBlur = 0;

                    // Mata
                    ctx.fillStyle = '#fff';
                    ctx.beginPath();
                    ctx.arc(-e.w * 0.15, -e.h * 0.1, e.w * 0.15, 0, Math.PI * 2);
                    ctx.fill();
                    ctx.beginPath();
                    ctx.arc(e.w * 0.15, -e.h * 0.1, e.w * 0.15, 0, Math.PI * 2);
                    ctx.fill();
                    ctx.fillStyle = '#000';
                    ctx.beginPath();
                    ctx.arc(-e.w * 0.12, -e.h * 0.08, e.w * 0.07, 0, Math.PI * 2);
                    ctx.fill();
                    ctx.beginPath();
                    ctx.arc(e.w * 0.18, -e.h * 0.08, e.w * 0.07, 0, Math.PI * 2);
                    ctx.fill();

                    // Jika hp > 1, tampilkan indikator
                    if (e.maxHp > 1 && e.hp < e.maxHp) {
                        ctx.fillStyle = 'rgba(255,255,255,0.7)';
                        ctx.fillRect(-e.w / 2, -e.h / 2 - 8, e.w * (e.hp / e.maxHp), 4);
                    }

                    ctx.restore();
                });

                // Player
                ctx.save();
                ctx.translate(player.x, player.y);

                // Glow player
                const playerGlow = ctx.createRadialGradient(0, 0, 10, 0, 0, 45);
                playerGlow.addColorStop(0, 'rgba(55,66,250,0.6)');
                playerGlow.addColorStop(1, 'transparent');
                ctx.fillStyle = playerGlow;
                ctx.beginPath();
                ctx.arc(0, 0, 45, 0, Math.PI * 2);
                ctx.fill();

                // Badan pesawat
                ctx.fillStyle = '#3742fa';
                ctx.shadowColor = '#3742fa';
                ctx.shadowBlur = 16;
                ctx.beginPath();
                ctx.moveTo(0, -player.h / 2); // hidung
                ctx.lineTo(player.w / 2, player.h / 3); // sayap kanan
                ctx.lineTo(player.w / 5, player.h / 5); // lekuk kanan
                ctx.lineTo(player.w / 6, player.h / 2); // ekor kanan
                ctx.lineTo(-player.w / 6, player.h / 2); // ekor kiri
                ctx.lineTo(-player.w / 5, player.h / 5); // lekuk kiri
                ctx.lineTo(-player.w / 2, player.h / 3); // sayap kiri
                ctx.closePath();
                ctx.fill();
                ctx.shadowBlur = 0;

                // Aksen putih
                ctx.fillStyle = '#fff';
                ctx.beginPath();
                ctx.arc(0, -player.h / 2 + 4, 5, 0, Math.PI * 2);
                ctx.fill();

                // Kokpit
                ctx.fillStyle = '#7bed9f';
                ctx.shadowColor = '#7bed9f';
                ctx.shadowBlur = 8;
                ctx.beginPath();
                ctx.ellipse(0, -4, 8, 14, 0, 0, Math.PI * 2);
                ctx.fill();
                ctx.shadowBlur = 0;

                // Sayap aksen
                ctx.fillStyle = '#5352ed';
                ctx.beginPath();
                ctx.moveTo(-player.w / 2 + 4, player.h / 3 - 2);
                ctx.lineTo(-player.w / 6, player.h / 5);
                ctx.lineTo(-player.w / 6, player.h / 2 - 4);
                ctx.closePath();
                ctx.fill();
                ctx.beginPath();
                ctx.moveTo(player.w / 2 - 4, player.h / 3 - 2);
                ctx.lineTo(player.w / 6, player.h / 5);
                ctx.lineTo(player.w / 6, player.h / 2 - 4);
                ctx.closePath();
                ctx.fill();

                // Api mesin (efek)
                const flameFlicker = Math.sin(frameCount * 0.5) * 3;
                const flameGrad = ctx.createLinearGradient(0, player.h / 2, 0, player.h / 2 + 14);
                flameGrad.addColorStop(0, '#ffa502');
                flameGrad.addColorStop(0.5, '#ff6348');
                flameGrad.addColorStop(1, 'transparent');
                ctx.fillStyle = flameGrad;
                ctx.beginPath();
                ctx.moveTo(-6, player.h / 2);
                ctx.lineTo(0, player.h / 2 + 10 + flameFlicker);
                ctx.lineTo(6, player.h / 2);
                ctx.closePath();
                ctx.fill();

                ctx.restore();

                // Ledakan
                explosions.forEach(exp => {
                    const alpha = exp.life;
                    const r = exp.radius;
                    const grad = ctx.createRadialGradient(exp.x, exp.y, r * 0.1, exp.x, exp.y, r);
                    grad.addColorStop(0, `rgba(255,255,255,${alpha})`);
                    grad.addColorStop(0.3, `rgba(255,165,2,${alpha * 0.9})`);
                    grad.addColorStop(0.7, `rgba(255,71,87,${alpha * 0.5})`);
                    grad.addColorStop(1, `rgba(255,71,87,0)`);
                    ctx.fillStyle = grad;
                    ctx.beginPath();
                    ctx.arc(exp.x, exp.y, r, 0, Math.PI * 2);
                    ctx.fill();

                    // Partikel
                    for (let p = 0; p < 6; p++) {
                        const angle = (p / 6) * Math.PI * 2 + exp.life * 3;
                        const dist = r * 1.2;
                        const px = exp.x + Math.cos(angle) * dist;
                        const py = exp.y + Math.sin(angle) * dist;
                        ctx.fillStyle = `rgba(255,200,50,${alpha * 0.6})`;
                        ctx.beginPath();
                        ctx.arc(px, py, 2 + Math.random() * 2, 0, Math.PI * 2);
                        ctx.fill();
                    }
                });

                // Overlay state
                if (gameState === 'idle') {
                    ctx.fillStyle = 'rgba(0,0,0,0.7)';
                    ctx.fillRect(0, 0, W, H);
                    ctx.fillStyle = '#fff';
                    ctx.font = 'bold 28px "Segoe UI", Arial, sans-serif';
                    ctx.textAlign = 'center';
                    ctx.fillText('✈️ PESAWAT TEMPUR', W / 2, H / 2 - 20);
                    ctx.font = '16px "Segoe UI", Arial, sans-serif';
                    ctx.fillStyle = '#ddd';
                    ctx.fillText('Klik "▶ Mulai" untuk bermain!', W / 2, H / 2 + 25);
                    ctx.fillText('Gunakan tombol atau keyboard', W / 2, H / 2 + 50);
                } else if (gameState === 'paused') {
                    ctx.fillStyle = 'rgba(0,0,0,0.6)';
                    ctx.fillRect(0, 0, W, H);
                    ctx.fillStyle = '#ffa502';
                    ctx.font = 'bold 32px "Segoe UI", Arial, sans-serif';
                    ctx.textAlign = 'center';
                    ctx.fillText('⏸️ JEDA', W / 2, H / 2);
                    ctx.font = '14px "Segoe UI", Arial, sans-serif';
                    ctx.fillStyle = '#ddd';
                    ctx.fillText('Klik "⏯️ Jeda" untuk lanjutkan', W / 2, H / 2 + 35);
                } else if (gameState === 'gameover') {
                    ctx.fillStyle = 'rgba(0,0,0,0.75)';
                    ctx.fillRect(0, 0, W, H);
                    ctx.fillStyle = '#ff4757';
                    ctx.font = 'bold 34px "Segoe UI", Arial, sans-serif';
                    ctx.textAlign = 'center';
                    ctx.fillText('💥 GAME OVER', W / 2, H / 2 - 15);
                    ctx.fillStyle = '#fff';
                    ctx.font = '18px "Segoe UI", Arial, sans-serif';
                    ctx.fillText('Skor Akhir: ' + score, W / 2, H / 2 + 25);
                    ctx.fillText('Level: ' + level, W / 2, H / 2 + 50);
                    ctx.fillStyle = '#ddd';
                    ctx.font = '14px "Segoe UI", Arial, sans-serif';
                    ctx.fillText('Klik "🔄 Ulang" untuk coba lagi', W / 2, H / 2 + 78);
                }

                ctx.textAlign = 'start';
            }

            function gameLoop() {
                update();
                draw();
                requestAnimationFrame(gameLoop);
            }

            // ============ EVENT LISTENERS ============
            // Keyboard
            window.addEventListener('keydown', (e) => {
                if (e.code in keys) {
                    keys[e.code] = true;
                    e.preventDefault();
                }
                if (e.code === 'Space') {
                    e.preventDefault();
                }
                // Pause dengan P
                if (e.code === 'KeyP' && gameState === 'playing') {
                    gameState = 'paused';
                } else if (e.code === 'KeyP' && gameState === 'paused') {
                    gameState = 'playing';
                }
            });

            window.addEventListener('keyup', (e) => {
                if (e.code in keys) {
                    keys[e.code] = false;
                    e.preventDefault();
                }
            });

            // Canvas click/touch untuk fire (opsional)
            canvas.addEventListener('mousedown', (e) => {
                e.preventDefault();
                mouseFire = true;
                if (gameState === 'idle') {
                    startGame();
                }
            });
            canvas.addEventListener('mouseup', (e) => {
                e.preventDefault();
                mouseFire = false;
            });
            canvas.addEventListener('mouseleave', () => {
                mouseFire = false;
            });
            canvas.addEventListener('touchstart', (e) => {
                e.preventDefault();
                mouseFire = true;
                if (gameState === 'idle') {
                    startGame();
                }
            }, { passive: false });
            canvas.addEventListener('touchend', (e) => {
                e.preventDefault();
                mouseFire = false;
            });
            canvas.addEventListener('touchcancel', () => {
                mouseFire = false;
            });

            // ============ TOMBOL UI ============
            function startGame() {
                if (gameState === 'idle' || gameState === 'gameover') {
                    resetGame();
                    gameState = 'playing';
                } else if (gameState === 'paused') {
                    gameState = 'playing';
                }
            }

            document.getElementById('btnStart').addEventListener('click', () => {
                if (gameState === 'idle' || gameState === 'gameover') {
                    resetGame();
                }
                gameState = 'playing';
            });

            document.getElementById('btnPause').addEventListener('click', () => {
                if (gameState === 'playing') {
                    gameState = 'paused';
                } else if (gameState === 'paused') {
                    gameState = 'playing';
                }
            });

            document.getElementById('btnRestart').addEventListener('click', () => {
                resetGame();
                gameState = 'playing';
            });

            document.getElementById('btnFire').addEventListener('mousedown', (e) => {
                e.preventDefault();
                mouseFire = true;
                if (gameState === 'idle' || gameState === 'gameover') {
                    resetGame();
                    gameState = 'playing';
                }
            });
            document.getElementById('btnFire').addEventListener('mouseup', (e) => {
                e.preventDefault();
                mouseFire = false;
            });
            document.getElementById('btnFire').addEventListener('mouseleave', () => {
                mouseFire = false;
            });
            document.getElementById('btnFire').addEventListener('touchstart', (e) => {
                e.preventDefault();
                mouseFire = true;
                if (gameState === 'idle' || gameState === 'gameover') {
                    resetGame();
                    gameState = 'playing';
                }
            }, { passive: false });
            document.getElementById('btnFire').addEventListener('touchend', (e) => {
                e.preventDefault();
                mouseFire = false;
            });

            // D-Pad buttons
            function setupDpadButton(id, keyCode, isFireKey = false) {
                const btn = document.getElementById(id);
                if (!btn) return;

                const setKey = (val) => {
                    if (isFireKey) {
                        mouseFire = val;
                    } else {
                        keys[keyCode] = val;
                    }
                    if (val && (gameState === 'idle' || gameState === 'gameover')) {
                        resetGame();
                        gameState = 'playing';
                    }
                };

                btn.addEventListener('mousedown', (e) => {
                    e.preventDefault();
                    setKey(true);
                });
                btn.addEventListener('mouseup', (e) => {
                    e.preventDefault();
                    setKey(false);
                });
                btn.addEventListener('mouseleave', () => {
                    setKey(false);
                });
                btn.addEventListener('touchstart', (e) => {
                    e.preventDefault();
                    setKey(true);
                }, { passive: false });
                btn.addEventListener('touchend', (e) => {
                    e.preventDefault();
                    setKey(false);
                });
                btn.addEventListener('touchcancel', () => {
                    setKey(false);
                });
            }

            setupDpadButton('btnUp', 'ArrowUp');
            setupDpadButton('btnDown', 'ArrowDown');
            setupDpadButton('btnLeft', 'ArrowLeft');
            setupDpadButton('btnRight', 'ArrowRight');

            // ============ MULAI GAME LOOP ============
            updateUI();
            gameLoop();

            console.log('🚀 Game Pesawat Tempur siap!');
            console.log('   ▶ Klik "Mulai" atau tekan spasi/tombol tembak untuk mulai');
            console.log('   ⌨️  Keyboard: Panah / WASD = gerak, Spasi = tembak');
            console.log('   📱 Mobile: Gunakan D-Pad dan tombol TEMBAK');
            console.log('   🎯 Tembak musuh, kumpulkan skor, naik level!');
            console.log('   ❤️  Hati-hati, hindari tabrakan dengan musuh!');
        })();
    </script>
</body>
</html>

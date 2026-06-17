<!DOCTYPE html>
<html lang="pt-PT">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Block Rush - Neon Edition</title>
    <!-- Fonte futurista estilo arcade -->
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&display=swap" rel="stylesheet">
    <style>
        :root {
            --neon-blue: #00f3ff;
            --neon-pink: #ff007f;
            --neon-green: #39ff14;
            --neon-yellow: #ffeb3b;
            --neon-red: #ff3131;
            --neon-purple: #b026ff;
            --neon-orange: #ff9d00;
        }

        body {
            margin: 0;
            background-color: #06060c;
            color: #fff;
            font-family: 'Orbitron', sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            overflow: hidden;
            background-image: 
                radial-gradient(circle at 50% 50%, #120d2b 0%, #05050a 100%),
                linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.25) 50%), 
                linear-gradient(90deg, rgba(255, 0, 0, 0.06), rgba(0, 255, 0, 0.02), rgba(0, 0, 255, 0.06));
            background-size: 100% 100%, 100% 4px, 6px 100%;
        }

        /* Efeito de cintilação neon no título principal */
        h1 {
            font-size: 42px;
            font-weight: 900;
            margin: 10px 0;
            letter-spacing: 5px;
            text-align: center;
            color: #fff;
            text-shadow: 
                0 0 5px #fff,
                0 0 10px var(--neon-blue),
                0 0 20px var(--neon-blue),
                0 0 40px var(--neon-blue);
            animation: flicker 3s infinite alternate;
        }

        #menu {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 20px;
            background: rgba(10, 10, 20, 0.85);
            padding: 40px;
            border: 3px solid var(--neon-blue);
            border-radius: 16px;
            box-shadow: 
                0 0 15px rgba(0, 243, 255, 0.3),
                inset 0 0 15px rgba(0, 243, 255, 0.2);
            backdrop-filter: blur(8px);
            width: 380px;
        }

        #menu h2 {
            margin: 0 0 10px 0;
            font-size: 18px;
            letter-spacing: 2px;
            color: #e0e0ff;
            text-align: center;
            text-shadow: 0 0 8px rgba(255, 255, 255, 0.5);
        }

        /* Botões de neon coloridos do menu */
        .btn-menu {
            width: 100%;
            padding: 15px;
            font-size: 18px;
            font-weight: 700;
            letter-spacing: 2px;
            font-family: inherit;
            cursor: pointer;
            background: rgba(0, 0, 0, 0.6);
            border-radius: 8px;
            transition: all 0.3s ease;
        }

        /* Botão Fácil (Verde) */
        .btn-facil {
            border: 2px solid var(--neon-green);
            color: var(--neon-green);
            text-shadow: 0 0 5px var(--neon-green);
        }
        .btn-facil:hover {
            background: var(--neon-green);
            color: #000;
            box-shadow: 0 0 20px var(--neon-green);
            text-shadow: none;
        }

        /* Botão Médio (Amarelo) */
        .btn-medio {
            border: 2px solid var(--neon-yellow);
            color: var(--neon-yellow);
            text-shadow: 0 0 5px var(--neon-yellow);
        }
        .btn-medio:hover {
            background: var(--neon-yellow);
            color: #000;
            box-shadow: 0 0 20px var(--neon-yellow);
            text-shadow: none;
        }

        /* Botão Difícil (Vermelho) */
        .btn-dificil {
            border: 2px solid var(--neon-red);
            color: var(--neon-red);
            text-shadow: 0 0 5px var(--neon-red);
        }
        .btn-dificil:hover {
            background: var(--neon-red);
            color: #fff;
            box-shadow: 0 0 20px var(--neon-red);
            text-shadow: none;
        }

        #game-container {
            position: relative;
        }

        canvas {
            border: 3px solid var(--neon-blue);
            background-color: #030307;
            display: none;
            border-radius: 12px;
            box-shadow: 
                0 0 30px rgba(0, 243, 255, 0.15),
                inset 0 0 20px rgba(0, 0, 0, 0.9);
        }

        /* Ecrã de fim de jogo estilizado */
        #game-over-screen {
            display: none;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(5, 5, 10, 0.95);
            border: 3px solid var(--neon-pink);
            padding: 40px;
            text-align: center;
            border-radius: 16px;
            box-shadow: 0 0 40px var(--neon-pink);
            backdrop-filter: blur(10px);
            width: 320px;
        }

        #game-over-screen h2 {
            color: var(--neon-pink);
            font-size: 32px;
            margin: 0 0 15px 0;
            text-shadow: 0 0 15px var(--neon-pink);
            letter-spacing: 3px;
        }

        #final-score {
            font-size: 16px;
            line-height: 1.6;
            margin-bottom: 25px;
            color: #e0e0ff;
        }

        .btn-restart {
            border: 2px solid var(--neon-blue);
            color: var(--neon-blue);
            background: transparent;
            padding: 12px 24px;
            font-size: 16px;
            font-weight: 700;
            letter-spacing: 1px;
            font-family: inherit;
            cursor: pointer;
            border-radius: 6px;
            transition: all 0.3s;
            text-shadow: 0 0 5px var(--neon-blue);
        }

        .btn-restart:hover {
            background: var(--neon-blue);
            color: #000;
            box-shadow: 0 0 15px var(--neon-blue);
            text-shadow: none;
        }

        /* Animação do neon a piscar */
        @keyframes flicker {
            0%, 18%, 22%, 25%, 53%, 57%, 100% {
                text-shadow: 
                    0 0 4px #fff,
                    0 0 10px var(--neon-blue),
                    0 0 20px var(--neon-blue),
                    0 0 35px var(--neon-blue);
            }
            20%, 24%, 55% {        
                text-shadow: none;
            }
        }

        /* Efeito de abalo do ecrã ao sofrer dano grave */
        .shake {
            animation: screenShake 0.4s ease-in-out;
        }

        @keyframes screenShake {
            0%, 100% { transform: translate(0, 0); }
            10%, 90% { transform: translate(-5px, 3px); }
            20%, 80% { transform: translate(6px, -4px); }
            30%, 50%, 70% { transform: translate(-8px, 6px); }
            40%, 62% { transform: translate(8px, -6px); }
        }
    </style>
</head>
<body>

    <h1 id="mainTitle">BLOCK RUSH</h1>

    <!-- MENU COLORIDO COM NEON -->
    <div id="menu">
        <h2>ESCOLHE A INTENSIDADE:</h2>
        <button class="btn-menu btn-facil" onclick="startGame('Facil')">FÁCIL</button>
        <button class="btn-menu btn-medio" onclick="startGame('Medio')">MÉDIO</button>
        <button class="btn-menu btn-dificil" onclick="startGame('Dificil')">DIFÍCIL</button>
    </div>

    <!-- ÁREA DO JOGO -->
    <div id="game-container">
        <canvas id="gameCanvas" width="800" height="600"></canvas>
        
        <!-- FIM DE JOGO -->
        <div id="game-over-screen">
            <h2>FIM DO JOGO</h2>
            <p id="final-score"></p>
            <button class="btn-restart" onclick="resetToMenu()">MENU PRINCIPAL</button>
        </div>
    </div>

    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const menu = document.getElementById('menu');
        const gameOverScreen = document.getElementById('game-over-screen');
        const finalScoreText = document.getElementById('final-score');
        const gameContainer = document.getElementById('game-container');
        const mainTitle = document.getElementById('mainTitle');

        // Configurações e Estado
        let score, lives, difficulty, gameOver, gameInterval;
        let keys = {};
        
        // Cores neon puras para o jogador (mudam a cada 40 pontos)
        const playerColors = ['#00f3ff', '#ff007f', '#b026ff', '#39ff14', '#ffeb3b', '#ff4e00'];

        let player, enemies, coins, powerUps, particles, trails;

        // ENTIDADES E ESTADOS DO CHEFE (DRAGÃO DE TRÊS CABEÇAS)
        let dragon = null;
        let tnts = [];
        let fireballs = [];
        let carryingTnt = false;
        let thrownTnts = [];
        let tntSpawnTimer = 0;
        let dragonDefeated = false; // Bloqueador para evitar o spawn infinito do chefe

        // Sintetizador de efeitos sonoros de 8-bit (Web Audio API)
        let audioCtx;
        let bossMusicInterval = null; // Guardará o loop de sequenciação de som do boss
        let bossMusicStep = 0;        // Passo do sequenciador da trilha de ação

        function getAudioContext() {
            if (!audioCtx) {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            }
            if (audioCtx.state === 'suspended') {
                audioCtx.resume();
            }
            return audioCtx;
        }

        // Toca uma nota rápida de baixo sintetizada para a trilha sonora de ação
        function playActionBassNote(frequency, duration = 0.15) {
            try {
                let context = getAudioContext();
                let osc = context.createOscillator();
                let gainNode = context.createGain();
                
                osc.connect(gainNode);
                gainNode.connect(context.destination);
                
                // Tipo dente de serra para soar mais enérgico/techno
                osc.type = 'sawtooth';
                osc.frequency.setValueAtTime(frequency, context.currentTime);
                
                // Filtro passa-baixo simples para deixar o som mais encorpado e menos estridente
                let filter = context.createBiquadFilter();
                filter.type = 'lowpass';
                filter.frequency.setValueAtTime(350, context.currentTime);
                
                osc.disconnect(gainNode);
                osc.connect(filter);
                filter.connect(gainNode);

                gainNode.gain.setValueAtTime(0.04, context.currentTime);
                gainNode.gain.exponentialRampToValueAtTime(0.001, context.currentTime + duration);
                
                osc.start();
                osc.stop(context.currentTime + duration);
            } catch (e) {
                // Silencia se houver falhas no contexto de áudio
            }
        }

        // Inicia o loop de sequenciação de música de ação do Boss
        function startBossMusic() {
            stopBossMusic();
            bossMusicStep = 0;
            
            // Notas de baixo rápidas e agressivas (linha de baixo clássica de boss retro)
            const bassSequence = [110, 110, 130.81, 110, 146.83, 110, 130.81, 82.41];
            
            bossMusicInterval = setInterval(() => {
                if (gameOver || !dragon || !dragon.active) {
                    stopBossMusic();
                    return;
                }
                
                let currentNote = bassSequence[bossMusicStep % bassSequence.length];
                // Se o dragão estiver em fúria, as oitavas dobram para aumentar a tensão
                if (dragon.enraged) {
                    currentNote = currentNote * 1.5;
                }
                
                playActionBassNote(currentNote, 0.18);
                bossMusicStep++;
            }, 180); // Ritmo frenético
        }

        function stopBossMusic() {
            if (bossMusicInterval) {
                clearInterval(bossMusicInterval);
                bossMusicInterval = null;
            }
        }

        function playSound(type) {
            try {
                let context = getAudioContext();
                let osc = context.createOscillator();
                let gainNode = context.createGain();
                osc.connect(gainNode);
                gainNode.connect(context.destination);

                if (type === 'coin') {
                    osc.type = 'sine';
                    osc.frequency.setValueAtTime(523.25, context.currentTime); // C5
                    osc.frequency.exponentialRampToValueAtTime(880, context.currentTime + 0.08); // A5
                    gainNode.gain.setValueAtTime(0.08, context.currentTime);
                    gainNode.gain.exponentialRampToValueAtTime(0.01, context.currentTime + 0.12);
                    osc.start();
                    osc.stop(context.currentTime + 0.12);
                } else if (type === 'hit') {
                    osc.type = 'sawtooth';
                    osc.frequency.setValueAtTime(180, context.currentTime);
                    osc.frequency.linearRampToValueAtTime(40, context.currentTime + 0.25);
                    gainNode.gain.setValueAtTime(0.15, context.currentTime);
                    gainNode.gain.exponentialRampToValueAtTime(0.01, context.currentTime + 0.25);
                    osc.start();
                    osc.stop(context.currentTime + 0.25);
                } else if (type === 'powerup') {
                    osc.type = 'triangle';
                    osc.frequency.setValueAtTime(300, context.currentTime);
                    osc.frequency.exponentialRampToValueAtTime(1500, context.currentTime + 0.4);
                    gainNode.gain.setValueAtTime(0.12, context.currentTime);
                    gainNode.gain.exponentialRampToValueAtTime(0.01, context.currentTime + 0.4);
                    osc.start();
                    osc.stop(context.currentTime + 0.4);
                } else if (type === 'gameover') {
                    osc.type = 'sawtooth';
                    osc.frequency.setValueAtTime(220, context.currentTime);
                    osc.frequency.linearRampToValueAtTime(55, context.currentTime + 0.6);
                    gainNode.gain.setValueAtTime(0.15, context.currentTime);
                    gainNode.gain.exponentialRampToValueAtTime(0.01, context.currentTime + 0.6);
                    osc.start();
                    osc.stop(context.currentTime + 0.6);
                } else if (type === 'tnt_grab') {
                    osc.type = 'square';
                    osc.frequency.setValueAtTime(440, context.currentTime);
                    osc.frequency.setValueAtTime(660, context.currentTime + 0.05);
                    gainNode.gain.setValueAtTime(0.05, context.currentTime);
                    gainNode.gain.exponentialRampToValueAtTime(0.01, context.currentTime + 0.1);
                    osc.start();
                    osc.stop(context.currentTime + 0.1);
                } else if (type === 'tnt_throw') {
                    osc.type = 'triangle';
                    osc.frequency.setValueAtTime(200, context.currentTime);
                    osc.frequency.exponentialRampToValueAtTime(800, context.currentTime + 0.2);
                    gainNode.gain.setValueAtTime(0.1, context.currentTime);
                    gainNode.gain.exponentialRampToValueAtTime(0.01, context.currentTime + 0.2);
                    osc.start();
                    osc.stop(context.currentTime + 0.2);
                } else if (type === 'dragon_damage') {
                    osc.type = 'sawtooth';
                    osc.frequency.setValueAtTime(120, context.currentTime);
                    osc.frequency.linearRampToValueAtTime(30, context.currentTime + 0.4);
                    gainNode.gain.setValueAtTime(0.25, context.currentTime);
                    gainNode.gain.exponentialRampToValueAtTime(0.01, context.currentTime + 0.4);
                    osc.start();
                    osc.stop(context.currentTime + 0.4);
                } else if (type === 'fireball') {
                    osc.type = 'sine';
                    osc.frequency.setValueAtTime(600, context.currentTime);
                    osc.frequency.exponentialRampToValueAtTime(150, context.currentTime + 0.15);
                    gainNode.gain.setValueAtTime(0.06, context.currentTime);
                    gainNode.gain.exponentialRampToValueAtTime(0.01, context.currentTime + 0.15);
                    osc.start();
                    osc.stop(context.currentTime + 0.15);
                } else if (type === 'roar') {
                    osc.type = 'sawtooth';
                    osc.frequency.setValueAtTime(90, context.currentTime);
                    osc.frequency.linearRampToValueAtTime(45, context.currentTime + 0.8);
                    gainNode.gain.setValueAtTime(0.3, context.currentTime);
                    gainNode.gain.exponentialRampToValueAtTime(0.01, context.currentTime + 0.8);
                    osc.start();
                    osc.stop(context.currentTime + 0.8);
                } else if (type === 'falling') {
                    // Som de queda: Tom retro contínuo que desce exponencialmente de agudo para grave
                    osc.type = 'sawtooth';
                    osc.frequency.setValueAtTime(300, context.currentTime);
                    osc.frequency.exponentialRampToValueAtTime(25, context.currentTime + 2.5); // Desce em 2.5 segundos
                    gainNode.gain.setValueAtTime(0.2, context.currentTime);
                    gainNode.gain.linearRampToValueAtTime(0.001, context.currentTime + 2.5);
                    osc.start();
                    osc.stop(context.currentTime + 2.5);
                }
            } catch(e) {
                console.log("Audio API impedida.");
            }
        }

        // Eventos de teclas
        window.addEventListener('keydown', e => {
            keys[e.key] = true;
            // Se o jogador carregar no "E" e o Boss estiver ativo e vivo
            if ((e.key === 'e' || e.key === 'E') && dragon && dragon.active && !dragon.dying) {
                handleTntAction();
            }
        });
        window.addEventListener('keyup', e => keys[e.key] = false);

        function startGame(selectedDifficulty) {
            getAudioContext(); // Desbloqueia áudio se necessário
            difficulty = selectedDifficulty;
            menu.style.display = 'none';
            canvas.style.display = 'block';
            gameOverScreen.style.display = 'none';
            
            score = 0;
            lives = 3;
            gameOver = false;
            enemies = [];
            coins = [];
            powerUps = [];
            particles = [];
            trails = [];

            // Reset do estado do Chefe
            dragon = null;
            tnts = [];
            fireballs = [];
            carryingTnt = false;
            thrownTnts = [];
            tntSpawnTimer = 0;
            dragonDefeated = false; // Permite que o chefe spawne uma vez nesta sessão
            stopBossMusic();

            player = {
                x: canvas.width / 2 - 15,
                y: canvas.height / 2 - 15,
                size: 28,
                speed: 5.5,
                colorIndex: 0,
                invincible: false,
                invincibleTimer: 0,
                hue: 0,
                lightPulse: 0
            };

            // Ajusta o brilho do rebordo do jogo para corresponder à dificuldade
            if (difficulty === 'Facil') canvas.style.borderColor = 'var(--neon-green)';
            if (difficulty === 'Medio') canvas.style.borderColor = 'var(--neon-yellow)';
            if (difficulty === 'Dificil') canvas.style.borderColor = 'var(--neon-red)';

            spawnCoin();
            
            // Velocidade ajustada à dificuldade
            let spawnRate = 1400;
            if (difficulty === 'Medio') spawnRate = 900;
            if (difficulty === 'Dificil') spawnRate = 500;

            // Loops
            gameInterval = setInterval(update, 1000 / 60);
            this.enemySpawner = setInterval(spawnEnemy, spawnRate);
            this.powerUpSpawner = setInterval(spawnPowerUp, 18000); // Power-up surge a cada 18s
        }

        function spawnEnemy() {
            if (gameOver) return;
            // Bloqueia spawns de inimigos normais se o dragão estiver ativo, EXCETO se o dragão estiver em modo Fúria (Rage)
            if (dragon && dragon.active && !dragon.enraged) return;
            // Não spawna se o dragão estiver a morrer
            if (dragon && dragon.dying) return;

            let size = Math.random() * 15 + 20; // tamanho inimigo
            let x, y, vx, vy;

            // Surge fora do ecrã
            if (Math.random() < 0.5) {
                x = Math.random() < 0.5 ? -size : canvas.width + size;
                y = Math.random() * canvas.height;
            } else {
                x = Math.random() * canvas.width;
                y = Math.random() < 0.5 ? -size : canvas.height + size;
            }

            // Velocidade ajustada à dificuldade
            let baseSpeed = difficulty === 'Facil' ? 2 : difficulty === 'Medio' ? 4.5 : 7;
            
            // Durante o Boss, diminui um pouco a velocidade dos blocos normais para não ser injusto
            if (dragon && dragon.active) {
                baseSpeed = baseSpeed * 0.7;
            }

            let angle = Math.atan2(player.y - y, player.x - x);
            vx = Math.cos(angle) * (baseSpeed + Math.random() * 1.5);
            vy = Math.sin(angle) * (baseSpeed + Math.random() * 1.5);

            // Cores vibrantes para os inimigos de neon
            let colors = [
                '#ff0055', // Rosa Choque
                '#ff5500', // Laranja Fogo
                '#00ffcc', // Ciano
                '#ff00ff', // Magenta
                '#cc00ff'  // Roxo Elétrico
            ];
            let color = colors[Math.floor(Math.random() * colors.length)];

            enemies.push({ x, y, size, vx, vy, color });
        }

        function spawnCoin() {
            // Não spawna moedas se estiver em batalha de boss para manter foco tático
            if (dragon && dragon.active) return;

            coins.push({
                x: Math.random() * (canvas.width - 60) + 30,
                y: Math.random() * (canvas.height - 60) + 30,
                size: 14,
                glowDir: 1,
                glowSize: 0
            });
        }

        function spawnPowerUp() {
            if (gameOver || player.invincible) return;
            if (dragon && dragon.active) return; // Desativa boosts na luta contra o boss

            powerUps.push({
                x: Math.random() * (canvas.width - 60) + 30,
                y: Math.random() * (canvas.height - 60) + 30,
                size: 14
            });
        }

        // Inicializa o Boss: Dragão Pixelado de Três Cabeças em qualquer dificuldade
        function initDragonBoss() {
            playSound('roar');
            
            // Configurações baseadas na dificuldade
            let maxHpValue = (difficulty === 'Facil') ? 50 : 100; // 50 HP (Fácil) ou 100 HP (Médio/Difícil)
            let damageValue = (difficulty === 'Facil') ? 5 : 10;   // Cada TNT tira 5 (Fácil) ou 10 (Médio/Difícil)

            dragon = {
                active: true,
                dying: false,       // Estado de colapso/queda lenta
                x: canvas.width / 2,
                y: -180, // Começa fora do ecrã e desce dramaticamente
                targetY: 100,
                hoverY: 0, // Posicionamento senoidal vertical
                width: 180,
                height: 90,
                hp: maxHpValue,
                maxHp: maxHpValue,
                tntDamage: damageValue,
                speedX: difficulty === 'Facil' ? 1.5 : (difficulty === 'Medio' ? 2.5 : 3.5),
                dirX: 1,
                shootCooldown: 0,
                flapAngle: 0,
                defeated: false,
                enraged: false // Estado de fúria (vida <= 50%)
            };
            
            // Limpa inimigos comuns do ecrã para a luta épica
            enemies = [];
            coins = [];
            powerUps = [];
            tnts = [];
            thrownTnts = [];
            carryingTnt = false;
            
            // Ajudar o jogador restaurando e aumentando as suas vidas para 5 corações ao enfrentar o chefe!
            lives = 5;
            
            // Spawna a primeira TNT logo na entrada do chefe
            tntSpawnTimer = 60; // 1 segundo de espera inicial

            // Inicia a música de ação retro do boss!
            startBossMusic();
        }

        // Lida com a tecla "E" para apanhar ou lançar TNT
        function handleTntAction() {
            if (!dragon || !dragon.active || dragon.defeated || dragon.dying) return;

            if (carryingTnt) {
                // Lança a TNT em linha reta para cima
                thrownTnts.push({
                    x: player.x + player.size / 2 - 10,
                    y: player.y - 15,
                    size: 20,
                    vy: -8
                });
                carryingTnt = false;
                playSound('tnt_throw');
                
                // Regra: Uma nova TNT só aparece exatamente 2 segundos (120 frames) após o arremesso!
                tntSpawnTimer = 120;
            } else {
                // Procura uma TNT no chão próxima ao jogador para apanhar
                for (let i = 0; i < tnts.length; i++) {
                    let tnt = tnts[i];
                    let dist = Math.hypot((player.x + player.size/2) - tnt.x, (player.y + player.size/2) - tnt.y);
                    if (dist < 45) { // Distância de proximidade para apanhar
                        carryingTnt = true;
                        tnts.splice(i, 1);
                        playSound('tnt_grab');
                        break;
                    }
                }
            }
        }

        // Sistema de partículas dinâmicas emissoras de luz
        function createExplosion(x, y, color, particleCount = 15, force = 6) {
            for (let i = 0; i < particleCount; i++) {
                particles.push({
                    x: x,
                    y: y,
                    vx: (Math.random() - 0.5) * force,
                    vy: (Math.random() - 0.5) * force,
                    size: Math.random() * 4 + 2,
                    alpha: 1,
                    decay: Math.random() * 0.02 + 0.015,
                    color: color
                });
            }
        }

        // Adiciona um abalo físico ao ecrã ao sofrer dano ou impactos
        function triggerScreenShake(customDuration = 400) {
            gameContainer.classList.add('shake');
            setTimeout(() => {
                gameContainer.classList.remove('shake');
            }, customDuration);
        }

        function update() {
            if (gameOver) return;

            // Ativação do Boss se o jogador atingir 100 pontos em QUALQUER dificuldade e ainda não o tiver derrotado
            if (score >= 100 && !dragon && !dragonDefeated) {
                initDragonBoss();
            }

            // Movimento fluido e preciso
            if (keys['ArrowUp'] || keys['w'] || keys['W']) player.y -= player.speed;
            if (keys['ArrowDown'] || keys['s'] || keys['S']) player.y += player.speed;
            if (keys['ArrowLeft'] || keys['a'] || keys['A']) player.x -= player.speed;
            if (keys['ArrowRight'] || keys['d'] || keys['D']) player.x += player.speed;

            // Limitar aos limites do ecrã
            if (player.x < 0) player.x = 0;
            if (player.x > canvas.width - player.size) player.x = canvas.width - player.size;
            if (player.y < 0) player.y = 0;
            if (player.y > canvas.height - player.size) player.y = canvas.height - player.size;

            // Pulsação de luz interna periódica
            player.lightPulse = Math.sin(Date.now() * 0.01) * 8 + 12;

            // Lógica de invencibilidade e arco-íris com rastro de luz neon
            if (player.invincible) {
                player.invincibleTimer--;
                player.hue = (player.hue + 4) % 360; 
                
                // Criação do rastro luminoso contínuo
                trails.push({
                    x: player.x,
                    y: player.y,
                    size: player.size,
                    color: `hsla(${player.hue}, 100%, 55%, 0.55)`,
                    alpha: 0.65
                });

                if (player.invincibleTimer <= 0) {
                    player.invincible = false;
                    canvas.style.borderColor = difficulty === 'Facil' ? 'var(--neon-green)' : difficulty === 'Medio' ? 'var(--neon-yellow)' : 'var(--neon-red)';
                }
            } else {
                // Rastro padrão com a cor atual em opacidade reduzida
                if (Math.random() < 0.3) {
                    trails.push({
                        x: player.x,
                        y: player.y,
                        size: player.size,
                        color: playerColors[player.colorIndex],
                        alpha: 0.25
                    });
                }
            }

            // Atualização de rastro
            trails.forEach((t, i) => {
                t.alpha -= 0.025;
                if (t.alpha <= 0) {
                    trails.splice(i, 1);
                }
            });

            // Atualiza a cor do jogador a cada 40 pontos
            player.colorIndex = Math.floor(score / 40) % playerColors.length;

            // LÓGICA DO BOSS DRAGÃO DE TRÊS CABEÇAS
            if (dragon && dragon.active) {
                
                // ----------------------------------------
                // ESTADO 1: O DRAGÃO ESTÁ ATIVO (VIVO E A COMBATER)
                // ----------------------------------------
                if (!dragon.dying) {
                    // Detetar e ativar Modo de Fúria (Rage) caso vida esteja abaixo de 50%
                    if (dragon.hp <= dragon.maxHp * 0.5 && !dragon.enraged) {
                        dragon.enraged = true;
                        playSound('roar'); // Rugido terrível de fúria!
                        triggerScreenShake(600); // Grande abalo inicial de fúria
                    }

                    // Descida inicial do dragão
                    if (dragon.y < dragon.targetY) {
                        dragon.y += 2;
                    } else {
                        // Animação de flutuação dinâmica senoidal (Subindo e descendo devagar)
                        let hoverSpeed = dragon.enraged ? 0.003 : 0.0015;
                        let hoverAmp = dragon.enraged ? 30 : 20;
                        dragon.hoverY = Math.sin(Date.now() * hoverSpeed) * hoverAmp;

                        // Movimento oscilatório horizontal clássico de Boss (mais rápido em fúria)
                        let currentSpeedX = dragon.enraged ? dragon.speedX * 1.5 : dragon.speedX;
                        dragon.x += currentSpeedX * dragon.dirX;
                        if (dragon.x - dragon.width/2 < 50 || dragon.x + dragon.width/2 > canvas.width - 50) {
                            dragon.dirX *= -1;
                        }
                        
                        // Disparo de bolas de fogo em direção ao jogador (mais rápido em fúria)
                        dragon.shootCooldown--;
                        if (dragon.shootCooldown <= 0) {
                            let headOffsetsX = [-50, 0, 50];
                            let activeHoverY = dragon.y + dragon.hoverY;
                            headOffsetsX.forEach(offset => {
                                let fX = dragon.x + offset;
                                let fY = activeHoverY + 20;
                                let angle = Math.atan2(player.y - fY, player.x - fX);
                                fireballs.push({
                                    x: fX,
                                    y: fY,
                                    vx: Math.cos(angle) * (difficulty === 'Facil' ? 3.5 : (difficulty === 'Medio' ? 4.5 : 5.5)),
                                    vy: Math.sin(angle) * (difficulty === 'Facil' ? 3.5 : (difficulty === 'Medio' ? 4.5 : 5.5)),
                                    size: 16
                                });
                            });
                            playSound('fireball');
                            
                            // Tempo de recarga reduzido se estiver furioso!
                            let baseCooldown = difficulty === 'Facil' ? 180 : (difficulty === 'Medio' ? 140 : 100);
                            dragon.shootCooldown = dragon.enraged ? baseCooldown * 0.7 : baseCooldown;
                        }
                    }

                    // Flapping das asas do dragão para animação procedural pixelada
                    dragon.flapAngle += dragon.enraged ? 0.15 : 0.08;

                    // Efeito visual de fumaça pixelada saindo do nariz (se furioso)
                    if (dragon.enraged && dragon.y >= dragon.targetY) {
                        let activeHoverY = dragon.y + dragon.hoverY;
                        let headOffsetsX = [-50, 0, 50];
                        headOffsetsX.forEach((offset, idx) => {
                            let hX = dragon.x + offset;
                            let hY = activeHoverY - 50 + (idx === 1 ? -15 : 0);
                            
                            if (Math.random() < 0.12) {
                                particles.push({
                                    x: hX + (Math.random() - 0.5) * 8,
                                    y: hY + 12,
                                    vx: (Math.random() - 0.5) * 1.2,
                                    vy: Math.random() * 2.5 + 1.2, // desce em direção ao ecrã
                                    size: Math.random() * 5 + 4,
                                    alpha: 0.7,
                                    decay: 0.015,
                                    color: '#555566' 
                                });
                            }
                        });
                    }

                    // Spawning automático de TNT no chão (Respeitando a regra de recarga de 2s)
                    if (tntSpawnTimer > 0) {
                        tntSpawnTimer--;
                    } else if (tnts.length === 0 && !carryingTnt && thrownTnts.length === 0) {
                        tnts.push({
                            x: Math.random() * (canvas.width - 120) + 60,
                            y: Math.random() * 120 + 420, 
                            size: 24
                        });
                    }
                } 
                // ----------------------------------------
                // ESTADO 2: O DRAGÃO FOI DERROTADO (A CAIR LENTAMENTE)
                // ----------------------------------------
                else {
                    // Ele desce suavemente pela gravidade em direção ao abismo
                    dragon.y += 1.2; // Velocidade de queda lenta e cinematográfica
                    
                    // O mapa treme de forma contínua durante a queda!
                    triggerScreenShake(50);
                    
                    // Vai soltando dezenas de explosões menores por onde passa
                    if (Math.random() < 0.25) {
                        let randomOffsetX = (Math.random() - 0.5) * 120;
                        let randomOffsetY = (Math.random() - 0.5) * 60;
                        createExplosion(dragon.x + randomOffsetX, dragon.y + dragon.hoverY + randomOffsetY, 'var(--neon-orange)', 8, 4);
                    }

                    // Condição para a explosão final definitiva ao passar do fundo da tela
                    if (dragon.y > canvas.height + 110) {
                        // 1. EXPLOSÃO ENORME INDIVIDUAL DAS TRÊS CABEÇAS!
                        let activeHoverY = dragon.y + dragon.hoverY;
                        let headOffsetsX = [-50, 0, 50];
                        headOffsetsX.forEach((offset, hIdx) => {
                            let hX = dragon.x + offset;
                            let hY = activeHoverY - 50 + (hIdx === 1 ? -15 : 0);
                            createExplosion(hX, hY, 'var(--neon-red)', 40, 14);
                            createExplosion(hX, hY, '#ffffff', 25, 9);
                        });

                        // 2. Explosões massivas centrais adicionais
                        createExplosion(dragon.x, activeHoverY, 'var(--neon-yellow)', 70, 16);
                        createExplosion(dragon.x, activeHoverY, 'var(--neon-pink)', 50, 12);
                        createExplosion(dragon.x, activeHoverY, 'var(--neon-blue)', 50, 10);
                        
                        // 3. Super tremor de finalização
                        triggerScreenShake(1000); 
                        
                        // Repõe a cor da borda original de acordo com a dificuldade
                        if (difficulty === 'Facil') canvas.style.borderColor = 'var(--neon-green)';
                        if (difficulty === 'Medio') canvas.style.borderColor = 'var(--neon-yellow)';
                        if (difficulty === 'Dificil') canvas.style.borderColor = 'var(--neon-red)';
                        
                        // Termina e limpa o chefe
                        dragonDefeated = true; // Impede novo spawn do dragão até que uma nova partida seja iniciada
                        dragon.defeated = true;
                        dragon.active = false;
                        dragon = null;
                        
                        // Limpa inimigos de suporte remanescentes
                        enemies = [];
                        
                        // CORREÇÃO: Volta tudo ao normal com exatamente 3 corações
                        lives = 3;
                        
                        // Ativa de novo as moedas normais da arena
                        spawnCoin();
                    }
                }

                // Atualizar TNTs lançadas
                thrownTnts.forEach((tnt, idx) => {
                    tnt.y += tnt.vy;

                    // Colisão da TNT com o corpo/cabeças flutuantes do dragão (Só se ele não estiver já a cair/morrer!)
                    let activeHoverY = dragon.y + dragon.hoverY;
                    if (!dragon.dying && 
                        tnt.x > dragon.x - dragon.width/2 - 25 && tnt.x < dragon.x + dragon.width/2 + 25 &&
                        tnt.y > activeHoverY - 55 && tnt.y < activeHoverY + 35) {
                        
                        // Impacto certeiro na besta!
                        dragon.hp -= dragon.tntDamage;
                        playSound('dragon_damage');
                        createExplosion(tnt.x, tnt.y, 'var(--neon-orange)', 30, 10);
                        createExplosion(tnt.x, tnt.y, '#ffffff', 15, 6);
                        thrownTnts.splice(idx, 1);
                        triggerScreenShake(400);

                        // Ativa o estado de morte e queda lenta
                        if (dragon.hp <= 0) {
                            dragon.hp = 0;
                            dragon.dying = true; // Inicia sequência de queda
                            score += 100;        // Adiciona pontuação de vitória
                            
                            // Cancela qualquer TNT que estivesse agarrada
                            carryingTnt = false;
                            tnts = [];

                            // Para a trilha musical de ação
                            stopBossMusic();
                            
                            // Toca o som de queda progressivo
                            playSound('falling');
                        }
                    } else if (tnt.y < -30) {
                        // Perdeu-se fora do ecrã, reinicia temporizador para re-spawnar outra tnt
                        thrownTnts.splice(idx, 1);
                        if (dragon && !dragon.dying) {
                            tntSpawnTimer = 60; // 1s se falhar o mapa
                        }
                    }
                });

                // Atualizar e colidir Bolas de Fogo
                fireballs.forEach((fb, idx) => {
                    fb.x += fb.vx;
                    fb.y += fb.vy;

                    // Colisão com o jogador
                    let dist = Math.hypot((player.x + player.size/2) - fb.x, (player.y + player.size/2) - fb.y);
                    if (dist < player.size/2 + fb.size/2) {
                        if (!player.invincible) {
                            lives--;
                            playSound('hit');
                            createExplosion(player.x + player.size/2, player.y + player.size/2, 'var(--neon-red)', 20);
                            triggerScreenShake(400);
                            if (lives <= 0) triggerGameOver();
                        }
                        fireballs.splice(idx, 1);
                    } else if (fb.y > canvas.height + 20 || fb.x < -20 || fb.x > canvas.width + 20) {
                        fireballs.splice(idx, 1);
                    }
                });
            }

            // ATUALIZAR INIMIGOS PADRÃO
            // Se o Boss estiver ativo e em fúria, eles aparecem para atrapalhar! (Mas param se ele estiver a cair)
            if (!dragon || !dragon.active || (dragon.enraged && !dragon.dying)) {
                enemies.forEach((enemy, i) => {
                    enemy.x += enemy.vx;
                    enemy.y += enemy.vy;

                    // Eliminação fora dos limites
                    if (enemy.x < -120 || enemy.x > canvas.width + 120 || enemy.y < -120 || enemy.y > canvas.height + 120) {
                        enemies.splice(i, 1);
                    }

                    // Deteção de colisão com o jogador
                    if (checkCollision(player, enemy)) {
                        if (player.invincible) {
                            createExplosion(enemy.x + enemy.size/2, enemy.y + enemy.size/2, enemy.color);
                            playSound('coin'); 
                            enemies.splice(i, 1);
                            score += 5;
                        } else {
                            lives--;
                            playSound('hit');
                            createExplosion(player.x + player.size/2, player.y + player.size/2, '#ff0055');
                            triggerScreenShake(400);
                            enemies.splice(i, 1);
                            if (lives <= 0) triggerGameOver();
                        }
                    }
                });
            }

            // Recolher moedas de ouro (Apenas se o boss não estiver ativo)
            if (!dragon || !dragon.active) {
                coins.forEach((coin, i) => {
                    coin.glowSize += 0.15 * coin.glowDir;
                    if (coin.glowSize > 5 || coin.glowSize < 0) coin.glowDir *= -1;

                    if (checkCollision(player, coin)) {
                        score += 10;
                        playSound('coin');
                        createExplosion(coin.x, coin.y, '#ffd700');
                        coins.splice(i, 1);
                        spawnCoin();
                    }
                });
            }

            // Recolher power-ups
            powerUps.forEach((pu, i) => {
                if (checkCollision(player, pu)) {
                    player.invincible = true;
                    player.invincibleTimer = 20 * 60; 
                    playSound('powerup');
                    createExplosion(pu.x, pu.y, 'var(--neon-green)');
                    canvas.style.borderColor = '#ffffff'; 
                    powerUps.splice(i, 1);
                }
            });

            // Atualização de partículas
            particles.forEach((p, i) => {
                p.x += p.vx;
                p.y += p.vy;
                p.alpha -= p.decay;
                if (p.alpha <= 0) particles.splice(i, 1);
            });

            draw();
        }

        function checkCollision(rect, obj) {
            let sizeObj = obj.size;
            return rect.x < obj.x + sizeObj &&
                   rect.x + rect.size > obj.x &&
                   rect.y < obj.y + sizeObj &&
                   rect.y + rect.size > obj.y;
        }

        // Função auxiliar para desenhar blocos pixelados (estilo pixel art retro)
        function drawPixelRect(x, y, w, h, color, strokeColor = '#000') {
            ctx.fillStyle = color;
            ctx.fillRect(x, y, w, h);
            ctx.strokeStyle = strokeColor;
            ctx.lineWidth = 1;
            ctx.strokeRect(x, y, w, h);
        }

        function draw() {
            ctx.save();
            
            // MECÂNICA DE MAPA TREMENDO (Rumble constante quando o boss está ativo/morrendo)
            let shakeX = 0;
            let shakeY = 0;
            if (dragon && dragon.active) {
                let currentRumble = dragon.dying ? 7.5 : (dragon.enraged ? 4.5 : 2.5);
                shakeX = (Math.random() - 0.5) * currentRumble;
                shakeY = (Math.random() - 0.5) * currentRumble;
            }
            if (gameContainer.classList.contains('shake')) {
                shakeX += (Math.random() - 0.5) * 11;
                shakeY += (Math.random() - 0.5) * 11;
            }
            ctx.translate(shakeX, shakeY);

            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Grelha de fundo cibernética (Grid Lines)
            ctx.strokeStyle = 'rgba(255, 255, 255, 0.02)';
            ctx.lineWidth = 1;
            for (let i = 0; i < canvas.width; i += 40) {
                ctx.beginPath();
                ctx.moveTo(i, 0);
                ctx.lineTo(i, canvas.height);
                ctx.stroke();
            }
            for (let j = 0; j < canvas.height; j += 40) {
                ctx.beginPath();
                ctx.moveTo(0, j);
                ctx.lineTo(canvas.width, j);
                ctx.stroke();
            }

            // Desenhar rastros de luz neon
            trails.forEach(t => {
                ctx.save();
                ctx.globalAlpha = t.alpha;
                ctx.shadowBlur = 15;
                ctx.shadowColor = t.color;
                ctx.fillStyle = t.color;
                ctx.fillRect(t.x, t.y, t.size, t.size);
                ctx.restore();
            });

            // Desenhar jogador
            ctx.save();
            let currentColor = player.invincible ? `hsl(${player.hue}, 100%, 50%)` : playerColors[player.colorIndex];
            ctx.shadowBlur = player.lightPulse;
            ctx.shadowColor = currentColor;
            ctx.fillStyle = currentColor;
            ctx.fillRect(player.x, player.y, player.size, player.size);
            
            ctx.fillStyle = '#ffffff';
            ctx.fillRect(player.x + 4, player.y + 4, player.size - 8, player.size - 8);
            ctx.restore();

            // Desenhar TNT carregada por cima do jogador
            if (carryingTnt) {
                ctx.save();
                ctx.shadowBlur = 10;
                ctx.shadowColor = 'var(--neon-red)';
                drawPixelRect(player.x + player.size/2 - 10, player.y - 25, 20, 20, '#d50000', '#ffffff');
                ctx.fillStyle = '#ffffff';
                ctx.font = 'bold 8px "Orbitron"';
                ctx.fillText("TNT", player.x + player.size/2 - 8, player.y - 12);
                ctx.restore();
            }

            // Desenhar TNTs caídas no chão (Apenas se o chefe não estiver em queda livre)
            if (dragon && !dragon.dying) {
                tnts.forEach(tnt => {
                    ctx.save();
                    ctx.shadowBlur = 15;
                    ctx.shadowColor = 'var(--neon-red)';
                    drawPixelRect(tnt.x - tnt.size/2, tnt.y - tnt.size/2, tnt.size, tnt.size, '#cc1111', '#ff3333');
                    drawPixelRect(tnt.x - tnt.size/2, tnt.y - 3, tnt.size, 6, '#ffffff', '#dddddd');
                    
                    ctx.fillStyle = '#ffffff';
                    ctx.font = 'bold 11px "Orbitron"';
                    ctx.shadowBlur = 5;
                    ctx.shadowColor = '#00f3ff';
                    ctx.fillText("[E] PEGAR", tnt.x - 28, tnt.y - tnt.size/2 - 8);
                    ctx.restore();
                });
            }

            // Desenhar TNTs em pleno voo
            thrownTnts.forEach(tnt => {
                ctx.save();
                ctx.shadowBlur = 20;
                ctx.shadowColor = 'var(--neon-orange)';
                drawPixelRect(tnt.x, tnt.y, tnt.size, tnt.size, '#ff4500', '#ffff00');
                ctx.fillStyle = '#ffffff';
                ctx.fillRect(tnt.x + tnt.size/2 - 2, tnt.y + tnt.size, 4, 4);
                ctx.restore();
            });

            // DESENHO DO DRAGÃO DE TRÊS CABEÇAS COM MOVIMENTO SENOIDAL E QUEDA
            if (dragon && dragon.active) {
                ctx.save();
                
                if (dragon.dying) {
                    ctx.shadowBlur = 35 + Math.sin(Date.now() * 0.02) * 15;
                    ctx.shadowColor = '#ff5500';
                } else if (dragon.enraged) {
                    ctx.shadowBlur = 30 + Math.sin(Date.now() * 0.01) * 10;
                    ctx.shadowColor = 'var(--neon-red)';
                } else {
                    ctx.shadowBlur = 25;
                    ctx.shadowColor = 'var(--neon-purple)';
                }

                let dX = dragon.x;
                let dY = dragon.y + dragon.hoverY;

                // 1. ASAS PIXELADAS
                let wingOffset = dragon.dying ? (Math.random() * 5) : (Math.sin(dragon.flapAngle) * 20);
                let wingColor = (dragon.dying || dragon.enraged) ? '#800000' : 'var(--neon-purple)';
                let wingDetailColor = (dragon.dying || dragon.enraged) ? '#cc0000' : '#5e00b3';
                
                drawPixelRect(dX - 140, dY - 40 - wingOffset, 70, 30, wingColor, '#ffffff');
                drawPixelRect(dX - 100, dY - 10 - wingOffset, 40, 20, wingDetailColor, '#ffffff');
                drawPixelRect(dX + 70, dY - 40 - wingOffset, 70, 30, wingColor, '#ffffff');
                drawPixelRect(dX + 60, dY - 10 - wingOffset, 40, 20, wingDetailColor, '#ffffff');

                // 2. CORPO CENTRAL DO DRAGÃO
                let bodyColor = (dragon.dying || dragon.enraged) ? '#550000' : '#3a0066';
                let bodyOutline = (dragon.dying || dragon.enraged) ? 'var(--neon-red)' : '#a300cc';
                drawPixelRect(dX - 40, dY - 30, 80, 60, bodyColor, bodyOutline);
                drawPixelRect(dX - 20, dY + 30, 40, 20, (dragon.dying || dragon.enraged) ? '#330000' : '#240041', bodyOutline);

                // 3. TRÊS PESCOÇOS E TRÊS CABEÇAS
                let headColors = (dragon.dying || dragon.enraged) ? ['#990000', '#d50000', '#990000'] : ['#5e00b3', '#8000ff', '#5e00b3'];
                let neckColor = (dragon.dying || dragon.enraged) ? '#4a0000' : '#240041';
                let neckOutline = (dragon.dying || dragon.enraged) ? 'var(--neon-red)' : '#8000ff';
                let headOffsets = [-50, 0, 50];

                for (let h = 0; h < 3; h++) {
                    let hX = dX + headOffsets[h];
                    let hY = dY - 50 + (h === 1 ? -15 : 0);

                    if (dragon.dying) {
                        hY += Math.sin(Date.now() * 0.05 + h) * 4;
                    }

                    drawPixelRect(hX - 8, hY + 15, 16, 40, neckColor, neckOutline);
                    drawPixelRect(hX - 16, hY - 16, 32, 32, headColors[h], '#ffffff');
                    
                    let eyeColor = (dragon.dying && Math.random() < 0.3) ? '#440000' : 'var(--neon-red)';
                    drawPixelRect(hX - 10, hY - 6, 6, 6, eyeColor, '#ff0000');
                    drawPixelRect(hX + 4, hY - 6, 6, 6, eyeColor, '#ff0000');
                }
                ctx.restore();

                // BARRA DE VIDA DO CHEFE (HP) (Oculta se estiver morrendo/dying)
                if (!dragon.dying) {
                    ctx.save();
                    let barW = 160;
                    let barH = 12;
                    let barX = dragon.x - barW / 2;
                    let barY = dY - 95;

                    if (barY < 15) barY = 15;

                    ctx.fillStyle = 'rgba(0,0,0,0.7)';
                    ctx.fillRect(barX, barY, barW, barH);
                    
                    ctx.strokeStyle = dragon.enraged ? '#ff0000' : 'var(--neon-pink)';
                    ctx.lineWidth = 2;
                    ctx.strokeRect(barX, barY, barW, barH);

                    let hpPercent = dragon.hp / dragon.maxHp;
                    ctx.fillStyle = dragon.enraged ? '#ff0000' : 'var(--neon-pink)';
                    ctx.shadowBlur = dragon.enraged ? 12 : 8;
                    ctx.shadowColor = dragon.enraged ? '#ff0000' : 'var(--neon-pink)';
                    ctx.fillRect(barX + 2, barY + 2, (barW - 4) * hpPercent, barH - 4);

                    ctx.fillStyle = '#ffffff';
                    ctx.font = 'bold 9px "Orbitron"';
                    ctx.shadowBlur = 3;
                    ctx.shadowColor = '#fff';
                    
                    let currentScale = Math.ceil(dragon.hp / 10);
                    let maxScale = dragon.maxHp / 10;
                    
                    let titleText = dragon.enraged ? "DRAGÃO ENFURECIDO!" : "DRAGÃO";
                    ctx.fillText(`${titleText}: ${currentScale} / ${maxScale}`, barX + 18, barY - 6);
                    ctx.restore();
                }
            }

            // Desenhar inimigos normais
            if (!dragon || !dragon.active || (dragon.enraged && !dragon.dying)) {
                enemies.forEach(enemy => {
                    ctx.save();
                    ctx.shadowBlur = 10;
                    ctx.shadowColor = enemy.color;
                    ctx.fillStyle = enemy.color;
                    ctx.fillRect(enemy.x, enemy.y, enemy.size, enemy.size);
                    ctx.restore();
                });
            }

            // Desenhar bolas de fogo do dragão
            fireballs.forEach(fb => {
                ctx.save();
                ctx.shadowBlur = 15;
                ctx.shadowColor = 'var(--neon-orange)';
                ctx.beginPath();
                ctx.arc(fb.x, fb.y, fb.size/2, 0, Math.PI * 2);
                ctx.fillStyle = '#ffffff'; 
                ctx.fill();
                ctx.lineWidth = 3;
                ctx.strokeStyle = 'var(--neon-orange)';
                ctx.stroke();
                ctx.restore();
            });

            // Desenhar moedas de ouro (penas se boss inativo)
            if (!dragon || !dragon.active) {
                coins.forEach(coin => {
                    ctx.save();
                    ctx.shadowBlur = 15 + coin.glowSize;
                    ctx.shadowColor = '#ffd700';
                    
                    ctx.beginPath();
                    ctx.arc(coin.x, coin.y, coin.size / 2, 0, Math.PI * 2);
                    ctx.fillStyle = '#ffffff'; 
                    ctx.fill();
                    
                    ctx.lineWidth = 3;
                    ctx.strokeStyle = '#ffd700'; 
                    ctx.stroke();
                    ctx.closePath();
                    ctx.restore();
                });
            }

            // Desenhar power-ups
            powerUps.forEach(pu => {
                ctx.save();
                ctx.shadowBlur = 20;
                ctx.shadowColor = 'var(--neon-green)';
                
                ctx.beginPath();
                ctx.arc(pu.x, pu.y, pu.size / 2, 0, Math.PI * 2);
                ctx.fillStyle = '#ffffff';
                ctx.fill();

                ctx.lineWidth = 3;
                ctx.strokeStyle = 'var(--neon-green)';
                ctx.stroke();
                ctx.closePath();
                ctx.restore();
            });

            // Desenhar partículas de explosão luminosas
            particles.forEach(p => {
                ctx.save();
                ctx.globalAlpha = p.alpha;
                if (p.color !== '#555566') {
                    ctx.shadowBlur = 8;
                    ctx.shadowColor = p.color;
                }
                ctx.fillStyle = p.color;
                ctx.fillRect(p.x, p.y, p.size, p.size);
                ctx.restore();
            });

            // HUD Neon
            ctx.fillStyle = '#ffffff';
            ctx.font = 'bold 16px "Orbitron"';
            
            ctx.shadowBlur = 5;
            ctx.shadowColor = '#fff';
            ctx.fillText(`SCORE: ${score}`, 25, 40);
            ctx.shadowBlur = 0;

            let diffColor = 'var(--neon-green)';
            if (difficulty === 'Medio') diffColor = 'var(--neon-yellow)';
            if (difficulty === 'Dificil') diffColor = 'var(--neon-red)';
            
            ctx.fillStyle = diffColor;
            ctx.fillText(`DIFICULDADE: ${difficulty.toUpperCase()}`, 25, 70);

            ctx.shadowBlur = 10;
            ctx.shadowColor = 'var(--neon-pink)';
            ctx.fillStyle = 'var(--neon-pink)';
            ctx.fillText(`VIDAS: ${'❤️'.repeat(lives)}`, canvas.width - 200, 40);
            ctx.shadowBlur = 0;

            // Barra de progresso da invencibilidade
            if (player.invincible) {
                let pCent = player.invincibleTimer / (20 * 60);
                ctx.save();
                ctx.fillStyle = `hsl(${player.hue}, 100%, 50%)`;
                ctx.shadowBlur = 10;
                ctx.shadowColor = `hsl(${player.hue}, 100%, 50%)`;
                ctx.fillText(`INVENCÍVEL: ${(player.invincibleTimer / 60).toFixed(1)}s`, canvas.width / 2 - 110, 40);
                ctx.fillRect(canvas.width / 2 - 110, 55, 220 * pCent, 6);
                ctx.restore();
            }

            // Alertas visuais de Boss
            if (dragon && dragon.active) {
                if (dragon.y < dragon.targetY) {
                    ctx.save();
                    ctx.fillStyle = 'var(--neon-red)';
                    ctx.font = 'bold 24px "Orbitron"';
                    ctx.shadowBlur = 15;
                    ctx.shadowColor = 'var(--neon-red)';
                    ctx.fillText("AVISO: DRAGÃO DETETADO!", canvas.width / 2 - 180, canvas.height / 2);
                    ctx.restore();
                } else if (dragon.dying) {
                    ctx.save();
                    ctx.fillStyle = 'var(--neon-orange)';
                    ctx.font = 'bold 24px "Orbitron"';
                    ctx.shadowBlur = 15;
                    ctx.shadowColor = 'var(--neon-orange)';
                    ctx.fillText("O DRAGÃO ESTÁ A CAIR!", canvas.width / 2 - 170, canvas.height / 2 + 50);
                    ctx.restore();
                }
            }

            ctx.restore(); // Restaura as transformações de tremor
        }

        function triggerGameOver() {
            gameOver = true;
            clearInterval(gameInterval);
            clearInterval(this.enemySpawner);
            clearInterval(this.powerUpSpawner);
            stopBossMusic();
            playSound('gameover');
            
            finalScoreText.innerHTML = `PONTUAÇÃO FINAL:<br><span style="font-size: 38px; font-weight:900; color:#00f3ff; text-shadow: 0 0 10px #00f3ff;">${score} PONTOS</span><br><br>NA DIFICULDADE: <span style="color: #ffeb3b;">${difficulty.toUpperCase()}</span>`;
            gameOverScreen.style.display = 'block';
        }

        function resetToMenu() {
            gameOverScreen.style.display = 'none';
            canvas.style.display = 'none';
            menu.style.display = 'flex';
            stopBossMusic();
        }
    </script>
</body>
</html>

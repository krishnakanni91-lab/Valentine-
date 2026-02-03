<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>For My Moon 🌙</title>
    <style>
        :root { --primary: #ff4d6d; --secondary: #ffb3c1; --dark: #590d22; }
        body { margin: 0; font-family: 'Arial', sans-serif; background: var(--secondary); display: flex; align-items: center; justify-content: center; height: 100vh; overflow: hidden; text-align: center; touch-action: manipulation; }
        .container { background: white; padding: 25px; border-radius: 20px; box-shadow: 0 10px 30px rgba(0,0,0,0.1); width: 85%; max-width: 350px; position: relative; z-index: 10; }
        h1 { color: var(--dark); font-size: 1.4rem; margin-bottom: 10px; }
        .btn-group { display: flex; justify-content: space-around; margin-top: 20px; height: 60px; position: relative; }
        button { padding: 12px 25px; border: none; border-radius: 50px; cursor: pointer; font-weight: bold; font-size: 1rem; }
        #yesBtn { background: var(--primary); color: white; z-index: 5; transition: 0.3s; }
        #noBtn { background: #555; color: white; position: absolute; transition: 0.1s; z-index: 10; }
        #timer { font-size: 3.5rem; color: var(--primary); display: none; font-weight: bold; }
        #slideshow { display: none; width: 100vw; height: 100vh; position: fixed; top: 0; left: 0; background: black; z-index: 100; }
        .slide { width: 100%; height: 100%; object-fit: contain; display: none; }
        .active { display: block; }
        #overlay { position: absolute; top: 50%; width: 100%; transform: translateY(-50%); text-align: center; color: white; pointer-events: none; }
        .glitch { animation: glitch 0.1s infinite; color: #ff0000; font-size: 2.2rem; display: none; font-weight: bold; text-shadow: 2px 2px black; }
        @keyframes glitch { 0% { transform: translate(2px,0) skew(5deg); } 50% { transform: translate(-2px,0) skew(-5deg); } }
        .roadmap-container { max-height: 55vh; overflow-y: auto; padding: 10px; margin-top: 10px; }
        .day-item { background: #fff0f3; margin: 8px 0; padding: 10px; border-radius: 10px; text-align: left; font-size: 0.85rem; border-left: 4px solid var(--primary); }
        input { width: 90%; padding: 12px; margin-top: 10px; border-radius: 10px; border: 1px solid #ccc; font-size: 1rem; box-sizing: border-box; }
        #secretMsg { display: none; margin-top: 15px; border: 2px dashed var(--primary); padding: 15px; background: #fff0f3; color: var(--dark); border-radius: 15px; line-height: 1.5; font-size: 0.95rem; }
    </style>
</head>
<body>

    <div id="proposal" class="container">
        <h1 id="mainText">Will you be my Valentine? ❤️</h1>
        <div class="btn-group">
            <button id="yesBtn" onclick="handleYes()">YES</button>
            <button id="noBtn" onclick="handleNo()">NO</button>
        </div>
        <div id="timer">10</div>
    </div>

    <div id="slideshow">
        <img src="5607.jpg" class="slide active">
        <img src="6251.jpg" class="slide">
        <img src="6252.jpg" class="slide">
        <img src="5534.jpg" class="slide">
        <img src="6246.jpg" class="slide">
        <img src="5551.jpg" class="slide">
        <img src="35428.jpg" class="slide">
        <img src="5550.jpg" class="slide">
        <img src="6294.jpg" class="slide">
        <div id="overlay">
            <p id="romanceText" style="font-size: 1.4rem; padding: 20px; text-shadow: 2px 2px 8px black;"></p>
            <div id="glitchText" class="glitch">SYSTEM ERROR</div>
        </div>
    </div>

    <canvas id="gameCanvas" style="display:none; position:fixed; top:0; left:0; z-index:200;"></canvas>

    
   <audio id="bgMusic" src="September 4.mp3" loop></audio>
    
    <script>
        let noCount = 0;
        const noBtn = document.getElementById('noBtn');
        const yesBtn = document.getElementById('yesBtn');
        const timerDisplay = document.getElementById('timer');
        const music = document.getElementById('bgMusic');

        function handleYes() { alert("Wrong choice, click No! 😉"); }

        function handleNo() {
            noCount++;
            if (noCount < 8) {
                const x = Math.random() * (window.innerWidth - 100);
                const y = Math.random() * (window.innerHeight - 100);
                noBtn.style.position = 'fixed';
                noBtn.style.left = x + 'px';
                noBtn.style.top = y + 'px';
            } else {
                noBtn.style.display = 'none';
                timerDisplay.style.display = 'block';
                let timeLeft = 10;
                let countdown = setInterval(() => {
                    timeLeft--;
                    timerDisplay.innerText = timeLeft;
                    if (timeLeft <= 0) {
                        clearInterval(countdown);
                        timerDisplay.style.display = 'none';
                        yesBtn.innerText = "OKAY FINE, YES!";
                        yesBtn.style.transform = "scale(1.5)";
                        yesBtn.onclick = startSlideshow;
                    }
                }, 1000);
            }
        }

        function startSlideshow() {
            document.getElementById('proposal').style.display = 'none';
            document.getElementById('slideshow').style.display = 'block';
            music.play();
            
            let slides = document.querySelectorAll('.slide');
            let current = 0;
            let slideInt = setInterval(() => {
                slides[current].classList.remove('active');
                current++;
                if (current >= slides.length) {
                    clearInterval(slideInt);
                    showFinalMessage();
                } else {
                    slides[current].classList.add('active');
                }
            }, 3000);
        }

        function showFinalMessage() {
            const txt = document.getElementById('romanceText');
            txt.innerText = "Every moment with you is a favorite memory...";
            setTimeout(() => {
                document.getElementById('glitchText').style.display = 'block';
                setTimeout(() => {
                    document.getElementById('glitchText').style.display = 'none';
                    txt.style.color = "#ff4d6d";
                    txt.innerText = "I want you in my arms, and I'm not taking no for an answer. 🔥";
                    setTimeout(initGame, 4000);
                }, 800);
            }, 3000);
        }

        function initGame() {
            document.getElementById('slideshow').style.display = 'none';
            const canvas = document.getElementById('gameCanvas');
            canvas.style.display = 'block';
            const ctx = canvas.getContext('2d');
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
            let score = 0;
            let hearts = [];
            let basketX = canvas.width / 2;

            canvas.addEventListener('touchmove', e => {
                basketX = e.touches[0].clientX - 50;
                e.preventDefault();
            }, {passive: false});

            function spawn() { hearts.push({ x: Math.random()*canvas.width, y: 0, s: 6 }); }
            
            function loop() {
                ctx.fillStyle = "#1a1a1a";
                ctx.fillRect(0,0,canvas.width, canvas.height);
                ctx.fillStyle = "white";
                ctx.font = "20px Arial";
                ctx.fillText("Catch 10 Hearts: " + score, 20, 40);
                ctx.fillStyle = "#ff4d6d";
                ctx.fillRect(basketX, canvas.height-100, 100, 20);
                hearts.forEach((h, i) => {
                    h.y += h.s;
                    ctx.fillText("❤️", h.x, h.y);
                    if(h.y > canvas.height-100 && h.x > basketX && h.x < basketX + 100) {
                        hearts.splice(i, 1);
                        score++;
                    }
                });
                if(Math.random() < 0.06) spawn();
                if(score < 10) requestAnimationFrame(loop);
                else showRoadmap();
            }
            loop();
        }

        function showRoadmap() {
            document.getElementById('gameCanvas').style.display = 'none';
            document.body.innerHTML = `
                <div class="container" style="overflow-y: auto; max-height: 90vh;">
                    <h2 style="color:var(--primary); margin:0;">Our Valentine's Plan 📅</h2>
                    <div class="roadmap-container">
                        <div class="day-item">🌹 <b>7th Rose Day:</b> To the prettiest flower.</div>
                        <div class="day-item">💍 <b>8th Propose Day:</b> I'd choose you every time.</div>
                        <div class="day-item">🍫 <b>9th Chocolate Day:</b> Something sweet for my sweetheart.</div>
                        <div class="day-item">🧸 <b>10th Teddy Day:</b> A hug you can keep forever.</div>
                        <div class="day-item">🤝 <b>11th Promise Day:</b> I promise to always be yours.</div>
                        <div class="day-item">🫂 <b>12th Hug Day:</b> My favorite place is in your arms.</div>
                        <div class="day-item">💋 <b>13th Kiss Day:</b> Can't wait for this one.</div>
                        <div class="day-item" style="background:var(--primary); color:white;">💝 <b>14th VALENTINE'S DAY:</b> I LOVE YOU.</div>
                    </div>
                    <input type="number" id="pass" placeholder="Anniversary Date (DDMM)">
                    <button onclick="checkPass()" style="background:var(--primary); color:white; margin-top:10px; width:100%;">Unlock Secret</button>
                    <div id="secretMsg">
                        <p><b>You finally unlocked it! You're stuck with me now. 😉</b></p>
                        <p>Looking at our old photos makes me realize how far we've come since college, and looking at you makes me realize how far I’m willing to go to keep you happy. 🔥❤️</p>
                    </div>
                </div>
            `;
            document.body.style.background = "var(--dark)";
        }

        function checkPass() {
            // SET YOUR DATE HERE (DDMM)
            if(document.getElementById('pass').value === '1205') {
                document.getElementById('secretMsg').style.display = 'block';
                window.scrollTo(0, document.body.scrollHeight);
            } else { alert("Wrong date, babe!"); }
        }
    </script>
</body>
</html>

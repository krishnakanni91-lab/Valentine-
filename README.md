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
        .warning { color: #ff0000; font-weight: bold; display: none; margin-top: 10px; font-size: 0.9rem; }
        #timer { font-size: 3.5rem; color: var(--primary); display: none; font-weight: bold; }
        
        /* Slideshow */
        #slideshow { display: none; width: 100vw; height: 100vh; position: fixed; top: 0; left: 0; background: black; z-index: 100; }
        .slide { width: 100%; height: 100%; object-fit: contain; display: none; }
        .active { display: block; }
        #overlay { position: absolute; top: 50%; width: 100%; transform: translateY(-50%); text-align: center; color: white; pointer-events: none; }
        .glitch { animation: glitch 0.1s infinite; color: #ff0000; font-size: 2.2rem; display: none; font-weight: bold; text-shadow: 2px 2px black; }
        @keyframes glitch { 0% { transform: translate(2px,0) skew(5deg); } 50% { transform: translate(-2px,0) skew(-5deg); } }

        /* Roadmap & Final Screen */
        .roadmap-container { max-height: 60vh; overflow-y: auto; padding: 10px; margin-top: 10px; }
        .day-item { background: #fff0f3; margin: 8px 0; padding: 10px; border-radius: 10px; text-align: left; font-size: 0.85rem; border-left: 4px solid var(--primary); }
        input { width: 90%; padding: 12px; margin-top: 10px; border-radius: 10px; border: 1px solid #ccc; font-size: 1rem; box-sizing: border-box; }
        #secretMsg { display: none; margin-top: 15px; border: 2px dashed var(--primary); padding: 15px; background: #fff0f3; color: var(--dark); border-radius: 15px; line-height: 1.5; font-size: 0.95rem; }
    </style>
</head>
<body>

    <div id="proposal" class="container">
        <h1 id="mainText">Will you be my Valentine? ❤️</h1>
        <p id="warning" class="warning">⚠️ CLICK NO! ⚠️</p>
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
        <div id="overlay">
            <p id="romanceText" style="font-size: 1.4rem; padding: 20px; text-shadow: 2px 2px 8px black;"></p>
            <div id="glitchText" class="glitch">SYSTEM ERROR</div>
        </div>
    </div>

    <canvas id="gameCanvas" style="display:none; position:fixed; top:0; left:0; z-index:200;"></canvas>
    <audio id="bgMusic"https://indirect-magenta-0ln0zr2dt5.edgeone.app/September%204.mp3" loop></audio>

    <script>
        let noCount = 0;
        const noBtn = document.getElementById('noBtn');
        const yesBtn = document.getElementById('yesBtn');
        const warning = document.getElementById('warning');
        const mainText = document.getElementById('mainText');
        const timerDisplay = document.getElementById('timer');
        const music = document.getElementById('bgMusic');

        function handleYes() {
            warning.style.display = 'block';
            mainText.innerText = "Error! Wrong Choice!";
        }

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
            music.play().catch(() => {});
            
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
                ctx.fillText("Catch 10 Hearts for her: " + score, 20, 40);
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
                    <button onclick="checkPass()" style="background:var(--primary); color:white; margin-top:10px; width:100%;">Unlock Secret Message</button>
                    <div id="secretMsg">
                        <p><b>You finally unlocked it! You're stuck with me now. 😉</b></p>
                        <p>Looking at our old photos makes me realize how far we've come since college, and looking at you makes me realize how far I’m willing to go to keep you happy.</p>
                        <p>Get ready for Valentine's week, because I'm planning to make every second unforgettable for you. I'm obsessed with you, babe. 🔥❤️</p>
                    </div>
                </div>
            `;
            document.body.style.background = "var(--dark)";
        }

        function checkPass() {
            // Replace '1205' with your actual anniversary date (DDMM)
            if(document.getElementById('pass').value === '0405![27095](https://github.com/user-attachments/assets/d3fb67d4-bc56-4e2c-8b72-313241690c53)
![27088](https://github.com/user-attachments/assets/0c4abeb0-777b-4dcc-832c-07d8a86bc3fd)
![27090](https://github.com/user-attachments/assets/93ed3fa7-7558-45d6-a7ce-8ebdc2e58861)
![27093](https://github.com/user-attachments/assets/4ff93042-99f5-4c2c-aa57-6a17d6ed1f46)
![23585](https://github.com/user-attachments/assets/a8fc5d97-ecfb-45ed-a290-db7aedeff226)
![23586](https://github.com/user-attachments/assets/54e04243-f108-4013-8c6d-c342867213ce)
![19841](https://github.com/user-attachments/assets/3838b55c-3503-4ede-b07b-8cef14198506)
![19840](https://github.com/user-attachments/assets/8406d5a0-febe-4e6b-afa7-154953679458)
![20372](https://github.com/user-attachments/assets/f3914ab6-a51b-49d4-a20b-adc16bbeb38e)
![6232](https://github.com/user-attachments/assets/d9d70af4-9f75-4c43-ab7c-247b9c36e573)
![6230](https://github.com/user-attachments/assets/5b68bb20-9dc3-4c71-bd6c-5d1925567409)
![6201](https://github.com/user-attachments/assets/328e7cd3-f3a1-4ba9-ba0b-8a60d8a90e8d)
![6290](https://github.com/user-attachments/assets/0e0397ae-a825-4ff4-bff6-e6942a1ac85c)
![6291](https://github.com/user-attachments/assets/3be41757-acfc-4af2-ba96-f0b14f09f42c)
![6212](https://github.com/user-attachments/assets/02f92e68-9d32-4fe6-9d11-1c91d585d2d3)
![6208](https://github.com/user-attachments/assets/fbcafc58-b0c9-4f8a-a8cf-4cdeb9bccd2e)
![6209](https://github.com/user-attachments/assets/cc974b53-04c9-4bda-97e3-b5461997c6ad)
![6222](https://github.com/user-attachments/assets/4476cb6e-e5d2-4f52-8348-c00b6e81e576)
![6220](https://github.com/user-attachments/assets/7b51fb1a-9e97-4dc2-932d-ff9ef6909027)
![6221](https://github.com/user-attachments/assets/a8b29065-12cf-4bed-ab88-aedd5e0b46ec)
![6248](https://github.com/user-attachments/assets/35615d26-df5d-4b4c-b011-99d4bb937131)
![6247](https://github.com/user-attachments/assets/232e4c64-ca87-4b0c-9cbc-6b1c3309fe8a)
![6246](https://github.com/user-attachments/assets/bf3976bb-4292-4b26-a218-bd4b87fc2e1e)
![6252](https://github.com/user-attachments/assets/8cf16fa7-a9d8-4703-ae91-08ad958c058b)
![6309](https://github.com/user-attachments/assets/b0dfea74-a2c5-46af-85b7-ea654f31f942)
![6334](https://github.com/user-attachments/assets/c0135329-2386-4050-8dee-9ff68e63394d)
![6311](https://github.com/user-attachments/assets/0260a47c-f215-4e60-b153-645b7cb4383f)
![35428](https://github.com/user-attachments/assets/819977b8-e8ec-4836-973e-522385590b1d)
![35485](https://github.com/user-attachments/assets/60cebe77-4ac7-4982-a1cf-50b0af28d7dd)
![35489](https://github.com/user-attachments/assets/835569bd-9123-4667-88d5-6632821a7447)
![35492](https://github.com/user-attachments/assets/44f68154-d764-4849-9dba-fb537483093f)
![35493](https://github.com/user-attachments/assets/7dc5afd4-3c7f-4a4e-8766-28d3fdabb0c7)
![35670](https://github.com/user-attachments/assets/743edc60-6f62-47c0-9577-b7ca83f6053f)
![35671](https://github.com/user-attachments/assets/04e76894-a107-42c0-a146-c25c142f87e0)
![35672](https://github.com/user-attachments/assets/48e73997-e04e-4dc6-b95c-9c9f81580cbe)
![35673](https://github.com/user-attachments/assets/d0e6838c-35a2-419e-849e-66558856bd11)
![35823](https://github.com/user-attachments/assets/9844e84f-60f6-4e4b-a193-9a51c0fe81f0)
![5562](https://github.com/user-attachments/assets/82155825-98de-44e3-911d-5bbf4738b5db)
![5554](https://github.com/user-attachments/assets/ef2471d7-83f4-434c-a430-e50bd3fd5ca8)
![5551](https://github.com/user-attachments/assets/51c8005b-2332-46e9-93c8-b7a6593cfda2)
![5550](https://github.com/user-attachments/assets/ca15a4a1-1596-45fd-9035-567bf71e63ef)
![5507](https://github.com/user-attachments/assets/9ab179b6-c16f-4343-a46b-13339d4c7ac8)
![5526](https://github.com/user-attachments/assets/9cf6ae68-eefc-41f8-93df-cefe0cb1c235)
![5614](https://github.com/user-attachments/assets/9f266cd9-879a-469b-a780-1d7b5bfe9e70)
![5544](https://github.com/user-attachments/assets/d3752b81-35a4-4397-b3b2-f3cdb173d517)
![5546](https://github.com/user-attachments/assets/c8d06a9c-e2b4-4bdf-846b-50d40f8c9517)
![5547](https://github.com/user-attachments/assets/08def30d-f586-4b89-853e-472de8c884c3)
![5639](https://github.com/user-attachments/assets/0c9c52ba-8495-4a5b-b46d-42b45e04c626)
![5607](https://github.com/user-attachments/assets/1a05df13-abeb-4013-8bbe-344ceefccc75)
') {
                document.getElementById('secretMsg').style.display = 'block';
                window.scrollTo(0, document.body.scrollHeight);
            } else {
                alert("Wrong date, babe! Hint: Think of our special day.");
            }
        }
    </script>
</body>
</html>

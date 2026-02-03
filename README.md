<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Be My Valentine?</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            height: 100dvh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background: linear-gradient(to right, #ffafbd, #ffc3a0);
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            overflow: hidden;
            text-align: center;
            touch-action: manipulation;
        }

        h1 {
            color: #d6336c;
            font-size: 2.2rem;
            margin-bottom: 10px;
            padding: 0 20px;
        }

        #feedback-msg {
            font-size: 1.3rem;
            color: #b33939;
            font-weight: bold;
            height: 50px;
            margin-bottom: 20px;
            padding: 0 10px;
        }

        .buttons-container {
            position: relative;
            width: 100%;
            height: 300px; /* Space for the button to jump around */
        }

        button {
            padding: 15px 35px;
            font-size: 1.2rem;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            font-weight: bold;
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
            transition: transform 0.1s ease;
        }

        #yesBtn {
            background-color: #ff4d6d;
            color: white;
            position: absolute;
            left: 50%;
            top: 20%;
            transform: translate(-50%, -50%);
            z-index: 10;
        }

        #noBtn {
            background-color: #7f8c8d;
            color: white;
            position: fixed; /* Fixed is key for mobile jumping */
            left: 50%;
            top: 60%;
            transform: translate(-50%, -50%);
            z-index: 5;
            transition: all 0.2s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }

        /* Success Screen */
        #success-container {
            display: none;
            position: fixed;
            inset: 0;
            background: #fff0f3;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 1000;
            padding: 20px;
        }

        .heart {
            background-color: #ff4d6d;
            height: 70px;
            width: 70px;
            transform: rotate(-45deg);
            animation: beat 0.6s infinite alternate;
            margin-bottom: 50px;
        }
        .heart:before, .heart:after {
            content: "";
            background-color: #ff4d6d;
            border-radius: 50%;
            height: 70px;
            width: 70px;
            position: absolute;
        }
        .heart:before { top: -35px; left: 0; }
        .heart:after { left: 35px; top: 0; }

        @keyframes beat {
            to { transform: scale(1.2) rotate(-45deg); }
        }

        .final-text {
            font-size: 1.6rem;
            color: #c9184a;
            font-weight: bold;
            white-space: pre-wrap;
        }
    </style>
</head>
<body>

    <div id="game-ui">
        <h1>Do you want to be my Valentine?</h1>
        <p id="feedback-msg">I promise I'm nice! 😉</p>
        
        <div class="buttons-container">
            <button id="yesBtn">YES</button>
            <button id="noBtn">NO</button>
        </div>
    </div>

    <div id="success-container">
        <div class="heart"></div>
        <div class="final-text" id="finalMsg"></div>
    </div>

    <audio id="bgMusic" loop>
        <source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3" type="audio/mpeg">
    </audio>

    <script>
        const noBtn = document.getElementById('noBtn');
        const yesBtn = document.getElementById('yesBtn');
        const feedback = document.getElementById('feedback-msg');
        const success = document.getElementById('success-container');
        const finalMsg = document.getElementById('finalMsg');
        const audio = document.getElementById('bgMusic');

        const funMessages = [
            "Nice try! 😂",
            "Too slow! 🏃‍♀️",
            "Not happening! 🙅‍♀️",
            "Are you even trying? 😏",
            "Wrong button! 🚩",
            "I'm over here now! ✨",
            "Catch me if you can! 💨",
            "Give up yet? 😜"
        ];

        let msgIndex = 0;

        function escape() {
            // Calculate random coordinates
            // We keep it within 10% to 90% of screen to avoid getting stuck in corners
            const newX = Math.random() * (window.innerWidth - 150) + 50;
            const newY = Math.random() * (window.innerHeight - 150) + 50;

            noBtn.style.left = `${newX}px`;
            noBtn.style.top = `${newY}px`;
            noBtn.style.transform = 'translate(-50%, -50%)';

            // Change the message
            feedback.innerText = funMessages[msgIndex];
            msgIndex = (msgIndex + 1) % funMessages.length;
        }

        // The "Running" logic for both Desktop and Mobile
        noBtn.addEventListener('mouseover', escape); // PC
        noBtn.addEventListener('touchstart', (e) => {
            e.preventDefault(); // Critical: Stops the phone from clicking the button
            escape();
        });

        function celebrate() {
            success.style.display = 'flex';
            finalMsg.innerText = "💝Clever got you 😄.\n\nThis is the best \"yes\" ever 💝💖\n\nYou are his valentine now 🥰🥰";
            audio.play().catch(() => console.log("Music started"));
        }

        yesBtn.addEventListener('click', celebrate);
        yesBtn.addEventListener('touchstart', (e) => {
            e.preventDefault();
            celebrate();
        });

    </script>
</body>
</html>

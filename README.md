<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lucky Spin Wheel</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #f0f0f0;
            margin-top: 50px;
        }
        #wheel {
            width: 300px;
            height: 300px;
            border-radius: 50%;
            background: conic-gradient(
                #ff6b6b 0deg 45deg, /* Lose */
                #4ecdc4 45deg 90deg, /* 50 Coins */
                #45b7d1 90deg 135deg, /* 100 Coins */
                #96c93d 135deg 180deg, /* Extra Spin */
                #f7d794 180deg 225deg, /* 500 Coins */
                #ff9f1c 225deg 270deg, /* Jackpot */
                #d4a5a5 270deg 315deg, /* 10 Coins */
                #9b59b6 315deg 360deg /* Mystery */
            );
            margin: 0 auto;
            position: relative;
            transition: transform 3s ease-out;
        }
        #spinBtn {
            margin-top: 20px;
            padding: 10px 20px;
            font-size: 18px;
            cursor: pointer;
            background-color: #2ecc71;
            border: none;
            color: white;
            border-radius: 5px;
        }
        #spinBtn:disabled {
            background-color: #95a5a6;
            cursor: not-allowed;
        }
        #result {
            margin-top: 20px;
            font-size: 20px;
            color: #333;
        }
        #coins {
            font-size: 24px;
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <h1>Lucky Spin Wheel</h1>
    <div id="wheel"></div>
    <button id="spinBtn">Spin (10 Coins)</button>
    <div id="coins">Coins: 100</div>
    <div id="result"></div>

    <script>
        const wheel = document.getElementById('wheel');
        const spinBtn = document.getElementById('spinBtn');
        const resultDiv = document.getElementById('result');
        const coinsDiv = document.getElementById('coins');
        let coins = 100;
        let isSpinning = false;

        const outcomes = [
            { name: "Lose", value: 0, color: "#ff6b6b" },
            { name: "50 Coins", value: 50, color: "#4ecdc4" },
            { name: "100 Coins", value: 100, color: "#45b7d1" },
            { name: "Extra Spin", value: 0, extraSpin: true, color: "#96c93d" },
            { name: "500 Coins", value: 500, color: "#f7d794" },
            { name: "Jackpot", value: 1000, color: "#ff9f1c" },
            { name: "10 Coins", value: 10, color: "#d4a5a5" },
            { name: "Mystery", value: Math.floor(Math.random() * 200), color: "#9b59b6" }
        ];

        spinBtn.addEventListener('click', () => {
            if (coins < 10 || isSpinning) return;

            coins -= 10;
            coinsDiv.textContent = `Coins: ${coins}`;
            spinBtn.disabled = true;
            isSpinning = true;
            resultDiv.textContent = '';

            // Random spin between 3-5 full rotations + random segment
            const randomSpin = 1080 + Math.floor(Math.random() * 360);
            wheel.style.transform = `rotate(${randomSpin}deg)`;

            setTimeout(() => {
                const segmentAngle = 360 / outcomes.length;
                const finalAngle = randomSpin % 360;
                const outcomeIndex = Math.floor(finalAngle / segmentAngle);
                const outcome = outcomes[outcomeIndex];

                if (outcome.extraSpin) {
                    resultDiv.textContent = `You won an Extra Spin!`;
                    coinsDiv.textContent = `Coins: ${coins} (+1 Spin)`;
                } else {
                    coins += outcome.value;
                    resultDiv.textContent = `You won: ${outcome.name}!`;
                    coinsDiv.textContent = `Coins: ${coins}`;
                }

                spinBtn.disabled = false;
                isSpinning = false;
                wheel.style.transform = `rotate(0deg)`; // Reset for next spin
            }, 3000); // Matches CSS transition duration
        });

        // Update button state based on coins
        setInterval(() => {
            spinBtn.disabled = coins < 10 || isSpinning;
        }, 100);
    </script>
</body>
</html>

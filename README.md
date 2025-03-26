<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Press for Cash</title>
    <style>
        body { text-align: center; font-family: Arial, sans-serif; }
        .top-bar { display: flex; justify-content: space-between; padding: 10px; }
        .convert-section { margin-top: 10px; }
        #adButton, #cashoutSection { display: none; }
    </style>
</head>
<body>

    <div class="top-bar">
        <div>Coins: <span id="coins">0</span></div>
        <div id="convertScreen">Convert: 10,000 = £0.01</div>
    </div>

    <h1>Balance: £<span id="balance">0.00</span></h1>
    <button id="clickMe">Click Me</button>
    <button id="adButton">Watch Ad</button>

    <div class="convert-section">
        <button id="convertButton">Convert Coins</button>
    </div>

    <div id="cashoutSection">
        <h3>Cashout</h3>
        <input type="email" id="paypalEmail" placeholder="Enter PayPal Email">
        <button id="cashoutButton">Cash Out</button>
    </div>

    <script>
        let balance = 0.00;
        let coins = 0;
        let clicks = 0;

        document.getElementById("clickMe").addEventListener("click", function() {
            balance += 1.50;
            clicks++;
            document.getElementById("balance").innerText = balance.toFixed(2);
            if (clicks >= 3) document.getElementById("adButton").style.display = "inline-block";
        });

        document.getElementById("adButton").addEventListener("click", function() {
            let adReward = Math.random() < 0.5 ? (Math.random() * 2 + 1).toFixed(2) : Math.floor(Math.random() * 991) + 10;
            if (typeof adReward === "string") {
                balance += parseFloat(adReward);
            } else {
                coins += adReward;
                document.getElementById("coins").innerText = coins;
            }
            document.getElementById("balance").innerText = balance.toFixed(2);
        });

        document.getElementById("convertButton").addEventListener("click", function() {
            if (coins >= 10000) {
                let cashToAdd = Math.floor(coins / 10000) * 0.01;
                coins %= 10000;
                document.getElementById("coins").innerText = coins;
                balance += cashToAdd;
                document.getElementById("balance").innerText = balance.toFixed(2);
                if (balance >= 0.30) document.getElementById("cashoutSection").style.display = "block";
            }
        });

        document.getElementById("cashoutButton").addEventListener("click", function() {
            let email = document.getElementById("paypalEmail").value;
            if (email && balance >= 0.30) {
                alert("Paid out £" + balance.toFixed(2) + " to " + email);
                balance = 0;
                document.getElementById("balance").innerText = "0.00";
                document.getElementById("cashoutSection").style.display = "none";
            } else {
                alert("Invalid email or insufficient funds.");
            }
        });
    </script>

</body>
</html>


<!DOCTYPE html>
<html lang="km">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>គណនាថ្លៃផ្ញើរយានជំនិះ</title>
    <style>
        .box {
            border: 2px solid #c62828;
            padding: 25px;
            width: 380px;
            border-radius: 6px;
        }
        h2 { text-align: center; color: #222; }

        select, input {
            width: 200px;
            padding: 12px;
            font-size: 16px;
            border: 1.5px solid #222;
            border-radius: 10px;
        }
        button {
            width: 100%;
            padding: 14px;
            font-size: 18px;
            color: #222;
            border: 2px solid #c62828;
            border-radius: 10px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div class="box">
        <h2>គណនាថ្លៃផ្ញើរយានជំនិះ</h2>

        <div class="field">
            <label for="vehicle">ជ្រើសរើស យានជំនិះ:</label>
            <select id="vehicle">
                <option value="car">ឡាន</option>
                <option value="moto">ម៉ូតូ</option>
            </select>
        </div>

        <div class="field">
            <label for="timeIn">ម៉ោងចូល</label>
            <input type="time" id="timeIn">
        </div>

        <div class="field">
            <label for="timeOut">ម៉ោងចេញ</label>
            <input type="time" id="timeOut">
        </div>

        <div class="field">
            <label for="riel">ថ្លៃផ្ញើរ រៀល</label>
            <input type="text" id="riel" placeholder="៛" readonly>
        </div>

        <div class="field">
            <label for="dollar">ថ្លៃផ្ញើរ ដុល្លា</label>
            <input type="text" id="dollar" placeholder="$" readonly>
        </div>

        <button type="button" onclick="calculate()">គណនា</button>
    </div>

    <div class="box">
        <p>ឡាន</p>
        <p style="margin-left:30px">+ ប្រសិនបើផ្ញើរ 8h ⇒ 1h = 500៛<br>
        + តែបើលើស 8h ⇒ 1h = 1000៛</p>
        <p>ម៉ូតូ</p>
        <p style="margin-left:30px">+ ប្រសិនបើផ្ញើរ 8h ⇒ 1h = 200៛<br>
        + តែបើលើស 8h ⇒ 1h = 300៛</p>
    </div>

    <script>
        const EXCHANGE_RATE = 4000; // 1 dollar = 4000 riel

        const rates = {
            car:  { normal: 500, over: 1000 },
            moto: { normal: 200, over: 300 }
        };

        function calculate() {
            let vehicle = document.getElementById('vehicle').value;
            let timeIn = document.getElementById('timeIn').value;
            let timeOut = document.getElementById('timeOut').value;

            if (timeIn === '' || timeOut === '') {
                alert('Please enter time in and time out');
                return;
            }

            let [inH, inM] = timeIn.split(':').map(Number);
            let [outH, outM] = timeOut.split(':').map(Number);
            let diff = (outH * 60 + outM) - (inH * 60 + inM);

            if (diff < 0) {
                diff += 24 * 60; // parked overnight
            }
            if (diff === 0) {
                alert('Time in and time out are the same');
                return;
            }

            let hours = Math.ceil(diff / 60);
            let normalHours = Math.min(hours, 8);
            let overHours = Math.max(hours - 8, 0);

            let rate = rates[vehicle];
            let totalRiel = normalHours * rate.normal + overHours * rate.over;
            let totalDollar = totalRiel / EXCHANGE_RATE;

            document.getElementById('riel').value = totalRiel.toLocaleString() + ' ៛';
            document.getElementById('dollar').value = totalDollar.toFixed(2) + ' $';
        }
    </script>
</body>
</html>

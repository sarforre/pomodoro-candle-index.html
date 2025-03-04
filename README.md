from IPython.display import HTML

html_code = """
<html>
<head>
  <style>
    /* Timer Display and Buttons */
    #timer-display {
      font-size: 2em;
      margin-bottom: 1em;
      font-family: sans-serif;
      text-align: center;
    }
    .timer-button {
      font-size: 1em;
      padding: 0.5em 1em;
      margin-right: 0.5em;
      cursor: pointer;
    }

    /* Candle Container */
    #candle-container {
      display: flex;
      justify-content: center;
      margin-top: 2em;
    }
    /* Candle Body */
    #candle {
      position: relative;
      width: 50px;
      height: 200px;
      background-color: #d3d3d3; /* Candle outline color */
      border-radius: 10px;
      box-shadow: inset 0 0 5px #999;
    }

    /* Candle Wax (the portion that burns down) */
    #wax {
      position: absolute;
      bottom: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-color: #ffe4c4;
      border-radius: 10px;
      transition: height 1s linear;
    }

    /* Wick — a narrow black rectangle above the candle */
    #wick {
      position: absolute;
      bottom: 100%;
      left: 50%;
      transform: translateX(-50%);
      width: 2px;
      height: 20px;
      background-color: black;
    }

    /* Flame glow at the top of the wick */
    #wick::after {
      content: '';
      position: absolute;
      bottom: 100%;
      left: 50%;
      transform: translateX(-50%);
      width: 10px;
      height: 10px;
      background: radial-gradient(circle, yellow 40%, orange 70%, transparent 80%);
      border-radius: 50%;
      animation: flicker 0.3s infinite alternate;
    }

    /* Keyframes to create a subtle flicker effect */
    @keyframes flicker {
      0% {
        transform: translateX(-50%) scale(1);
        opacity: 1;
      }
      100% {
        transform: translateX(-50%) scale(0.9);
        opacity: 0.8;
      }
    }

  </style>
</head>
<body>

  <div id="timer-display">25:00</div>
  <button class="timer-button" onclick="startTimer()">Start</button>
  <button class="timer-button" onclick="stopTimer()">Stop</button>

  <!-- Candle Markup -->
  <div id="candle-container">
    <div id="candle">
      <div id="wax"></div>
      <div id="wick"></div>
    </div>
  </div>

  <script>
    var totalSeconds = 25 * 60;
    var remainingSeconds = totalSeconds;
    var timerId = null;

    function updateDisplay() {
      let mins = Math.floor(remainingSeconds / 60);
      let secs = remainingSeconds % 60;
      let displayString =
         (mins < 10 ? '0' : '') + mins + ':' +
         (secs < 10 ? '0' : '') + secs;
      document.getElementById('timer-display').textContent = displayString;
    }

    function updateCandle() {
      // Fraction of candle that remains
      var fraction = remainingSeconds / totalSeconds;
      // Convert fraction to a percentage for the wax height
      var waxHeight = fraction * 100;
      document.getElementById('wax').style.height = waxHeight + '%';
    }

    function tick() {
      if (remainingSeconds > 0) {
        remainingSeconds--;
        updateDisplay();
        updateCandle();
      } else {
        clearInterval(timerId);
        timerId = null;
        document.getElementById('timer-display').textContent = "Time's up!";
        // Candle is fully burned
        document.getElementById('wax').style.height = '0%';
      }
    }

    function startTimer() {
      stopTimer(); // ensure no other timer is running
      remainingSeconds = totalSeconds; // reset to 25:00 each start
      updateDisplay();
      updateCandle();
      timerId = setInterval(tick, 1000);
    }

    function stopTimer() {
      if (timerId) {
        clearInterval(timerId);
        timerId = null;
      }
    }

    // Initialize display and candle on page load
    updateDisplay();
    updateCandle();
  </script>
</body>
</html>
"""

HTML(html_code)

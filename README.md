<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Modern Stopwatch</title>

<style>
*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family:'Segoe UI',sans-serif;
}

body{
    min-height:100vh;
    display:flex;
    justify-content:center;
    align-items:center;
    background:linear-gradient(135deg,#0f172a,#1e293b,#334155);
    transition:0.4s ease;
}

.container{
    width:400px;
    padding:30px;
    border-radius:25px;
    backdrop-filter:blur(15px);
    background:rgba(255,255,255,0.08);
    border:1px solid rgba(255,255,255,0.1);
    box-shadow:0 8px 32px rgba(0,0,0,0.3);
    text-align:center;
    color:white;
    transition:0.4s ease;
}

h1{
    margin-bottom:20px;
    font-size:2rem;
}

.time{
    font-size:3rem;
    font-weight:bold;
    margin:20px 0;
    letter-spacing:2px;
}

.buttons{
    display:flex;
    justify-content:center;
    gap:10px;
    flex-wrap:wrap;
    margin-top:20px;
}

button{
    padding:12px 20px;
    border:none;
    border-radius:12px;
    cursor:pointer;
    font-size:1rem;
    font-weight:600;
    transition:0.3s;
}

button:hover{
    transform:translateY(-3px);
}

.start{
    background:#22c55e;
    color:white;
}

.pause{
    background:#f59e0b;
    color:white;
}

.reset{
    background:#ef4444;
    color:white;
}

.lap{
    background:#3b82f6;
    color:white;
}

.theme{
    background:#8b5cf6;
    color:white;
}

.laps{
    margin-top:25px;
    text-align:left;
    max-height:220px;
    overflow-y:auto;
}

.laps h3{
    margin-bottom:10px;
}

.lap-item{
    background:rgba(255,255,255,0.08);
    padding:10px;
    border-radius:10px;
    margin-bottom:8px;
}

/* LIGHT MODE */

body.light{
    background:linear-gradient(135deg,#e2e8f0,#f8fafc,#ffffff);
}

body.light .container{
    background:rgba(255,255,255,0.85);
    color:#111827;
    border:1px solid rgba(0,0,0,0.1);
}

body.light .lap-item{
    background:rgba(0,0,0,0.05);
}

body.light h1,
body.light .time,
body.light .laps h3{
    color:#111827;
}

::-webkit-scrollbar{
    width:5px;
}

::-webkit-scrollbar-thumb{
    background:#64748b;
    border-radius:10px;
}

@media(max-width:450px){

.container{
    width:95%;
}

.time{
    font-size:2rem;
}

button{
    width:100%;
}

}
</style>
</head>

<body>

<div class="container">

    <h1>⏱ Stopwatch</h1>

    <div class="time" id="display">
        00:00:00:000
    </div>

    <div class="buttons">

        <button class="start" onclick="startStopwatch()">
            ▶ Start
        </button>

        <button class="pause" onclick="pauseStopwatch()">
            ⏸ Pause
        </button>

        <button class="lap" onclick="recordLap()">
            🏁 Lap
        </button>

        <button class="reset" onclick="resetStopwatch()">
            🔄 Reset
        </button>

        <button class="theme" onclick="toggleTheme()">
            🌙 Light
        </button>

    </div>

    <div class="laps">
        <h3>Lap Records</h3>
        <div id="lapContainer"></div>
    </div>

</div>

<script>

let startTime;
let elapsedTime = 0;
let timerInterval;
let running = false;
let lapCount = 1;

function formatTime(ms){

    let milliseconds = ms % 1000;
    let seconds = Math.floor(ms / 1000) % 60;
    let minutes = Math.floor(ms / (1000 * 60)) % 60;
    let hours = Math.floor(ms / (1000 * 60 * 60));

    return (
        String(hours).padStart(2,'0') + ":" +
        String(minutes).padStart(2,'0') + ":" +
        String(seconds).padStart(2,'0') + ":" +
        String(milliseconds).padStart(3,'0')
    );
}

function updateDisplay(){

    document.getElementById("display").textContent =
    formatTime(Date.now() - startTime + elapsedTime);

}

function startStopwatch(){

    if(!running){

        startTime = Date.now();

        timerInterval =
        setInterval(updateDisplay,10);

        running = true;
    }
}

function pauseStopwatch(){

    if(running){

        clearInterval(timerInterval);

        elapsedTime +=
        Date.now() - startTime;

        running = false;
    }
}

function resetStopwatch(){

    clearInterval(timerInterval);

    running = false;
    elapsedTime = 0;
    lapCount = 1;

    document.getElementById("display")
    .textContent = "00:00:00:000";

    document.getElementById("lapContainer")
    .innerHTML = "";
}

function recordLap(){

    if(!running) return;

    const currentTime =
    formatTime(Date.now() - startTime + elapsedTime);

    const lap =
    document.createElement("div");

    lap.classList.add("lap-item");

    lap.innerHTML =
    `<strong>Lap ${lapCount}</strong> — ${currentTime}`;

    document.getElementById("lapContainer")
    .prepend(lap);

    lapCount++;
}

/* THEME TOGGLE */

function toggleTheme(){

    document.body.classList.toggle("light");

    const isLight =
    document.body.classList.contains("light");

    localStorage.setItem(
        "theme",
        isLight ? "light" : "dark"
    );

    document.querySelector(".theme").innerHTML =
    isLight ? "☀ Dark" : "🌙 Light";
}

/* LOAD SAVED THEME */

window.onload = () => {

    const savedTheme =
    localStorage.getItem("theme");

    if(savedTheme === "light"){

        document.body.classList.add("light");

        document.querySelector(".theme").innerHTML =
        "☀ Dark";
    }
};

</script>

</body>
</html>

<script>

	import logo from './assets/HabitZup.png';

  let max_time = 180;
  let player1Time = max_time; // 5 minutes in seconds
  let player2Time = max_time;
  let player3Time = max_time; // 5 minutes in seconds
  let player4Time = max_time;
  let activePlayer = null; // "player1" or "player2"
  let direction = "forward";
  let timer;
  let ended = false;
  let paused = false;
  let started = false;
  let points = {}

  function formatTime(seconds) {
    const m = Math.floor(seconds / 60);
    const s = seconds % 60;
    return `${m}:${s < 10 ? '0' : ''}${s}`;
  }

  function startTimer() {
    ended = false;
    started = true;
    paused = false;
    if (!activePlayer) activePlayer = 'player1';
    clearInterval(timer);
    timer = setInterval(() => {
      if (activePlayer === 'player1' && player1Time > 0) {
        player1Time--;
        if (player1Time === 0) stopTimer();
      } else if (activePlayer === 'player2' && player2Time > 0) {
        player2Time--;
        if (player2Time === 0) stopTimer();
      }
      else if (activePlayer === 'player3' && player3Time > 0) {
        player3Time--;
        if (player3Time === 0) stopTimer();
      } else if (activePlayer === 'player4' && player4Time > 0) {
        player4Time--;
        if (player4Time === 0) stopTimer();
      }
    }, 1000);
  }

  function switchTurn() {
    if (direction === 'forward') {
    if (activePlayer === 'player1') activePlayer = 'player2';
    else if (activePlayer === 'player2') activePlayer = 'player3';
    else if (activePlayer === 'player3') activePlayer = 'player4';
    else if (activePlayer === 'player4') activePlayer = 'player1';
    }
    else if (direction === 'reverse') {
      if (activePlayer === 'player1') activePlayer = 'player4';
      else if (activePlayer === 'player2') activePlayer = 'player1';
      else if (activePlayer === 'player3') activePlayer = 'player2';
      else if (activePlayer === 'player4') activePlayer = 'player3';
    }
  }

  function reverseTurn() {
    if (direction === 'forward') {
      direction = 'reverse';
    }else {
      direction = 'forward';
    }
  }

  function stopTimer() {
    clearInterval(timer);
    ended = true;
    started = false;
    paused = false;
    activePlayer = null;
  }

  function pauseTimer() {
    clearInterval(timer);
    paused = true;
  }
  function resumeTimer() {
    paused = false;
    startTimer();
  }

  function resetTimers() {

    stopTimer();
    ended = true;
    started = false;
    paused = false;

    displayPoints();
    
    player1Time = max_time;
    player2Time = max_time;
    player3Time = max_time;
    player4Time = max_time;
  }

  function displayPoints() {
    points = {
      player1: player1Time/10,
      player2: player2Time/10,
      player3: player3Time/10,
      player4: player4Time/10
    };
    console.log(points);
  }


    function gameEnded() {
        ended = true;
    }
    function isEnded() {
        return ended;
    }

</script>

<main class="app">
  <h1>  <img width="50" height="50" alt="The project logo" src={logo} />
    HabitZup Timer </h1>

  {#if ended}
   <h2> Game Over </h2>
{/if}
  

  <div class="controls">
    {#if paused === false && started === false}
      <button on:click={startTimer}> 🟢 Start Game</button>
    {/if}
    {#if paused === true && started === true}
      <button on:click={resumeTimer}> ▶️ Resume</button>
    {/if}
    {#if paused === false && started === true}
      <button on:click={pauseTimer}> ⏸️ Pause</button>
    {/if}
    <button disabled='{isEnded()}' on:click={switchTurn}> 🚀 Pass Turn</button>
  </div>

  <div class="timers">

    {#if player1Time < 10}
    <div class="timer player1" class:red={activePlayer === 'player1'}>
      <h1>🐶</h1>
      <h2>Player 1</h2>

      <h1>{formatTime(player1Time)}</h1>
    </div>
    {:else}
    <div class="timer player1" class:active={activePlayer === 'player1'}>
      <h1>🐶</h1>
      <h2>Player 1</h2>

      <h1>{formatTime(player1Time)}</h1>
    </div>
    {/if}

  
    {#if player2Time < 10}
    <div class="timer player2" class:red={activePlayer === 'player2'}>
      <h1>🦁</h1>
      <h2>Player 1</h2>

      <h1>{formatTime(player2Time)}</h1>
    </div>
    {:else}
    <div class="timer player2" class:active={activePlayer === 'player2'}>
      <h1>🦁</h1>
      <h2>Player 2</h2>

      <h1>{formatTime(player2Time)}</h1>
    </div>
    {/if}

    {#if player3Time < 10}
    <div class="timer player3" class:red={activePlayer === 'player3'}>
      <h1>🦄</h1>
      <h2>Player 1</h2>

      <h1>{formatTime(player3Time)}</h1>
    </div>
    {:else}
    <div class="timer player3" class:active={activePlayer === 'player3'}>
      <h1>🦄</h1>
      <h2>Player 3</h2>

      <h1>{formatTime(player3Time)}</h1>
    </div>
    {/if}

    {#if player4Time < 10}
    <div class="timer player4" class:red={activePlayer === 'player4'}>
      <h1>🐹</h1>
      <h2>Player 4</h2>

      <h1>{formatTime(player4Time)}</h1>
    </div>
    {:else}
    <div class="timer player4" class:active={activePlayer === 'player4'}>
      <h1>🐹</h1>
      <h2>Player 4</h2>

      <h1>{formatTime(player4Time)}</h1>
    </div>
    {/if}

  </div>
  
  <div class="controls">


    <button on:click={reverseTurn}>◀️ Reverse</button>
    <button  on:click={resetTimers}>🛑 End Game</button>

  </div>


  {#if ended}
  <div class="controls">
    <div >
      <h1>🐶</h1>
      <h2>Player 1</h2>
      <h1>{points.player1}</h1>
    </div>
    <div >
      <h1>🦁 </h1>
      <h2>Player 2</h2>
      <h1>{points.player2}</h1>
    </div>
    <div >
      <h1>🐱 </h1>
      <h2> Player 3</h2>
      <h1>{points.player3}</h1>
    </div>
    <div >
      <h1>🦄 </h1>
      <h2>Player 4</h2>
      <h1>{points.player4}</h1>
    </div>
    </div>
    {/if}
    
  
</main>

<style>
  .app {
    font-family: 'Arial', sans-serif;
    text-align: center;
    padding: 2rem;
    background: #f5f7fa;
    color: #333;
  }
  .timers {
    display: flex;
    justify-content: space-around;
    margin: 2rem 0;
  }
  .timer {
    padding: 1rem 2rem;
    border-radius: 10px;
    background: #ddd;
    width: 150px;
    transition: background 0.3s;
  }
  .timer.active {
    background: purple;
    color: white;
  }

  .timer.red {
    background: orange;
    color: white;
  }

  .controls button {
    margin: 0.5rem;
    padding: 0.7rem 1.2rem;
    font-size: 1rem;
    border: none;
    border-radius: 6px;
    background: #219ebc;
    color: white;
    cursor: pointer;
  }

  .rcontrols button {
    margin: 0.5rem;
    padding: 0.7rem 1.2rem;
    font-size: 1rem;
    border: none;
    border-radius: 6px;
    background: red;
    color: white;
    cursor: pointer;
  }

  .controls button:hover {
    background: #126782;
  }
</style>

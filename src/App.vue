<template>
  <header>
    <h1>Monster Slayer</h1>
  </header>
  <section id="monster" class="container">
    <h2>Monster Health</h2>
    <div class="healthbar">
      <div class="healthbar__value"></div>
    </div>
  </section>
  <section id="monster" class="container">
    <h2>Your Health</h2>
    <div class="healthbar">
      <div class="healthbar__value"></div>
    </div>
  </section>
  <section id="controls">
    <button>ATTACK</button>
    <button>SPECIAL ATTACK</button>
    <button>HEAL</button>
    <button>SURRENDER</button>
  </section>
  <section id="log" class="container">
    <h2>Battle Log</h2>
    <ul></ul>
  </section>
</template>

<script>
</script>

<style scoped>
header {
  display: flex;
  justify-content: center;
  align-items: center;
  background: darkred;
}

h1 {
  color: whitesmoke;
  font-size: 2.5rem;
}

.container {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 2em;
  border-radius: 0.75em;
  background-color: whitesmoke;
}

section {
  width: 85%;
  max-width: 85%;
  margin: 1em auto;
}

.healthbar {
  width: 100%;
  height: 40px;
  border: 1px solid #575757;
  margin: 1rem 0;
  background: #fde5e5;
}

.healthbar__value {
  background-color: #00a876;
  width: 100%;
  height: 100%;
}

#monster h2,
#player h2 {
  margin: 0.25rem;
  width: 30%;
}

#controls {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
}

button {
  font: inherit;
  border: 1px solid #88005b;
  background-color: #88005b;
  color: white;
  padding: 1rem 2rem;
  border-radius: 12px;
  margin: 1rem;
  width: 12rem;
  cursor: pointer;
  box-shadow: 1px 1px 4px rgba(0, 0, 0, 0.26);
}

button:focus {
  outline: none;
}

button:hover,
button:active {
  background-color: #af0a78;
  border-color: #af0a78;
  box-shadow: 1px 1px 8px rgba(0, 0, 0, 0.26);
}

button:disabled {
  background-color: #ccc;
  border-color: #ccc;
  box-shadow: none;
  color: #3f3f3f;
  cursor: not-allowed;
}

#log ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

#log li {
  margin: 0.5rem 0;
}

.log--player {
  color: #7700ff;
}

.log--monster {
  color: #da8d00;
}

.log--damage {
  color: red;
}

.log--heal {
  color: green;
}
</style>
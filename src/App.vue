<template>
  <header>
    <h1>Monster Slayer</h1>
  </header>
  <section id="monster" class="container">
    <health-bar
      name="monster"
      :monsterHealth="monsterHealth"
      :styleClass="monsterBarStyles"
    />
  </section>
  <section id="player" class="container">
    <health-bar
      name="your"
      :monsterHealth="playerHealth"
      :styleClass="playerBarStyles"
    />
  </section>
  <section v-if="winner" class="container flex-wrap">
    <h2 class="w-100 text-center">Game Over!</h2>
    <h3 v-if="winner === 'player'">You Win!</h3>
    <h3 v-else-if="winner === 'monster'">You Lose!</h3>
    <h3 v-else>It's a draw.</h3>
    <button @click="startGame" class="w-100">Start New Game</button>
  </section>
  <section id="controls" v-else>
    <button @click="attackMonster">ATTACK</button>
    <button @click="specialAttackMonster" :disabled="mayUseSpecialAttack">
      SPECIAL ATTACK
    </button>
    <button @click="healPlayer">HEAL</button>
    <button @click="surrender">SURRENDER</button>
  </section>
  <section id="log" class="container flex-wrap">
    <h2 class="w-100 text-center">Battle Log</h2>
    <ul>
      <li v-for="(logMsg, i) in logMessages" :key="i">
        <span
          :class="{
            'log--player': logMsg.actionBy === 'player',
            'log--monster': logMsg.actionBy === 'monster',
          }"
          >{{ logMsg.actionBy === "player" ? "Player " : "Monster " }}</span
        >
        <span v-if="logMsg.actionType === 'heal'"
          >healed by
          <span class="log--heal">{{ logMsg.actionValue }}</span> points</span
        >
        <span v-else
          >attacks and deals
          <span class="log--damage">{{ logMsg.actionValue }}</span> damage</span
        >
      </li>
    </ul>
  </section>
</template>

<script>
import HealthBar from "./components/HealthBar.vue";
function getRandomValue(max, min) {
  return Math.floor(Math.random() * (max - min)) + min;
}
export default {
  components: {
    HealthBar,
  },
  data() {
    return {
      playerHealth: 100,
      monsterHealth: 100,
      currentRound: 0,
      // If the current round is divisible by 3 only then enable Special Attack Button
      winnner: null,
      logMessages: [],
    };
  },
  computed: {
    monsterBarStyles() {
      if (this.monsterHealth <= 0) {
        return { width: "0%" };
      }
      return { width: this.monsterHealth + "%" };
    },
    playerBarStyles() {
      if (this.playerHealth <= 0) {
        return { width: "0%" };
      }
      return { width: this.playerHealth + "%" };
    },
    mayUseSpecialAttack() {
      return this.currentRound % 3 !== 0;
    },
  },
  watch: {
    playerHealth(currValue) {
      if (currValue <= 0 && this.monsterHealth <= 0) {
        // It's a Draw
        this.winnner = "draw";
      } else if (currValue <= 0) {
        // Player lost
        this.winnner = "monster";
      }
    },
    monsterHealth(currValue) {
      if (currValue <= 0 && this.playerHealth <= 0) {
        // It's a draw
        this.winnner = "draw";
      } else if (currValue <= 0) {
        // Monster Lost
        this.winner = "player";
      }
    },
  },
  methods: {
    startGame() {
      this.playerHealth = 100;
      this.monsterHealth = 100;
      this.counter = 0;
      this.winner = null;
      this.logMessages = [];
    },
    attackMonster() {
      this.currentRound++;
      const attackValue = getRandomValue(12, 5);
      this.monsterHealth -= attackValue;
      this.addLogMessage("player", "attack", attackValue);
      // When we attack the monster, the monster attacks back
      this.attackPlayer();
    },
    attackPlayer() {
      const attackValue = getRandomValue(15, 8);
      this.playerHealth -= attackValue;
      this.addLogMessage("monster", "attack", attackValue);
    },
    specialAttackMonster() {
      this.currentRound++;
      const attackValue = getRandomValue(10, 25);
      this.monsterHealth -= attackValue;
      this.addLogMessage("player", "attack", attackValue);
      this.attackPlayer();
    },
    healPlayer() {
      this.currentRound++;
      const healValue = getRandomValue(8, 20);
      this.playerHealth + healValue > 100
        ? (this.playerHealth = 100)
        : (this.playerHealth += healValue);
      this.addLogMessage("player", "heal", healValue);
      // When we heal ourself, the monster attacks back
      this.attackPlayer();
    },
    surrender() {
      this.winner = "monster";
    },
    addLogMessage(who, what, value) {
      this.logMessages.unshift({
        actionBy: who,
        actionType: what,
        actionValue: value,
      });
    },
  },
};
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

.w-100 {
  width: 100%;
}

.text-center {
  text-align: center;
}

.flex-wrap {
  flex-wrap: wrap;
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
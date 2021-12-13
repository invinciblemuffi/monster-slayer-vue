<template>
  <header>
    <h1>Monster Slayer</h1>
  </header>
  <PlayerCards />
</template>

<script>
import PlayerCards from "./components/PlayerCards.vue";
export default {
  components: PlayerCards,
};
</script>

<style scoped>
header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 5em 10em;
  background: darkblue;
}

h1 {
  color: whitesmoke;
  font-size: 2.5rem;
}
</style>
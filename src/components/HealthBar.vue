<template>
  <h2>{{ capitalizeName }} Health</h2>
  <div class="healthbar">
    <div class="healthbar__value" :style="styleClass">
      {{ monsterHealth }}
    </div>
  </div>
</template>

<script>
export default {
  name: "HealthBar",
  props: {
    monsterHealth: { type: Number, required: true },
    name: { type: String, required: true },
    styleClass: Object,
  },
  computed: {
    capitalizeName() {
      return this.name[0].toUpperCase() + this.name.slice(1);
    },
  },
};
</script>

<style scoped>
h2 {
  margin: 0.25rem;
  width: 30%;
}

.healthbar {
  width: 100%;
  height: 40px;
  border: 1px solid gray;
  margin: 1rem 0;
  background: lightpink;
}

.healthbar__value {
  background-color: darkgreen;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-size: 1.5em;
}
</style>
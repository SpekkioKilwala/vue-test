<script setup lang="ts">
import { ref } from 'vue'
import { DiceRoll } from '@dice-roller/rpg-dice-roller'
// docs: https://www.npmjs.com/package/@dice-roller/rpg-dice-roller

const result = ref(0)

function newRoll() {
  const roll = new DiceRoll('2d6');

  result.value = roll.total;
}

</script>

<template>
  <div class="roller">
      <button @click="newRoll">Roll them bones</button>
      Result: {{ result }}
  </div>
</template>

<style scoped>

</style>
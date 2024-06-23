<script setup lang="ts">
import { ref } from 'vue'
import { DiceRoll } from '@dice-roller/rpg-dice-roller'
import { default as EloRank } from 'elo-rank'

// docs: https://www.npmjs.com/package/@dice-roller/rpg-dice-roller

const result = ref(0)

function newRoll() {
  const roll = new DiceRoll('2d6');
  result.value = roll.total;
}

const elo = new EloRank(32);
let playerA = 1200;
let playerB = 1400;

let expectedScoreA = elo.getExpected(playerA, playerB);
let expectedScoreB = elo.getExpected(playerB, playerA);
 
//update score, 1 if won 0 if lost
playerA = elo.updateRating(expectedScoreA, 1, playerA);
playerB = elo.updateRating(expectedScoreB, 0, playerB);

console.log(`Scores of players A, B: ${playerA}, ${playerB}`)

</script>

<template>
  <div class="roller">
    <button @click="newRoll">Roll them bones</button>
    Result: {{ result }}
  </div>
</template>

<style scoped>

</style>
<script setup lang="ts">
import { ref, reactive } from 'vue'
import { DiceRoll } from '@dice-roller/rpg-dice-roller'
import { default as EloRank } from 'elo-rank'

// docs: https://www.npmjs.com/package/@dice-roller/rpg-dice-roller

const resultA = ref(0)
const resultB = ref(0)

const elo = new EloRank(32);
const playerA = ref(1080); // you
const playerB = ref(1000); // infinite supply of Computer players

const eloHistory = reactive([playerA.value])
const average = array => array.reduce((a: number, b: number) => a + b) / array.length;

// A single function should not oversee the rolling and the 0-1 rescaling
// TODO: split it back up
function contest(rollA: string, rollB: string) {
  // accepting two dice roll inputs ('2d6' and '2d6+2')
  // and returns which one won
  // 1 if A won, 0.5 if a draw, and 0 if B won.
  resultA.value = new DiceRoll(rollA).total
  resultB.value = new DiceRoll(rollB).total
  if (resultA.value > resultB.value) {
    return 1
  }
  else if (resultA.value == resultB.value) {
    return 0.5
  }
  else {
    return 0
  }
}

function play10k() {
  for(let i = 0; i < 10000; i++) {
    playRound()
  }
}

function playRound() {
  const contestOutcome = contest('2d6+2', '2d6')

  let expectedScoreA = elo.getExpected(playerA.value, playerB.value);
  let expectedScoreB = elo.getExpected(playerB.value, playerA.value);

  //update score, 1 if won 0 if lost
  playerA.value = elo.updateRating(expectedScoreA, contestOutcome, playerA.value);
  // playerB = elo.updateRating(expectedScoreB, 0, playerB);

  eloHistory.push(playerA.value)
}


</script>

<template>
  <div class="roller">
    <button @click="playRound">Roll them bones</button>
    <button @click="play10k">Roll them bones 10,000 times</button>
    <div>Combat summary: You got {{ resultA }}, the computer got {{ resultB }}</div>
    <div>Your new score: {{ playerA }}</div>
    <div>Last 5 scores: {{ eloHistory.slice(-5) }}</div>
    <div>Average score: {{ average(eloHistory) }}</div>
  </div>
</template>

<style scoped>

</style>
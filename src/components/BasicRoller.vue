<script setup lang="ts">
import { ref } from 'vue'

const count = ref(0)

function increment() {
  count.value++
}

</script>

<template>
  <div class="roller">
      <button @click="increment">Count is: {{ count }}</button>
  </div>
</template>

<style scoped>

</style>
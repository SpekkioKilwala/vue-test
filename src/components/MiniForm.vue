<script setup lang="ts">
import { ref } from 'vue'

const count = ref(0)

function inc() {
  count.value++
}

</script>

<template>
  <div>
    <button type="button" @click="inc">Press me</button>
  </div>
  <div>
    Count: {{ count }}
  </div>
</template>

<style scoped>

</style>
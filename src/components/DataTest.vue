<script setup lang="ts">
  import { ref } from 'vue';

  const count = ref(0);

</script>

<template>
  <div>
    Testing testing
    <button type="button" @click="count++">Count: {{ count }}</button>
  </div>
</template>

<style scoped>

</style>
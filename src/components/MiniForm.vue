<script setup lang="ts">
import { ref } from 'vue'
import axios from 'axios';

const responseDisplay = ref("no response yet")

function postThing() {
  axios.post('https://httpbin.org/post', {
    firstName: 'Howell',
    lastName: 'Jenkins'
  })
  .then(function (response) {
    console.log(response)
    responseDisplay.value = JSON.stringify(response.data)
  })
  .catch(function (error) {
    console.log(error)
    responseDisplay.value = JSON.stringify(error.data)
  })
}

</script>

<template>
  <div>
    <button type="button" @click="postThing">Press me</button>
  </div>
  {{ responseDisplay }}
</template>

<style scoped>

</style>
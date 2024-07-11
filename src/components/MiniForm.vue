<script setup lang="ts">
import { ref } from 'vue'
import axios from 'axios';

const responseDisplay = ref("no response yet")

const _axios = axios.create({
  baseURL: 'https://httpbin.org/',
  headers: {'X-Custom-Header': 'custom header payload!'}
})

function postThing() {
  _axios.post('/post', {
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
  <div>{{ responseDisplay }}</div>
  <div><input type="text"></div>
</template>

<style scoped>

</style>
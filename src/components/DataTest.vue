<script setup lang="ts">
  import { ref } from 'vue';
  import * as Realm from "realm-web";

  const { MODE, PROD, DEV, SSR, BASE_URL, VITE_DATA_PASS } = import.meta.env;

  const token = ref("unset");

const app = new Realm.App({ id: 'data-yqnwfye' });
async function loginEmailPassword(email:string, password:string) {
  // Create an email/password credential
  const credentials = Realm.Credentials.emailPassword(email, password);
  // Authenticate the user
  const user = await app.logIn(credentials);
  // 'App.currentUser' updates to match the logged in user
  console.assert(user.id === app.currentUser.id);
  console.log(user)
  return user;
}

async function getToken() {
  const user = await loginEmailPassword('jonmwalsh@gmail.com', VITE_DATA_PASS);
  return user.accessToken;
}

</script>

<template>
  <div>
    Testing testing
    <button type="button" @click="() => {token = getToken() || 'waiting'}">Get token?</button>
    {{ token }}
  </div>
</template>

<style scoped>

</style>
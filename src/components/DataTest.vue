<script setup lang="ts">
  import { ref } from 'vue';
  import * as Realm from "realm-web";
  import axios from 'axios';

  // https://www.mongodb.com/docs/atlas/device-sdks/web/mongodb/#std-label-web-query-mongodb
  const {
    BSON: { ObjectId },
  } = Realm;

  const { MODE, PROD, DEV, SSR, BASE_URL, VITE_DATA_PASS } = import.meta.env;

  const token = ref("unset");
  const res = ref("nothing yet");

const app = new Realm.App({ id: 'data-yqnwfye' });
async function loginEmailPassword(email:string, password:string) {
  // Create an email/password credential
  const credentials = Realm.Credentials.emailPassword(email, password);
  // Authenticate the user
  const user = await app.logIn(credentials);
  // 'App.currentUser' updates to match the logged in user
  console.assert(user.id === app.currentUser!.id);
  console.log(user)
  return user;
}

async function getToken() {
  token.value = 'waiting...';
  const user = await loginEmailPassword('jonmwalsh@gmail.com', VITE_DATA_PASS);
  token.value = user.accessToken || "null";
  return user.accessToken;
}

var data = JSON.stringify({
    "collection": "listingsAndReviews",
    "database": "sample_airbnb",
    "dataSource": "clusterFuffle",
    "projection": {
        "_id": 1
    }
});
var config = {
    method: 'post',
    url: 'https://ap-southeast-2.aws.data.mongodb-api.com/app/data-yqnwfye/endpoint/data/v1/action/findOne',
    headers: {
      'Content-Type': 'application/json',
      'Access-Control-Request-Headers': '*',
      'Authorization': `Bearer <${ token.value }>`,
    },
    data: data
};

async function firePost() {
  axios(config)
    .then(function (response) {
        res.value = JSON.stringify(response.data);
        console.log(res.value);
    })
    .catch(function (error) {
        console.log(error);
    });
}



</script>

<template>
  <div>
    <button type="button" @click="getToken">Get token?</button>
    {{ token }}
  </div>
  <div>
    <button type="button" @click="firePost">This will be fine</button>
    {{ res }}
  </div>
</template>

<style scoped>

</style>
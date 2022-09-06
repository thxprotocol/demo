<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <p>{{ client.session.cached.user }}</p>
  <HelloWorld msg="Welcome to Your Vue.js App" />
</template>

<script>
import { THXClient } from "@thxprotocol/sdk";
import HelloWorld from "./components/HelloWorld.vue";

const client = new THXClient({
  clientId: process.env["VUE_APP_CLIENT_ID"],
  clientSecret: process.env["VUE_APP_CLIENT_SECRET"],
  grantType: "client_credentials",
  redirectUrl: "http://localhost:8080/signin-oidc",
});

export default {
  name: "App",
  components: {
    HelloWorld,
  },
  data() {
    return {
      client,
    };
  },

  async mounted() {
    const authenticated = await client.init();
    if (!authenticated) await client.signin();
    else console.log(await client.account.get());
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

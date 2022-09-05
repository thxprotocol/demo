<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <p>{{ client.session.cached.user }}</p>
  <HelloWorld msg="Welcome to Your Vue.js App" />
</template>

<script>
import { THXClient } from "@thxprotocol/sdk";
import HelloWorld from "./components/HelloWorld.vue";

const client = new THXClient({
  clientId: "2M07qh8c16Eu_e7f78md5",
  clientSecret:
    "m301KSeyeRFwRHxRfOnYMFD6vFDL5pndfT96XEgASVU6MWM40xM48HqSnRDM8xuD2S4tv1Yzx0ZDYeE3fd5Pvg",
  grantType: "authorization_code",
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

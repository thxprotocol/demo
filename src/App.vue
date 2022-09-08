<template>
  <nav id="header" class="w-full z-30 top-0 py-1">
    <div
      class="
        w-full
        container
        mx-auto
        flex flex-wrap
        items-center
        justify-between
        mt-0
        px-6
        py-3
      "
    >
      <label for="menu-toggle" class="cursor-pointer md:hidden block">
        <svg
          class="fill-current text-gray-900"
          xmlns="http://www.w3.org/2000/svg"
          width="20"
          height="20"
          viewBox="0 0 20 20"
        >
          <title>menu</title>
          <path d="M0 3h20v2H0V3zm0 6h20v2H0V9zm0 6h20v2H0v-2z"></path>
        </svg>
      </label>
      <input class="hidden" type="checkbox" id="menu-toggle" />

      <div
        class="
          hidden
          md:flex md:items-center md:w-auto
          w-full
          order-3
          md:order-1
        "
        id="menu"
      >
        <nav>
          <ul
            class="
              md:flex
              items-center
              justify-between
              text-base text-gray-700
              pt-4
              md:pt-0
            "
          ></ul>
        </nav>
      </div>

      <div class="order-1 md:order-2">
        <a
          class="
            flex
            items-center
            tracking-wide
            no-underline
            hover:no-underline
            font-bold
            text-gray-800 text-xl
          "
          href="#"
        >
          <svg
            class="fill-current text-gray-800 mr-2"
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            viewBox="0 0 24 24"
          ></svg>
          THX PROTOCOL SDK DEMO PAGE
        </a>
      </div>
    </div>
  </nav>

  <!--	 
Alternatively if you want to just have a single hero
<section class="w-full mx-auto bg-nordic-gray-light flex pt-12 md:pt-0 md:items-center bg-cover bg-right" style="max-width:1600px; height: 32rem; background-image: url('https://images.unsplash.com/photo-1422190441165-ec2956dc9ecc?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1600&q=80');">
	<div class="container mx-auto">
		<div class="flex flex-col w-full lg:w-1/2 justify-center items-start  px-6 tracking-wide">
			<h1 class="text-black text-2xl my-4">Stripy Zig Zag Jigsaw Pillow and Duvet Set</h1>
			<a class="text-xl inline-block no-underline border-b border-gray-600 leading-relaxed hover:text-black hover:border-black" href="#">products</a>
		</div>
    </div>
</section>
-->

  <section class="bg-white py-8">
    <div class="container mx-auto flex items-center flex-wrap pt-4 pb-12">
      <nav id="store" class="w-full z-30 top-0 px-6 py-1">
        <div
          class="
            w-full
            container
            mx-auto
            flex flex-wrap
            items-center
            justify-between
            mt-0
            px-2
            py-3
          "
        >
          <a
            class="
              uppercase
              tracking-wide
              no-underline
              hover:no-underline
              font-bold
              text-gray-800 text-xl
            "
            href="#"
          >
            FEATURES
          </a>
        </div>
      </nav>

      <div class="md:w-1/2 p-2 xl:w-1/2">
        <a href="#">
          <vue-json-pretty
            virtual
            class="
              border-4
              rounded-sm
              aspect-square
              font-bold
            "
            :data="userData"
          />

          <div class="pt-3 flex items-center justify-between">
            <p class="flex justify-center font-bold">User Profile</p>
            <button
              @click="fetchProfile()"
              class="
                bg-blue-500
                hover:bg-blue-700
                text-white
                font-bold
                py-2
                px-4
                rounded
              "
            >
              Fetch
            </button>
          </div>
        </a>
      </div>

    <div class="md:w-1/2  p-2 xl:w-1/2">
        <a href="#">
          <vue-json-pretty
            virtual
            class="
              border-4
              rounded-sm
              aspect-square
              font-bold
            "
            :data="erc721Tokens"
          />

          <div class="pt-3 flex items-center justify-between">
            <p class="flex justify-center font-bold">ERC721 Tokens</p>
            <button
              @click="fetchERC721List()"
              class="
                bg-blue-500
                hover:bg-blue-700
                text-white
                font-bold
                py-2
                px-4
                rounded
              "
            >
              Fetch
            </button>
          </div>
        </a>
      </div>

      
    </div>
  </section>
</template>

<script>
import { THXClient } from "@thxprotocol/sdk";
import VueJsonPretty from "vue-json-pretty";

import "vue-json-pretty/lib/styles.css";

const client = new THXClient({
  clientId: process.env["VUE_APP_CLIENT_ID"],
  clientSecret: process.env["VUE_APP_CLIENT_SECRET"],
  grantType: "authorization_code",
  redirectUrl: "http://192.168.1.3:8080/signin-oidc",
  scopes: "openid account:read erc20:read erc721:read",
});

export default {
  name: "App",
  components: {
    VueJsonPretty,
  },
  data() {
    return {
      client,
      userData: null,
      erc721Tokens: null,
    };
  },

  methods: {
    async fetchProfile() {
      this.userData = await client.account.get();
    },

    async fetchERC721List() {
      this.erc721Tokens = await client.erc721.list()
    }
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

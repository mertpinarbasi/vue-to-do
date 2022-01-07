<template>
  <v-app>
    <AppHeader/>
    <HomePage />
  </v-app>
</template>

<script>
import HomePage from "./views/HomePage.vue";
import AppHeader from "./components/AppHeader.vue"
export default {
  name: "App",

  components: {
    HomePage,
   AppHeader
  },

  data: () => ({
    //
  }),
};
</script>

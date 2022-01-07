<template>
  <v-app>
    <HomePage />
  </v-app>
</template>

<script>
import HomePage from "./views/HomePage.vue";
export default {
  name: "App",

  components: {
    HomePage,
  },

  data: () => ({
    //
  }),
};
</script>

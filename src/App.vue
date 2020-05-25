<template>
  <v-app >
    <Header />
    <v-content style="background : #4A4A4B; height : 100% !important" >
      <router-view />
    </v-content>
  </v-app>
</template>

<script>
import Header from './components/header.vue';

export default {
  name: 'App',

  components: {
    Header,
  },

  data: () => ({
    //
  }),
};
</script>

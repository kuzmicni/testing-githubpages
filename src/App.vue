<template>
  <v-app>
    <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase">
        <span class="font-weight-light">MOVIE RECOMMENDATION ENGINE</span>
      </v-toolbar-title>
    </v-toolbar>
<v-content>
      <MainComponent/>
    </v-content>
  </v-app>
</template>

<style>
  @import './assets/styles/css_styles.css';
</style>

<script>
import MainComponent from './components/MainComponent';

export default {
  name: 'App',

  components: {
    MainComponent
  },

  data: () => ({
    //
  }),
};
</script>

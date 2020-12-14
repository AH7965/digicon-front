<template>
  <v-app>
    <v-app-bar
      app
      color="orange"
      dark
    >
      <div class="d-flex align-center">
        <h2>eNNergy Drink </h2>
      </div>

    </v-app-bar>
    <v-main>
      <RandomGet/>
    </v-main>

  <v-footer>
    <div class="flex-grow-1"></div>
    <div> @Hasshi7965 </div>
  </v-footer>
  </v-app>
</template>

<script>
import RandomGet from './components/randomget';
// import RandomGet from './components/HelloWorld';

export default {
  name: 'App',

  components: {
    RandomGet,
  },

  data: () => ({
    //
  }),
};
</script>

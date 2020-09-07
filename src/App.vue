<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        Callenge Web Developer
      </div>

      <v-spacer></v-spacer>

    </v-app-bar>

    <v-main>
      <AnimalList/>
    </v-main>
  </v-app>
</template>

<script>
import AnimalList from './components/AnimalList';

export default {
  name: 'App',

  components: {
    AnimalList,
  },

  data: () => ({
    //
  }),
};
</script>

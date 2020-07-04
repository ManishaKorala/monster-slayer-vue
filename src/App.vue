<template>
  <v-app>
    <v-main class="body-color">
      <FightMonster/>
    </v-main>
  </v-app>
</template>

<script>
import FightMonster from './components/FightMonster';

export default {
  name: 'App',

  components: {
    FightMonster,
  },
};
</script>
<style scoped>

  .body-color {
    background-color: lightcyan;
  }

</style>
<template>
  <v-app>

    <v-main>
      <taskList/>
    </v-main>
  </v-app>
</template>

<script>
import taskList from './components/taskList';

export default {
  name: 'App',

  components: {
    taskList,
  },

  data: () => ({
    //
  }),
};
</script>

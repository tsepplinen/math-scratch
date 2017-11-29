<template>
  <button class="add-button" @click="addNewScope">
      Add a box
  </button>
</template>

<script>

import { mapActions } from 'vuex';

export default {
  name: 'AddScopeButton',
  data() {
    return {
      msg: 'Welcome to Your Vue.js App',
    };
  },
  methods: {
    ...mapActions(['addScope']),
    addNewScope() {
      this.addScope('1+1');
      const scopes = this.$store.state.scopes;
      console.log(scopes[scopes.length - 1]);
    },
  },
};

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .add-button {
    background: mediumspringgreen;
  }
</style>

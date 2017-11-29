<template>
  <div id="app">
    <img src="./assets/logo.png">
    <ScopeList/>
    <AddScopeButton/>
  </div>
</template>

<script>
import ScopeList from './components/ScopeList';
import AddScopeButton from './components/AddScopeButton';

export default {
  name: 'app',
  components: {
    ScopeList,
    AddScopeButton,
  },
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 0;
  padding: 0 50px;
  background: lightcyan;
}
</style>

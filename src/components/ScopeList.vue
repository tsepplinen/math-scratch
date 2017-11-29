<template>
  <div class="scope-list">
    <EditableScope
      v-for="(scope, index) in $store.state.scopes"
        :key="index"
        :index="index"
        :data="scope.data" 
      />
  </div>
</template>

<script>

import EditableScope from './EditableScope';

export default {
  name: 'ScopeList',
  data() {
    return {
    };
  },
  components: {
    EditableScope,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>

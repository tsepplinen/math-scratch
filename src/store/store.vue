<script>

import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

const state = {
  scopes: [
    { data: 'PMT = [(1 + 0.075/12)^60 * 0.075/12] / [(1 + 0.075/12)^60 - 1] * 200990.92 = 1000' },
    { data: 'PMT = [(1 + 0.075/12)^60 * 0.075/12] / [(1 + 0.075/12)^60 - 1] * 200990.92' },
    { data: 'PMT = [(1 + 0.075/12)^60 * 0.075/12] / [(1 + 0.075/12)^60 - 1] * 200990.92' },
    { data: 'PMT = [(1 + 0.075/12)^60 * 0.075/12] / [(1 + 0.075/12)^60 - 1] * 200990.92' },
    { data: 'PMT = [(1 + 0.075/12)^60 * 0.075/12] / [(1 + 0.075/12)^60 - 1] * 200990.92' },
  ],
};

const actions = {
  addScope: ({ commit }, data) => {
    commit('addScope', data);
  },
  editScope: ({ commit }, payload) => {
    commit('editScope', payload);
  },
};

const mutations = {
  addScope(s, data) {
    s.scopes.push({ data });
  },
  editScope(store, payload) {
    const index = payload.index;
    const data = payload.data;
    Vue.set(store.scopes, index, { data });
  },
};

export default new Vuex.Store({
  state,
  actions,
  mutations,
});

</script>

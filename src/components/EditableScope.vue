<template>
<div class="scope" contenteditable="true"
      v-on:focus="handleFocus"
      v-on:blur="handleBlur">
  <div class="edit"
        v-show="editing"
  >{{data}}</div>
  <div class="pretty"
    tabindex="-1"
    v-show="!editing"
  >``</div>
</div>
</template>

<script>

import { mapActions } from 'vuex';

export default {
  name: 'EditableScope',
  data() {
    return {
      editing: true,
      editBox: null,
      prettyBox: null,
    };
  },
  props: ['data', 'index'],
  methods: {
    ...mapActions(['editScope']),
    handleBlur() {
      const math = this.editBox.innerText;
      this.prettifyMath(math);
    },
    prettifyMath(math) {
      const jaxFromPretty = window.MathJax.Hub.getAllJax(this.prettyBox)[0];
      window.MathJax.Hub.Queue(['Text', jaxFromPretty, math]);
      window.MathJax.Hub.Queue(() => {
        const payload = {
          index: this.index,
          data: math,
        };
        this.editScope(payload);
        this.editing = false;
      });
    },
    handleFocus() {
      this.editing = true;
    },
    displayContent() {
      return this.data;
    },
    show() {
      this.editing = false;
    },
  },
  mounted() {
    this.editBox = this.$el.querySelector('.edit');
    this.prettyBox = this.$el.querySelector('.pretty');
    this.prettyBox.innerHTML = '`#`'.replace('#', this.data);
    window.MathJax.Hub.Queue(['Typeset', window.MathJax.Hub, this.prettyBox]);
    window.MathJax.Hub.Queue(() => this.show());
  },
};

// function onFocus() {
//     console.log("focus")
//     editti.innerHTML = savedData;
// }

// function onBlur() {
//     console.log("blur")
//     savedData = editti.innerHTML;
//     editti.innerHTML = "`" + editti.innerHTML + "`";
//     MathJax.Hub.Queue(["Typeset", MathJax.Hub, editti]);

//     tulos.innerHTML = "`= " + math.eval(savedData, scope) + "`";
//     MathJax.Hub.Queue(["Typeset", MathJax.Hub, tulos]);

//     // let testing = MathJax.Hub.getAllJax("MathDiv")[0];
//     // MathJax.Hub.Queue(["Text",testing,"x+1"]);
// }
// let scope = {};
// let tulos = document.getElementById("tulos");
// let savedData = "K = 1/5";
// let editti = document.getElementById("editti");
// editti.innerHTML = savedData;
// editti.addEventListener("focus", onFocus);
// editti.addEventListener("blur", onBlur);

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .scope {
    background: white;
    border: 1px solid darkcyan;
    padding: 2px;
    margin: 5px 0;    
  }

</style>

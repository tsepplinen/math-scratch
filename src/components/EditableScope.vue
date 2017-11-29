<template>
<div class="scope" :tabindex="tabindex"
      v-on:focus="handleFocus"
      @click="handleFocus">
  <textarea class="edit" 
        v-show="editing"
        v-on:blur="handleBlur"
        v-model="internalData"
  />
  <div class="pretty"
    tabindex="-1"
    v-show="!editing"
  >``</div>
</div>
</template>

<script>

import { mapActions } from 'vuex';

const NO_TABBING = -1;
const NORMAL_TABBING = 0;

export default {
  name: 'EditableScope',
  data() {
    return {
      editing: true,
      focused: false,
      editBox: null,
      prettyBox: null,
      value: this.data,
      tabindex: 0,
    };
  },
  props: ['data', 'index'],
  computed: {
    internalData: {
      get() {
        return this.value;
      },
      set(newValue) {
        this.value = newValue;
      },
    },
  },
  methods: {
    ...mapActions(['editScope']),
    handleBlur() {
      const math = this.value;
      this.tabindex = NORMAL_TABBING;
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
        this.focused = false;
      });
    },
    handleFocus() {
      this.editing = true;
      this.tabindex = NO_TABBING;
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
  updated() {
    if (this.editing && !this.focused) {
      this.focused = true;
      this.editBox.focus();
    }
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
  
  textarea.edit {
    width: 100%;
    height: 100%;
    border: none;
    resize: none;
    padding: 0;
    margin: 0;  
    text-align: center;
  }

  textarea.edit:focus {
    /* outline: -webkit-focus-ring-color auto 5px; */
    outline: darkslateblue dashed 2px; 
    /* outline-offset: -2px; */
    outline-offset: -1px;
  }

</style>

<template>
<div>
  <div class="scope" :tabindex="tabindex"
        v-on:focus="handleFocus"
        @click="handleFocus">
    <textarea class="edit" 
          v-show="editing"
          v-on:blur="handleBlur"
          v-model="internalData"
          :rows="rows"
          @keyup.enter="updateRows"
          @keyup.delete="updateRows"
    />
    <div class="pretty"
      tabindex="-1"
      v-show="!editing"
    >``</div>
  </div>
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
      rows: 5,
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
    ...mapActions(['editScope', 'removeScope']),
    handleBlur() {
      this.value = this.value.trim();

      if (this.value.length === 0) {
        this.removeScope({ index: this.index });
      } else {
        const math = this.value;
        this.tabindex = NORMAL_TABBING;
        this.prettifyMath(math);
        this.updateRows();
      }
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
    focus() {
      this.editing = true;
      this.focused = false;
    },
    updateRows() {
      const matches = this.internalData.match(/\n/g);
      let rows;
      if (matches) {
        rows = matches.length + 2;
        this.rows = Math.max(rows, 2);
      } else {
        this.rows = 2;
      }
    },
  },
  mounted() {
    this.editBox = this.$el.querySelector('.edit');
    this.updateRows();
    this.prettyBox = this.$el.querySelector('.pretty');
    this.prettyBox.innerHTML = '`#`'.replace('#', this.data);
    window.MathJax.Hub.Queue(['Typeset', window.MathJax.Hub, this.prettyBox]);
    window.MathJax.Hub.Queue(() => this.show());
    window.MathJax.Hub.Queue(() => this.updateRows());
    window.MathJax.Hub.Queue(() => this.focus());
  },
  updated() {
    if (this.editing && !this.focused) {
      this.focused = true;
      this.editBox.focus();
    }
  },
};

// function onBlur() {

//     tulos.innerHTML = "`= " + math.eval(savedData, scope) + "`";
//     MathJax.Hub.Queue(["Typeset", MathJax.Hub, tulos]);

//     // let testing = MathJax.Hub.getAllJax("MathDiv")[0];
//     // MathJax.Hub.Queue(["Text",testing,"x+1"]);
// }
// let scope = {};
// let tulos = document.getElementById("tulos");

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
    height: auto;
    border: none;
    resize: none;
    padding: 0;
    margin: 0;  
    text-align: center;
    line-height: 2;
    overflow: hidden;
  }

  textarea.edit:focus {
    /* outline: -webkit-focus-ring-color auto 5px; */
    outline: darkslateblue dashed 2px; 
    /* outline-offset: -2px; */
    outline-offset: -1px;
  }

</style>

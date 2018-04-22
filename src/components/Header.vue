<template>
  <header class="header">

    <div class="container">

      <!-- search -->
      <form class="header__search" @submit.prevent="onSearchSubmit">
        <input type="text" class="header__search-textfield"
          v-model="query"
          placeholder=""
          :class="{ 'header__search-textfield--invalid': invalid }"
        >
        <span class="invalid" v-if="invalid === true">Пустое поле</span>
        <button v-on:click="resetForm" class="header__reset visible" v-if="visible === true">
          <i class="material-icons">clear</i>
        </button>
        <button class="header__search-submit">
          <i class="material-icons">search</i>
        </button>
      </form> <!-- ./search -->

      <app-nav />
    </div>

  </header>
</template>

<script>
import Nav from './Nav.vue'
export default {
  data () {
    return {
      query: '',
      invalidMessage: '',
      invalid: false,
      visible: false,
    }
  },
  methods: {
    onSearchSubmit () {
      if (this.query.length === 0) {
        this.invalid = true;
        this.invalidMessage = 'Пустое поле ввода';
      } else {
        this.$emit('searchQuery', this.query);
      }
    },
    resetForm () {
      if(this.query.length > 0) {
        this.query = "";
      } 
    }
  },
  watch: {
    query () {
      if (this.query.length > 0 && this.invalid === true) {
        this.invalid = false;
        this.invalidMessage = '';
      }
      if (this.query.length > 0) {
          this.visible = true;
      } else {
        this.visible = false;
      }
      

      console.log(this.query);
    }
  },
  components: {
    appNav: Nav,
  }
}
</script>

<style scoped>
  .header {
    background: #fff;
    border-bottom: 1px solid rgba(0, 0, 0, .1);
    box-shadow: 0 5px 5px rgba(0, 0, 0, .07);
    padding: 25px 0;
  }

  .container {
    display: flex;
    justify-content: space-between;
  }

  .header__search {
    display: flex;
    align-items: center;
    position: relative;
  }

  .header__search-textfield {
    border: none;
    outline: none;
    border-bottom: 1px solid rgba(0, 0, 0, .5);
    font-size: 16px;
    padding-right: 20px;
    margin-right: 20px;
  }

  .header__search-textfield--invalid {
    border-bottom-color: red;
  }
  
  .header__search-submit,
  .header__reset {
    background: none;
    border: none;
    outline: none;
    display: flex;
    align-items: center;
    justify-content: center;
    line-height: 1;
    padding: 0; 
    cursor: pointer;
    
  }
  .header__reset {
    display: none;
    position: absolute;
    top: 0;
    right: 40px;
    animation: hide .5s ease-in-out;
  }
  .visible {
    display:flex;
    animation: hide 1s ease-in-out;
  }
  @keyframes hide {
    from{ opacity: 0; }   
    to {  opacity: 1; }
   }
  .header__search-submit > i,
  .header__reset > i{
    font-size: 18px;
  }

  .invalid {
    position: absolute;
    color: red;
    left: 0;
    bottom: -20px;
    font-size: 13px;
    animation: move 1s ease-in-out;
  }
  @keyframes move {
    from{ transform: translateY(-20px) }   
    to {  transform: translateY(0) }
   }
</style>

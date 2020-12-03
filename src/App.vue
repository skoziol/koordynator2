<template>
  <Popup msg="Welcome to Your Vue.js App"/>
</template>

<script>
import Popup from './components/Popup.vue'

export default {
  name: 'App',
  components: {
    Popup
  }
}
</script>

<style lang='scss'>
body{
  background: lightgray;

  button{
    border: none;
    display:flex;

    &.sample{
      margin: 2px 5px;
      height: 35px;
    }
    outline: none;
  }

  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    display:flex;
    align-content: center;
    justify-content: center;
    height: 100%;
    width: 100%;

    .popup{
      box-shadow: 0px 10px 10px 1px rgba(0,0,0,0.15);
      height: 90vh;
      width: 90vw;
    }
  }
}
</style>

<template>
  <div id="app">
    <Header 
    @inizioRicerca='inizioRicerca'
    />
    <Main :toSearch=this.toSearchText 
        
    />
  </div>
</template>

<script>

import Main from './components/Main'
import Header from './components/Header'

export default {
  name: 'App',

  data(){
    return{
      toSearchText:''
    }
  },

  methods:{
    inizioRicerca(text){
      this.toSearchText=text
    }
  },

  components: {
    Header,
    Main
  }
}
</script>

<style lang="scss">

  @import './assets/styles/general.scss';
  body{
    background-color: #1B1B1B;
  }
</style>

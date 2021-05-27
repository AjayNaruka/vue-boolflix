<template>
  <div id="app">
    <Header 
    @inizioRicerca='inizioRicerca'
    @invioActive='invioActive'
    />
    <Main 
    :toSearch=this.toSearchText 
    :activeUser=this.currentUser
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
      toSearchText:'',
      currentUser:undefined
    }
  },

  methods:{
    inizioRicerca(text){
      this.toSearchText=text
    },
    invioActive(user){
      console.log('user ricevuto: ',user);
      this.currentUser=user
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

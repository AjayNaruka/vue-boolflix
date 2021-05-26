<template>
  <div class="header d-flex align-items-center justify-content-between ps-5 pe-5">
    <h1>BOOLFLIX</h1>
    <div class="myInput">
      <form action="" class="d-flex">
        <input type="text" placeholder="Cerca un film o serie" v-model="inputText">
        <button class="btn btn-primary ms-5" @click.prevent='ricerca'>INVIO</button>
      </form>
      
    </div>
  </div>
</template>

<script>
export default {
  name:'Header',
  data(){
    return{
      inputText:''
    }
  },
  methods:{
    ricerca(){
      this.$emit('inizioRicerca',this.inputText)
    }
  }
}
</script>

<style lang='scss' scoped>

  .header{
    height: 80px;
    background-color: black;
    h1{
      color:white
    }
    input{
      border-radius: 15px;
      padding: 8px;
      border: 0;
      outline: 0;
    }
  }

</style>
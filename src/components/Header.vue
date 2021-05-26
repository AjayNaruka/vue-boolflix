<template>
  <div class="header d-flex align-items-center justify-content-between ps-5 pe-5">
    <h1>BOOLFLIx</h1>
    <div class="myInput">
      <form action="">
        <input type="text" placeholder="Text" v-model="inputText">
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
    background-color: red;
    
  }

</style>
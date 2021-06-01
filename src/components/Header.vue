<template>
  <div class="header d-flex align-items-center justify-content-between ps-5 pe-5">
    <h1>BOOLFLIX</h1>
    <div class="myInput">
      <form action="" class="d-flex">
        <input type="text" placeholder="Cerca un film o serie" v-model="inputText">
        <button class="btn btn-primary ms-5" @click.prevent='ricerca'>INVIO</button>
        <i class="user-icon fas fa-user" @click='openLista()'><span class="ms-2">{{this.userArray[this.activeHomeUser].name.toUpperCase()}}</span></i>
        
        <div
        v-if="this.openList"
         class="user-list">
          <ul class="text-center">
            <li
            v-for='(user,index) in userArray'
            :key="index"
            @click='attivaUser(index)'
            >{{user.name}}</li>
          </ul>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name:'Header',
  data(){
    return{
      inputText:'',
      openList:false,
      activeHomeUser:0,
      userArray:[
        {
          name:'Ajay',
          favGen:[12],
          stillWatching:[
            {
              id:20,
              timeWatched:50
            }
          ]
      },
      {
          name:'Paolo',
          favGen:[36]
      },
      {
          name:'Luca',
          favGen:[16]
      },
      {
          name:'Giorgio',
          favGen:[35]
      },
      ]
    }
  },
  created(){
    this.$emit("invioActive",this.userArray[0])
  },
  methods:{
    ricerca(){
      this.$emit('inizioRicerca',this.inputText)
    },
    attivaUser(index){
      this.activeHomeUser=index
      console.log('utente attivo ',this.activeHomeUser);
      this.openList=false
      this.invioActiveUser()
    },
    openLista(){
      this.openList=!this.openList
    },
    invioActiveUser(){
      this.$emit("invioActive",this.userArray[this.activeHomeUser])
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
    .user-icon{
      color: white;
      margin: auto;
      margin-left: 35px;
      font-size: 30px;
    }
    .user-list{
      background-color: white;
      padding: 5px 30px;
      position: absolute;
      right: 45px;
      top: 80px;
      ul{
        text-align: left;
        list-style: none;
        margin-bottom: 0;
        padding: 0;
        li{
          cursor: pointer;
        }
      }
    }
  }

</style>
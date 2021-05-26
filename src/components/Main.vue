<template>
  <div class="myContainer d-flex flex-wrap">
    <div 
    v-for='(movie,index) in received'
    :key='index'
    class="carta">
      <h3>{{movie.title}}</h3>
      <h3>TEST</h3>
      <h3>TEST</h3>
      <h3>TEST</h3>
    </div>
    
  </div>
</template>

<script>

import axios from'axios'

export default {
  name:'Main',
  data(){
    return{
      toSearchText:'',
      received:[],
      apikey:'7b9e9b8d883ea487635e20b63041c707',
      apiurl:'https://api.themoviedb.org/3/search/movie'
    }
  },
  props:{
    toSearch: String
  },
  watch:{
    toSearch:function(){
      console.log('watching');
      
        this.axiosCall()
      console.log(this.received);
      
      
    } 
  },
  
  
  methods:{
    axiosCall(){
      if(this.toSearch!=''){
        axios.get(this.apiurl,{
        params:{
          api_key:this.apikey,
          query: this.toSearch,
          language:'it-IT'
        }
      })
      .then(res => {
      this.received=res.data.results
      console.log(res.data.results)
      })
      .catch(() => {
        
      })
      return 0
      }else{
        console.log('ricerca vuota');
        this.received=[]
      }
      }
        
      
    
  },
  
}

  
</script>

<style lang="scss" scoped>

  .myContainer{
    background-color: green;
    width: 90%;
    margin: 0 auto;
    .carta{
      flex-basis: calc(100% / 5);
      background-color: yellow;
      margin-right: 10px;
      padding: 5px;
      margin-top: 10px;
    }
  }
</style>
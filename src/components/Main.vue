<template>
  <div class="myContainer d-flex flex-wrap">
    <div 
    v-for='(movie,index) in received'
    :key='index'
    class="carta">
      <h5>{{movie.title}}</h5>
      <h5>{{movie.original_title}}</h5>
      <h5>{{movie.original_language}}</h5>
      <h5>{{movie.vote_average}}</h5>
    </div>
    
  </div>
</template>

<script>

import axios from'axios'

export default {
  name:'Main',
  data(){
    return{
      received:[],
      receivedMovies:[],
      receivedSeries:[],
      apikey:'7b9e9b8d883ea487635e20b63041c707',
      apiurl:'https://api.themoviedb.org/3/search/movie',
      pageNumber:1,
      isFirstCall: true
    }
  },
  props:{
    toSearch: String
  },
  watch:{
    toSearch:function(){
        this.received=[]
        this.axiosCall()
    } 
  },

  methods:{
    axiosCall(){
      if(this.toSearch!=''){
        axios.get(this.apiurl,{
        params:{
          api_key:this.apikey,
          query: this.toSearch,
          language:'it-IT',
          page:this.pageNumber++,
        }
      })
      .then(res => {
      let toConcatArray=[]
      this.maxpages=res.data.total_pages
      toConcatArray=res.data.results
      toConcatArray.forEach(element => {
        this.received.push(element)
      });
      if(this.pageNumber<=res.data.total_pages){
        this.axiosCall()
      }else{
        this.pageNumber=1
      }
      
      })
      .catch(() => {
        console.log('errore');
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
<template>
  <div>
    <!-- <h1>{{this.toSearch}}</h1> -->
    <div class="myContainer d-flex flex-wrap justify-content-center">
    <h1 v-if="emptySearch" class="mt-5">CERCA UN FILM O SERIE</h1>
    <!-- <h1 v-if="noResults">NESSUN FILM TROVATO</h1> -->
    <div 
    v-for='(movie,index) in received'
    :key='index'
    class="carta">
      <div class="poster">
        <img :src="getPoster(movie)" class="img-fluid" alt="">
        <img src="https://image.shutterstock.com/image-vector/no-image-available-sign-internet-260nw-261719003.jpg" class="notfound" alt="">
        
      </div>
      <div 
      v-if="true"
      class="infos">
        <h5>{{movie.title}}</h5>
        <h5>{{movie.name}}</h5>
        <h5>{{movie.original_title}}</h5>
        <h5>{{movie.original_name}}</h5>
        <h5><img class="countryFlag" :src="getLangImg(movie)" alt=""></h5>
        <i 
        v-for="index in Math.round(movie.vote_average/2)"
        :key="index"
        class="fas fa-star"></i>
        <p>{{movie.overview}}</p>
      </div>
      
    </div>
    
  </div>
  </div>  
  
</template>

<script>

import axios from'axios'

export default {
  name:'Main',
  data(){
    return{
      emptySearch:true,
      noResults:false,
      received:[],
      receivedMovies:[],
      receivedSeries:[],
      apikey:'7b9e9b8d883ea487635e20b63041c707',
      apiurl:'https://api.themoviedb.org/3/search/multi',
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
        if(this.received.length===0 && !this.emptySearch){
          this.noResults=true
        }else{
          this.noResults=false
        }
    } 
  },

  methods:{
    axiosCall(){
      if(this.toSearch!=''){
        this.emptySearch=false
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
        this.emptySearch=true
        this.noResults=false
      }
      },
      getLangImg(movie){
        let lang = movie.original_language
        if(lang==='en') lang='us'
        if(lang==='ja') lang ='jp'
        if(lang==='ko') lang ='kr'
        return "https://www.countryflags.io/"+lang+ "/flat/64.png"
      },
      getPoster(movie){
        let url = 'https://image.tmdb.org/t/p/w342'
        return url+movie.poster_path
      }
  }, 
} 
</script>

<style lang="scss" scoped>

  .myContainer{
    width: 90%;
    margin: 0 auto;
    .carta{
      i{
        color: yellow;
      }
      cursor: pointer;
      position: relative;
      flex-basis: calc(100% / 5);
      margin-right: 10px;
      margin-top: 50px;
      margin-bottom: 20px;
      overflow: hidden;
      text-overflow: ellipsis;
      height: 513px;
      .poster{
        height: 100%;
      }
      .poster img{
        height: 100%;
        transition: all .7s;
      }
      &:hover .poster img{
        opacity: .05;
        transform: scale(1.2,1.2);
      }
      &:hover .infos{
        position: absolute;
        top: 0;
        display: block;
        width: 100%;
      }
      &:hover .poster{
        background-color: rgba(25,25,112,.2);
      }
      &:hover img.notfound{
        opacity: 0;
      }
      .infos{
        display: none;
        height: 100%;
        border: 2px solid white;
        color: white;
        padding: 15px;
      }
      }
      img.countryFlag{
          width: 20px;
      }
    }
  
</style>
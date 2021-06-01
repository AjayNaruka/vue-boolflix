<template>
  <div
    class="carta">
      <div class="poster">
        <img :src="getPoster(movie)" class="img-fluid" alt="">
        
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
        <div class="star"
        v-for="index in 5"
        :key="index"
        >
          <i
          v-if="index <Math.round(movie.vote_average/2)"
           class="fas fa-star"></i>
          <i class="far fa-star"
          v-else
          ></i>
        </div>
        <p>{{movie.overview}}</p>
      </div>
    </div>
</template>

<script>
export default {
  name:'Film',
  props:{
    movie:Object
  },
  methods:{
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
      },
  }
}
</script>

<style lang='scss'>
  .carta{
      i{
        color: yellow;
      }
      background-color: rgba(25,25,112,.2);
      cursor: pointer;
      position: relative;
      /* flex-basis: calc(100% / 5); */
      margin-right: 10px;
      margin-top: 80px;
      margin-bottom: 20px;
      overflow: hidden;
      max-height: 513px;
    
      .poster img{
        width: 100%;
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
</style>
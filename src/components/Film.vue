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
          <p>{{movie.overview}}</p>
        </div>
      </div>
</template>

<script>
export default {
  name:'Film'
}
</script>

<style>

</style>
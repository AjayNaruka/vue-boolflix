<template>
  <div>
    <!-- <h1>{{this.toSearch}}</h1> -->
    <div class="myContainer d-flex flex-wrap justify-content-center">

      <!-- HOME SCREEN:  -->

      <div
        v-if="emptySearch"
        class="d-flex flex-wrap home-screen justify-content-center mt-5"
      >
        <div class="benvenuto d-block">
          <h2>In base alle tue preferenze: {{ stampaGeneri() }}</h2>
        </div>

        <div
          v-for="(movie, index) in initial.slice(0, 4)"
          :key="index"
          class="carta home"
        >
          <div class="poster">
            <img :src="getPoster(movie)" class="img-fluid" alt="" />
          </div>
          <div v-if="true" class="infos">
            <h5>{{ movie.title }}</h5>
            <h5>{{ movie.name }}</h5>
            <h5>{{ movie.original_title }}</h5>
            <h5>{{ movie.original_name }}</h5>
            <h5>Attore: {{ stampaAttori(movie.id)}} </h5>
            <h5><img class="countryFlag" :src="getLangImg(movie)" alt="" /></h5>
            <div class="stars"
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
            <p>{{ movie.overview }}</p>
          </div>
        </div>
      </div>

      <!-- RICERCA -->
      
      <div v-if="!emptySearch" class="filterSelect">
        <select name="" id="" v-model="currentFilter">
          <option value="all">All</option>
          <option value="movies">Movies</option>
          <option value="series">Series</option>
        </select>
      </div>
      <div v-for="(movie, index) in filterArray" :key="index" class="carta">
        <div class="poster">
          <img :src="getPoster(movie)" class="img-fluid" alt="" />
        </div>
        <div v-if="true" class="infos">
          <h5>{{ movie.title }}</h5>
          <h5>{{ movie.name }}</h5>
          <h5>{{ movie.original_title }}</h5>
          <h5>{{ movie.original_name }}</h5>
          <h5>Attore: {{ stampaAttori(movie.id)}} </h5>
          <h5><img class="countryFlag" :src="getLangImg(movie)" alt="" /></h5>
          <!-- <i
            v-for="index in Math.round(movie.vote_average / 2)"
            :key="index"
            class="fas fa-star"
          ></i> -->
          <div class="stars"
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
          <p>{{ movie.overview }}</p>
        </div>
      </div>

      <div v-if="filterArray.length === 0 && !emptySearch" class="mt-5">
        <h1>Nessun Risultato</h1>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
/* import Film from '../components/Film' */

export default {
  name: "Main",
  data() {
    return {
      currentFilter: "all",
      emptySearch: true,
      noResults: false,
      initial: [],
      received: [],
      receivedMovies: [],
      receivedSeries: [],
      apikey: "7b9e9b8d883ea487635e20b63041c707",
      apiurl: "https://api.themoviedb.org/3/search/multi",
      pageNumber: 1,
      isFirstCall: true,
      possibleGenres: []
    };
  },
  props: {
    toSearch: String,
    activeUser: Object
  },
  watch: {
    toSearch: function() {
      this.received = [];
      this.axiosCall();
      if (this.received.length === 0 && !this.emptySearch) {
        this.noResults = true;
      } else {
        this.noResults = false;
      }
    },
    activeUser: function() {
      axios
        .get("https://api.themoviedb.org/3/discover/movie", {
          params: {
            api_key: this.apikey,
            with_genres: this.activeUser.favGen[0]
          }
        })
        .then(res => {
          this.initial = res.data.results;
        });
    }
  },
  components: {
    /* Film, */
  },
  created() {
    axios
      .get(
        "https://api.themoviedb.org/3/genre/movie/list?api_key=7b9e9b8d883ea487635e20b63041c707"
      )
      .then(res => {
        this.possibleGenres = res.data.genres;
      })
      .catch(err => {
        console.error(err);
      });
  },
  computed: {
    filterArray() {
      let movies = this.received.filter(item => "original_title" in item);
      let series = this.received.filter(item => "original_name" in item);
      if (this.currentFilter === "movies") {
        return movies;
      } else if (this.currentFilter === "series") {
        return series;
      }
      console.log("filter rec: ", this.received);
      return movies.concat(series);
    }
  },
  methods: {

    stampaAttori(id){
      let actors =[]
      let actorsNames=[]
      let primoAttore = ''
      axios.get(`https://api.themoviedb.org/3/movie/${id}/credits?api_key=7b9e9b8d883ea487635e20b63041c707`)
      .then(res => {
        actors = res.data.cast
        console.log(id,actors);
        actors.forEach((element,index) => {
          if(index===0){
            console.log('elemento : ',element.name);
          }
          actorsNames.push(element.name)
          /* console.log(actorsNames); */
        });
        console.log('primo: ',actorsNames[0]);
        primoAttore = actorsNames[0]
        console.log(primoAttore);
        
      })
      .catch(() => {
        console.error('errore'); 
        return 0
      })

      return primoAttore
    },

    axiosCall() {
      if (this.toSearch != "") {
        this.emptySearch = false;
        axios
          .get(this.apiurl, {
            params: {
              api_key: this.apikey,
              query: this.toSearch,
              language: "it-IT",
              page: this.pageNumber
            }
          })
          .then(res => {
            this.received = [];
            console.log("sto cercando: ", this.toSearch);
            this.received = res.data.results;
            console.log(this.received);
           /*  if(this.pageNumber<=res.data.total_pages){
        this.axiosCall()
      }else{
        this.pageNumber=1
        console.log('sto cercando: ',this.toSearch);
      } */
          })
          .catch(() => {
            console.log("errore");
          });
        return 0;
      } else {
        console.log("ricerca vuota");
        this.received = [];
        this.emptySearch = true;
        this.noResults = false;
      }
    },
    getLangImg(movie) {
      let lang = movie.original_language;
      if (lang === "en") lang = "us";
      if (lang === "ja") lang = "jp";
      if (lang === "ko") lang = "kr";
      return "https://www.countryflags.io/" + lang + "/flat/64.png";
    },
    getPoster(movie) {
      let url = "https://image.tmdb.org/t/p/w342";
      return url + movie.poster_path;
    },
    stampaGeneri() {
      let genere = "";
      this.possibleGenres.forEach(element => {
        if (this.activeUser.favGen.includes(element.id)) {
          genere = element.name;
        }
      });
      return genere;
    }
  }
};
</script>

<style lang="scss" scoped>
.stars{
  display: inline-block;
}
.filterSelect {
  position: absolute;
  right: 150px;
  top: 100px;
  select {
    padding: 5px;
    border-radius: 10px;
    outline: 0;
  }
}
.imagenotfound {
  height: 100%;
}
.benvenuto {
  text-align: center;
  width: 100%;
}
.myContainer {
  width: 90%;
  margin: 0 auto;
  .home-screen {
    color: white;
  }
  .carta {
    i {
      color: yellow;
    }
    background-color: rgba(25, 25, 112, 0.2);
    cursor: pointer;
    position: relative;
    /* flex-basis: calc(100% / 5); */
    margin-right: 10px;
    margin-top: 80px;
    margin-bottom: 20px;
    overflow: hidden;
    max-height: 513px;
    /* min-height: 400px; */

    .poster{
      height: 100%;
    }
    .poster img {
      width:100%;
      width: auto;
      transition: all 0.7s;
    }
    &:hover .poster img {
      opacity: 0.05;
      transform: scale(1.2, 1.2);
    }
    &:hover .infos {
      position: absolute;
      top: 0;
      display: block;
      width: 100%;
    }
    &:hover .poster {
      background-color: rgba(25, 25, 112, 0.2);
    }
    &:hover img.notfound {
      opacity: 0;
    }
    .infos {
      display: none;
      height: 100%;
      border: 2px solid white;
      color: white;
      padding: 15px;
    }
  }
  img.countryFlag {
    width: 20px;
  }
}
@media all and (max-width: 600px) {
  .carta {
    flex-basis: calc(100% / 2);
  }
}
@media all and(min-width: 601px ) and(max-width:1100px) {
  .carta {
    flex-basis: calc(100% / 3);
  }
}
@media all and (min-width: 1101px) {
  .carta {
    flex-basis: calc(100% / 6);
  }
  .carta.home{
    flex-basis: calc(100% / 5);
  }
}
</style>

<template>
<div>
  <v-container>
    <v-layout text-xs-center wrap>
      <v-flex>

        <form>
          <v-text-field v-model="movieTitle" label="Movie Title" required></v-text-field>

          <v-btn @click="submit">submit</v-btn>
          <v-btn @click="clear">clear</v-btn>
        </form>

        <br/><br/>



      </v-flex>
    </v-layout>
  </v-container>
  
      <h2 v-if="this.recommendations.length > 1"> Your Recommendations </h2>

      <div v-if="this.recommendations.length > 1" class="container" style="max-width: 1200px; margin-top:20px;">
        <div class="col-lg-12">
              <div class="row">

                <div v-for="movie in this.recommendations" :key="movie" class="movie-card col-md-2">

                        <img v-if="movie['poster']=='True'" :src="getImgUrl(movie['movieId'])" v-bind:alt="pic">

                </div>
              </div>
        </div>
      </div>

    <h2 v-if="this.recommendations.length === 1"> Please Search Another Movie </h2>
  </div>
</template>

<script>
import $ from 'jquery';
import axios from 'axios';
import all_movies_json from './json/movies.json';

export default {
    data() {
        return {
                movieTitle: '',
                recMovie : '',
                api_recommendations: [],
                recommendations:[],
                movie:{}
        }
      
    },
    
    methods: {
    submit () {
      var self = this;
      self.recommendations = [];
      axios.post('http://127.0.0.1:5000/recms', {
        movie_title: this.movieTitle
      })
      .then(function(response){

        var api_recommendations = response.data.rec_movie                           //api_recommendations is an array containing a list of titles
        var found_movie = [];                                                       //dummy variable to store a matched movie
        
        $.each(api_recommendations,function(movie_title){                           //movie_title is just an index 0,1,2..
                    
            found_movie = all_movies_json.filter((obj) => obj.title === api_recommendations[movie_title]);
            self.recommendations.push(found_movie[0])

        });
      })
    },

    // computed:{
    //           searchResults(){

    //             if(this.search.length < 2){
    //               return []
    //             }
    //             all_movies = (this.all_movies_json || [])
    //             self = this;

    //             result = all_movies_json.filter(item => {
    //                 movie_name = item["title"]
    //                 pos = movie_name.toString().toLowerCase().indexOf(self.search.toLowerCase())
    //                 if (pos > -1){
    //                     return -1
    //                  }else{
    //                   return 0;
    //                  }

    //                  });    

    //            if(result.length > 100){
                
    //             result = result.slice(0,100)
               
    //            } 


    //            return result

    //            }
    //   },

    clear () {
      this.movieTitle = ''
      this.api_recommendations = []
      this.recommendations = []
    },

    getImgUrl(pet) {
            var images = require.context('../assets/posters', false, /\.jpg$/)
            return images('./' + pet + ".jpg")
  }
  }
}
</script>
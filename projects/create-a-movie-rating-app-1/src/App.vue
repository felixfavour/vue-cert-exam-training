<script setup>
import { items } from "./movies.json";
/*
 This is an Icon that you can use to represent the stars if you like
 otherwise you could just use a simple ⭐️ emoji, or * character.
*/
import { StarIcon } from "@heroicons/vue/24/solid";
import { ref } from "vue";

const movies = ref({...items, {
			"id": 5,
			"name": "The Shawshank Redemption",
			"description": "Two imprisoned men bond over a number of years, finding solace and eventual redemption through acts of common decency.",
			"image": "https://m.media-amazon.com/images/M/MV5BNDE3ODcxYzMtY2YzZC00NmNlLWJiNDMtZDViZWM2MzIxZDYwXkEyXkFqcGdeQXVyNjAwNDUxODI@._V1_FMjpg_UX1200_.jpg",
			"rating": 4,
			"genres": [
				"Drama"
			],
			"inTheaters": false
		}})

</script>

<template>
  <!-- This is where your template goes	-->
  <div class="movies flex justify-around">
    <div v-for="movie in movies" :key="movie.id" class="movie-card bg-white w-[250px]">
      <img :src="movie.image" :alt="`${movie.rating} image`">
      <div class="movie-info">
        <h3 class="movie-title text-xl font-bold">
          {{movie.name}}
        </h3>
        <div class="movie-genres">
          <span v-for="genre in movie.genres" :key="genre" class="movie-genre">
            {{genre}}
          </span>
        </div>
        <p>{{movie.description}}</p>
        <div class="movie-rating flex">
          <span>Rating: ({{movie.rating}}/5)</span>
          <div class="stars w-[100px] flex">
            <StarIcon v-for="star in movie.rating" :key="`star-${star}`" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { items } from "./movies.json";
import { StarIcon } from "@heroicons/vue/24/solid";
import { ref } from "vue";

const movies = ref(items)
</script>

<template>
  <div class="app">
    <div class="movie-list">
      <div v-for="movie in movies" class="movie-item">
        <div class="movie-image-wrapper">
          <img :src="movie.image" alt="" class="movie-item-image" />
        </div>
        <div class="movie-item-info-wrapper">
          <h3 class="movie-item-title">{{ movie.name }}</h3>
          <div class="movie-genres-wrapper">
            <span
                v-for="genre in movie.genres"
                :key="genre"
                class="genre"
            >{{ genre }}
            </span>
          </div>
          <p>{{ movie.description }}</p>
          <div class="movie-item-rating-wrapper">
            <span
                v-for="rating in movie.rating"
                :key="rating"
            >
              <StarIcon class="w-5 h-5 text-yellow-500" />
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

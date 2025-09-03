<script setup>
import { items } from "./movies.json";
import { StarIcon } from "@heroicons/vue/24/solid";
import { ref } from "vue";

const movies = ref(items);

function updateRating(movieIndex, rating) {
  movies.value[movieIndex].rating = rating;
}
</script>

<template>
  <div class="h-screen flex items-center justify-center">
    <div class="max-w-7xl">
      <div class="flex flex-row gap-5">
        <div
            v-for="(movie, movieIndex) in movies"
            :key="movie.id"
            class="text-black bg-white rounded-xl shadow-md overflow-hidden w-96"
        >
          <div class="relative">
            <img
                :src="movie.image"
                alt="movie image"
                class="block object-cover object-center h-[600px]"
            />
            <div class="absolute top-3 right-3 w-16 h-16">
              <!-- Star-Icon -->
              <StarIcon
                  v-if="movie.rating"
                  class="w-16 h-16 text-yellow-500"
              />
              <StarIcon
                  v-else
                  class="w-16 h-16 text-gray-300"
              />

              <!-- Text in der Mitte -->
              <span
                  class="absolute inset-0 flex items-center justify-center text-white font-bold"
              >
                {{ movie.rating || "-" }}
              </span>
            </div>
          </div>

          <div class="p-3">
            <h2 class="text-xl font-bold p-1">{{ movie.name }}</h2>
            <div class="flex flex-row gap-1">
              <span
                  v-for="genre in movie.genres"
                  :key="`${movie.id}-${genre}`"
                  class="bg-purple-500 rounded-xl p-1 text-white"
              >
                {{ genre }}
              </span>
            </div>

            <p>{{ movie.description }}</p>

            <div class="flex flex-row gap-1 py-3">
              <span>Rating: ( {{ movie.rating }} / 5 )</span>
            </div>

            <div class="flex flex-row gap-1 py-3">
              <button
                  v-for="star in 5"
                  :key="star"
                  :class="
                  star <= movie.rating
                    ? 'text-yellow-500'
                    : 'text-gray-500'
                "
                  :disabled="star === movie.rating"
                  @click="updateRating(movieIndex, star)"
              >
                <StarIcon class="w-5 h-5 inline-block" />
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

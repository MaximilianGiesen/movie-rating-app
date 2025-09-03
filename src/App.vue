<script setup>
import { items } from "./movies.json";
import { StarIcon } from "@heroicons/vue/24/solid";
import { ref } from "vue";

const movies = ref(items);

function updateRating(movieIndex, rating) {
  movies.value[movieIndex].rating = rating;
}

const dialog = ref(null);

function openModal() {
  dialog.value.showModal();
}

function closeModal() {
  dialog.value.close();
}
</script>

<template>
  <dialog ref="dialog" class="bg-white rounded-md p-4">
    <div class="relative p-4 bg-white rounded-md">
      <button
          class="absolute top-2 right-2 inline-flex items-center justify-center w-8 h-8 border-2 border-black rounded-full transition-colors duration-150 hover:bg-gray-300 focus:shadow-outline"
          @click="closeModal()"
      >
        x
      </button>
      <div class="mt-10">
        <p>Greetings, one and all!</p>
        <form method="dialog" class="mt-4">
          <button class="bg-blue-500 text-white px-4 py-2 rounded-md">send</button>
        </form>
      </div>
    </div>
  </dialog>
  <div class="h-screen flex items-center justify-center space-x-4">
    <div class="absolute top-4 right-8">
      <button class="bg-blue-500 text-white px-4 py-2 rounded-md" @click="openModal()">
        Add movie
      </button>
    </div>
    <div class="flex flex-row gap-5">
      <div
          v-for="(movie, movieIndex) in movies"
          :key="movie.id"
          class="w-96 h-auto bg-white rounded-md flex flex-col items-center justify-start overflow-hidden shadow-2xl"
      >
        <div class="h-[520px] overflow-hidden w-full relative">
          <img
              :src="movie.image"
              alt="movie image"
              class="object-cover object-center h-[600px]"
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
</template>

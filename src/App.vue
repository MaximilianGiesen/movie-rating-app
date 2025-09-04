<script setup>
import { items } from "./movies.json";
import { StarIcon } from "@heroicons/vue/24/solid";
import { ref } from "vue";

const movies = ref(items);

const formData = ref({
  name: '',
  description: '',
  image: '',
  genres: [],
  rating: 0,
  inTheaters: false,
});

const errors = ref({
  name: "",
  description: "",
  image: "",
  genres: "",
  inTheaters: "",
});

function updateRating(movieIndex, rating) {
  movies.value[movieIndex].rating = rating;
}

const dialog = ref(null);

function openModal() {
  dialog.value.showModal();
}

function closeModal() {
  errors.value = {
    name: "",
    description: "",
    image: "",
    genres: "",
    inTheaters: "",
  };

  formData.value = {
    name: '',
    description: '',
    image: '',
    genres: [],
    rating: 0,
    inTheaters: false,
  }
  dialog.value.close();
}

function validateForm(e) {
  e.preventDefault();

  errors.value = {
    name: "",
    description: "",
    image: "",
    genres: "",
    inTheaters: "",
  };

  if (!formData.value.name) {
    errors.value.name = "Please enter a movie name";
  }

  if (!formData.value.genres.length) {
    errors.value.genres = "Please choose at least one genre";
  }

  else {
    addMovie();
    closeModal();
  }

}

function addMovie() {
  const newMovie = {
    name: formData.value.name,
    description: formData.value.description,
    image: formData.value.image,
    genres: formData.value.genres,
    rating: 0,
    inTheaters: formData.value.inTheaters,
  };

  movies.value.push(newMovie);
}

function deleteMovie(movieIndex) {
  movies.value.splice(movieIndex, 1);
}

const averageRating = () => {
  let sum = 0;

  for(let i = 0; i < movies.value.length; i++) {
    sum += movies.value[i].rating;
  }
  let avg = sum / movies.value.length;
  avg = Math.round(avg * 10) / 10;

  return avg;
}

const editingIndex = ref(null);

function editMovie(movie, index) {
  formData.value = {
    name: movie.name,
    description: movie.description,
    image: movie.image,
    genres: [...movie.genres],
    rating: movie.rating,
    inTheaters: movie.inTheaters,
  };
  editingIndex.value = index;
  openModal();
}


function updateMovie() {
  const i = editingIndex.value;
  if (i == null || !movies.value[i]) return;

  movies.value[i] = {
    ...movies.value[i],
    ...formData.value,
  };

  closeModal();
}

</script>

<template>
  <dialog ref="dialog" class="bg-white rounded-md p-4 w-96">
    <div class="relative p-4 bg-white rounded-md">
      <button
          class="absolute top-2 right-2 inline-flex items-center justify-center w-8 h-8 border-2 border-black rounded-full transition-colors duration-150 hover:bg-gray-300 focus:shadow-outline"
          @click="closeModal()"
      >
        x
      </button>
      <div class="mt-10">
        <h2 class="text-xl">Add a new Movie</h2>
        <form method="dialog" class="mt-4 flex flex-col gap-4" @submit.prevent="validateForm">
          <label for="name" class="text-sm font-medium text-gray-700">Movie name:</label>
          <input
              type="text"
              id="name"
              v-model="formData.name"
              placeholder="Enter the movie name"
              :class="[
                'border rounded-md p-2',
                errors.name ? 'border-red-500' : 'border-gray-300'
              ]"
          >
          <p v-if="errors.name" class="text-red-500">{{ errors.name }}</p>

          <label for="description" class="text-sm font-medium text-gray-700">Movie description:</label>
          <textarea
              id="description"
              v-model="formData.description"
              placeholder="Enter the movie description"
              class="border rounded-md p-2 border-gray-300"
          ></textarea>

          <label for="image" class="text-sm font-medium text-gray-700">Movie image:</label>
          <input
              type="text"
              id="image"
              v-model="formData.image"
              placeholder="Enter the movie image-path"
              class="border rounded-md p-2 border-gray-300"
          >

          <label for="genres">Choose genres:</label>
          <select
              name="genres"
              v-model="formData.genres"
              id="genres" multiple
              :class="[
                'border rounded-md p-2',
                errors.genres ? 'border-red-500' : 'border-gray-300'
              ]"
          >
            <option value="drama">Drama</option>
            <option value="action">Action</option>
            <option value="crime">Crime</option>
          </select>
          <p v-if="errors.genres" class="text-red-500">{{ errors.genres }}</p>

          <div class="flex flex-row gap-2">
            <label for="theaters" class="text-sm font-medium text-gray-700">Is in theaters:</label>
            <input
                type="checkbox"
                id="theaters"
                v-model="formData.inTheaters"
                class="mr-2"
            >
          </div>
          <div class="justify-between flex flex-row gap-2">
            <button type="button" class="bg-gray-500 text-white px-4 py-2 rounded-md" @click="closeModal()">cancel</button>
            <button type="button" class="bg-blue-500 text-white px-4 py-2 rounded-md" @click="updateMovie()">update</button>
          </div>
        </form>
      </div>
    </div>
  </dialog>
  <p class="bg-dark:bg-gray-900 text-center text-white p-4">
    Total Movies: {{ movies.length }}
  </p>
  <p class="bg-dark:bg-gray-900 text-center text-white p-4">
    Average Rating: {{ averageRating() }} / 5
  </p>
  <div class="h-screen flex flex-cols-3 items-center justify-center space-x-4">
    <div class="absolute top-8 right-8">
      <button class="bg-blue-500 text-white px-4 py-2 rounded-md" @click="openModal()">
        Add movie
      </button>
    </div>
    <div class="grid grid-cols-3 gap-5">
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

          <div class="flex flex-row gap-1 py-3 justify-between">
            <div>
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
            <div>
              <button class="bg-blue-500 text-white px-2 py-1 rounded-md mr-2" @click="deleteMovie(movieIndex)">Remove</button>
              <button class="bg-blue-500 text-white px-2 py-1 rounded-md" @click="editMovie(movie, movieIndex)">edit</button>
            </div>
          </div>

        </div>
      </div>
    </div>
  </div>
</template>

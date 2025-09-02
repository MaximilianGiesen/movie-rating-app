<script setup>
import { items } from "../movies.json";
</script>

<template>
  <div>
    <ul class="flex flex-row gap-3">
      <li v-for="item in items" class="text-black bg-white border-2 border-black rounded-lg p-3">
        <img :src="item.image" alt="movie image" class="w-full h-auto rounded-lg">
        <p>{{ item.name}}</p>
        <p>{{ item.description}}</p>
        <p>{{ item.genres.toLocaleString }}</p>
        <p>{{ item.rating }} / 5</p>
      </li>
    </ul>
  </div>
</template>

<style scoped>
</style>


<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input type="text" v-model="searchQuery" @input="getSearchResults" placeholder="Search for a City or State"
        class="py-2 px-2 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]">


      <ul class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-2 top-[66px]
   " v-if="mapboxSearchResult">

        <p v-if="searchError">{{ searchError }} </p>

        <p v-if="!searchError && mapboxSearchResult.length === 0">
          No results found
        </p>
        <p v-if="!searchError && mapboxSearchResult.length > 0">
          Search results
        </p>


        <li v-for="searchResult in mapboxSearchResult" :key="searchResult.id" class="py-2 cursor-pointer">
          {{ searchResult.place_name }}
        </li>
      </ul>
    </div>
  </main>
</template>

<script setup>
import { ref } from 'vue'
import axios from "axios";

const searchQuery = ref("")
const searchError = ref("")


const queryTomeout = ref(null)
const mapboxAPIKey =
  "pk.eyJ1Ijoiam9obmtvbWFybmlja2kiLCJhIjoiY2t5NjFzODZvMHJkaDJ1bWx6OGVieGxreSJ9.IpojdT3U3NENknF6_WhR2Q";

const mapboxSearchResult = ref(null)

const getSearchResults = () => {
  clearTimeout(queryTomeout.value)
  queryTomeout.value = setTimeout(async () => {
    console.log(searchQuery.value)

    if (searchQuery.value !== "") {

      try {
        console.log("searching")
        const result = await axios.get(
          `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapboxAPIKey}&types=place`
        );
        mapboxSearchResult.value = result.data.features;
      } catch (error) {
        searchError.value = error
      }


      return;
    }
    mapboxSearchResult.value = null;
  }, 300)
}
</script>
<template>
  <div>
    <!-- search field -->
    <form>
      <div class="bg-white border border-indigo-600/30 rounded-lg shadow-lg flex items-center">
        <i class="fa-solid fa-magnifying-glass p-2 text-indigo-600"></i>
        <input
          type="text"
          placeholder="Search for a place"
          class="rounded-r-lg p-2 border-0 outline-0 focus:ring-2 focus:ring-indigo-600 ring-inset w-full"
          v-model="searchTerm.query"
          @input="handleSearch"
        />
      </div>
    </form>
    <!-- search suggestions -->
    <div class="bg-white my-2 rounded-lg shadow-lg">
      <div v-if="searchTerm.results !== null">
        <div v-for="place in searchTerm.results" :key="place.id">
          <button
            @click="getWeather(place.id)"
            class="px-3 my-2 hover:text-indigo-600 hover:font-bold w-full text-left"
          >
            {{ place.name }}, {{ place.region }}, {{ place.country }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive } from 'vue'

const emit = defineEmits(['place-data'])

const searchTerm = reactive({
  query: '',
  timeout: 0,
  results: null
})

const handleSearch = () => {
  clearTimeout(searchTerm.timeout)

  searchTerm.timeout = setTimeout(async () => {
    if (searchTerm.query !== '') {
      try {
        const response = await fetch(
          `http://api.weatherapi.com/v1/search.json?key=2ee1979b9244456c94150530242203&q=${searchTerm.query}`
        )
        const data = await response.json()
        searchTerm.results = data
        console.log(searchTerm.results)
      } catch (error) {
        throw new Error('Error while fecthing data')
      }
    } else {
      searchTerm.results = null
    }
  }, 500)
}

const getWeather = async (id: number) => {
  const response = await fetch(
    `http://api.weatherapi.com/v1/forecast.json?key=2ee1979b9244456c94150530242203&q=id:${id}&days=3&aqi=no&alerts=no`
  )

  const data = await response.json()

  emit('place-data', data)

  searchTerm.query = ''
  searchTerm.results = null

  console.log(data)
}
</script>

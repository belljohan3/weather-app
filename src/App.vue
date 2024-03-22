<template>
  <main>
    <!-- Date -->
    <div>
      <TodayDate />
    </div>

    <!-- Search -->
    <div>
      <SearchInput @place-data="addPlace" />
    </div>

    <!-- Weather Cards -->
    <div class="grid grid-cols-2 gap-4">
      <div v-for="(place, idx) in places" :key="idx">
        <WeatherCard :place="place" @delete-place="deletePlace" />
      </div>
    </div>
  </main>
</template>

<script setup lang="ts">
import TodayDate from './components/TodayDate.vue'
import SearchInput from './components/SearchInput.vue'
import WeatherCard from './components/WeatherCard.vue'
import { ref } from 'vue'

const places = ref([])

const addPlace = (data) => {
  places.value.push(data)
}

const deletePlace = (name) => {
  if (confirm('Are you sure that you want to delete ?')) {
    places.value = places.value.filter((p) => p.location.name !== name)
  }
}
</script>

<script setup lang="ts">
import SelectFilter from '@/components/SelectFilter.vue'
import BreweryCard from '@/views/Home/BreweryCard.vue'
import services from '@/services'
import { useBreweryStore } from '@/store'
import { onMounted } from 'vue'
import { useRouter } from 'vue-router'

const FILTER_DESCRIPTION = 'Select a type'

const router = useRouter()
const store = useBreweryStore()

async function getBreweries() {
  const response = await services.breweries.getAll()
  store.setBreweries(response.data)
}

function openBreweryCard(id: string) {
  router.push({ name: 'Brewery', params: { id } })
}

onMounted(() => {
  getBreweries()
})
</script>

<template>
  <main data-cy="home-view" class="home">
    <div class="home__filter">
      <SelectFilter
        :options="store.breweriesTypes"
        :filterDescription="FILTER_DESCRIPTION"
        :onChange="(type: string) => store.setSelectedType(type)"
      />
    </div>

    <div class="home__list">
      <BreweryCard
        data-cy="brewery-card"
        @click="() => openBreweryCard(brewery.id)"
        v-for="brewery in store.filteredBreweries"
        :key="brewery.id"
        :brewery="brewery"
      />
    </div>
  </main>
</template>

<style scoped>
.home {
  padding: 20px var(--default-width-padding);
  height: auto;
  background-color: var(--home-bg);
}

.home__list {
  margin-top: 1.5rem;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
}


@media(max-width: 767px) {
  .home__list {
    grid-template-columns: repeat(1, 1fr);
  }
}
</style>

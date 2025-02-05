<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

import CardList from '../components/CardList.vue'

const favorites = ref([])

const removeFromFavorites = async (item) => {
  try {
    await axios.delete(`https://16a086a5c6bf3891.mokky.dev/favorites/${item.favoriteId}`)
    favorites.value = favorites.value.filter((fav) => fav.id !== item.id)
    item.isFavorite = false
  } catch (error) {
    console.log(error)
  }
}

onMounted(async () => {
  try {
    const { data } = await axios.get(
      'https://16a086a5c6bf3891.mokky.dev/favorites?_relations=items',
    )
    favorites.value = data.map((obj) => ({ ...obj.item, favoriteId: obj.id, isFavorite: true }))
  } catch (error) {
    console.log(error)
  }
})
</script>

<template>
  <h2 class="text-3xl font-bold mb-8">Мои закладки</h2>

  <CardList :items="favorites" is-favorites @remove-from-favorites="removeFromFavorites" />
</template>

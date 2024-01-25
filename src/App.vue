<script setup>
import Header from '@/components/Header.vue'
import CardList from '@/components/CardList.vue'
import Drawer from '@/components/Drawer.vue'
import { onMounted, provide, reactive, ref, watch } from 'vue'
import axios from 'axios'

const items = ref([])


const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const onChangeSelect = event => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = (event) => {
  filters.searchQuery = event.target.value
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(`https://d871c66b172f8d2c.mokky.dev/favorites`
    )
    items.value = items.value.map(item => {
      const favorite = favorites.find(favorites => favorites.parentId === item.id)

      if (!favorite) {
        return item
      }
      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (err) {
    console.log(err)
  }
}

const addToFavorite = async (item) => {
  try {

    if (!item.isFavorite) {
      const obj = {
        parentId: item.id
      }
      item.isFavorite = true
      const { data } = await axios.post(`https://d871c66b172f8d2c.mokky.dev/favorites`, obj);
      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      await axios.delete(`https://d871c66b172f8d2c.mokky.dev/favorites/${item.favoriteId}`)
      item.favoriteId = null
    }
  } catch (err) {
    console.log(err)
  }
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const { data } = await axios.get(`https://604781a0efa572c1.mokky.dev/items`,
      { params }
    )
    items.value = data.map(obj => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }))
  } catch (err) {
    console.log(err)
  }
}

onMounted(async () => {
  await fetchItems()
  await fetchFavorites()
})
watch(filters, fetchItems)


provide('addToFavorite', addToFavorite)
</script>

<template>
    <Drawer />
  <div
    class='bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14'
  >
    <Header />
    <div class='p-10'>
      <div class='flex justify-between items-center'>
        <h2 class='text-3xl font-bold mb-8'>Все кроссовки</h2>

        <div class='flex gap-4'>
          <select @change='onChangeSelect' class='py-2 px-3 border rounded-md outline-none'>
            <option value='name'>По названию</option>
            <option value='price'>По цене (дешево)</option>
            <option value='-price'>По цене (дорого)</option>
          </select>
          <div class='relative'>
            <img class='absolute left-4 top-3' src='/search.svg' alt='search'>
            <input @input='onChangeSearchInput'
                   class='border rounded-md py-1.5 pl-11 pr-4 outline-none focus:border-gray-400' type='text'
                   placeholder='Поиск...' />
          </div>
        </div>
      </div>
      <div class='mt-10'>
        <CardList :items='items' @addToFavorite='addToFavorite' />
      </div>

    </div>
  </div>
</template>

<style scoped>
</style>

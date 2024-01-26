<script setup>
import Card from './Card.vue'

defineProps({
  items: Array
})

const emit = defineEmits(['addToFavorite','addToCart'])

</script>

<template>
  <div class='grid grid-cols-4 gap-5' v-auto-animate>
    <Card
      v-for='item in items'
      :key='item.id'
      :id='item.id'
      :price='item.price'
      :title='item.title'
      :image-url='item.imageUrl'
      :is-added='item.isAdded'
      :is-favorite='item.isFavorite'
      :on-click-favorite='() => emit("addToFavorite",item)'
      :on-click-add='() => emit("addToCart",item)'
    />

  </div>
</template>

<script setup>
import { inject } from 'vue'

const props = defineProps({
  id:Number,
  imageUrl: String,
  title: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickAdd: Function,
  onClickFavorite: Function
})

</script>

<template>
  <div
    class='relative m-10 border-slate-200 rounded-3xl p-8 cursor-pointer hover:-translate-y-2 hover:shadow-xl transition'>
    <img :src='!isFavorite ? "/like-1.svg" : "/like-2.svg"'
         alt='Like-1'
         class='absolute top-8 left-8'
         @click='onClickFavorite'
    >
    <img :src='imageUrl' alt='Sneakers'>
    <p class='mt-2'>{{ title }}</p>
    <div class='flex justify-between mt-5'>
      <div class='flex flex-col'>
        <span class='text-slate-400'>Цена:</span>
        <b>{{ price }}</b>
      </div>
      <img @click='onClickAdd' :src='isAdded ? "/checked.svg" : "/plus.svg"' alt='plus'>
    </div>
  </div>
</template>
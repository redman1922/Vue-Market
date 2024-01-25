<script setup >
  import Card from './Card.vue'

  defineProps({
    items: Array
  })

  function onClickAdd(){
    alert('Dobav')
  }


</script>

<template>
  <div class='grid grid-cols-4 gap-5'>
    <Card
      v-for='item in items'
      :key='item.id'
      :price='item.price'
      :title='item.title'
      :image-url='item.imageUrl'
      :is-added='true'
      :is-favorite='true'
      :on-click-add='onClickAdd'
    />

  </div>
</template>

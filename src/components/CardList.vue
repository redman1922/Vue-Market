<script setup lang='ts'>
  import Card from './Card.vue'

  function onClickAdd(){
    alert('Dobav')
  }


</script>

<template>
  <div class='grid grid-cols-4 gap-5'>
    <Card
      :price='1499'
      title='Мужсские Кроссовки Nike Mid Suede'
      image-url='/sneakers/sneakers-1.jpg'
      :is-added='true'
      :is-favorite='true'
      :on-click-add='onClickAdd'
    />

  </div>
</template>

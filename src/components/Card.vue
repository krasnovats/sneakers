<script setup>
defineProps({
    title: String,
    price: Number,
    imageUrl: String,
    isFavotite: Boolean,
    isAdded: Boolean,
    onClickAdd: Function,
    onClickFavorite: Function
})

</script>


<template>
      <div class="bg-white relative border border-slate-100 rounded-3xl p-8 cursor-pointer hover:-translate-y-2 hover:shadow-xl transition-all">
        <img @click="onClickFavorite" :src="isFavotite ? '/like-1.svg' : '/like-2.svg'" alt="like" class="absolute top-8 left-8">
        <img :src="imageUrl" alt="sneaker">
        <p class="mt-2">{{ title }}</p>


        <div class="flex justify-between mt-5">
          <div class="flex flex-col">
            <span class="text-slate-400">Цена:</span>
            <b>{{ price }}</b>
          </div>

          <img @click="onClickAdd" :src="!isAdded ? '/plus.svg' : '/checked.svg'" alt="plus">

        </div>
      </div>

</template>

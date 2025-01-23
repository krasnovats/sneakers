
<script setup>
import { onMounted, ref, watch, reactive } from 'vue';
import axios from 'axios';
import Header from './components/Header.vue';
import CardList from './components/CardList.vue';
import Drawer from './components/Drawer.vue';

const items = ref([]);

const filters= reactive ({
  sortBy: 'title',
  searchQuery: ''
});


const onChangeSelect = (event) => {
  filters.sortBy = event.target.value; // Обновляем значение sortBy
}

const onChangeSearchInput = (event) => {
  filters.searchQuery = event.target.value; // Обновляем значение sortBy
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(`https://ad7c786d6aba5622.mokky.dev/favorites`)

   items.value = items.value.map( item => {
    const favorite = favorites.find(favorite => favorite.id === item.id)
   }
    
  )} catch (err) {
    console.log(err);
  }
}



const fetchItems = async () => {
  const params = {
    sortBy: filters.sortBy,
  };

  if (filters.searchQuery) {
    params.title = `*${filters.searchQuery}*`;
  }

  try {
    const { data } = await axios.get(
      `https://ad7c786d6aba5622.mokky.dev/items`, {
        params
      }
      )
   items.value = data;
  } catch (err) {
    console.log(err);
  }
}

onMounted( async () => {
await fetchItems();
await fetchFavorites();
})
watch(filters, fetchItems);

</script>

<template>

  <!-- <Drawer /> -->

  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">

    <Header />

    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>

        <div class="flex gap-4">
          <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
            <option value="name">По названию</option>
            <option value="price">По цене (дешевые)</option>
            <option value="-price">По цене (дорогие)</option>
          </select>
          <div class="relative">
            <img class="absolute left-4 top-3.5" src="/search.svg" alt="">
            <input @input="onChangeSearchInput" class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400" placeholder="Поиск..."/>
          </div>
        </div>
      </div>
  
      
    <div class="mt-10">
      <CardList :items="items"/> 
    </div> 
    </div>
    

  </div>
</template>


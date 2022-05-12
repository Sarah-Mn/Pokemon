<template>

<div v-if="state.pokemon" class="select-none max-w-sm m-auto rounded overflow-hidden shadow-lg bg-white flex justify-center flex-col items-center">
  <h3 class="text-2xl font-bold text-green-900 uppercase my-5">{{state.pokemon.name}}</h3>
    <!-- <img class="w-full" :src="img" alt="Sunset in the mountains"> -->
    <div class="flex justify-center w-full">
      <img :src="state.pokemon.sprites.front_shiny" alt="" class="w-48 drop-shadow-3xl">
      <img :src="state.pokemon.sprites.back_shiny" alt="" class="w-48 drop-shadow-3xl">
    </div>

    <div class="px-6 py-10">
      <div class="font-bold text-xl mb-2">Types</div>
      <p class="text-gray-700 text-xl flex flex-col items-center justify-center" v-for="(type, inx) in state.pokemon.types" :key="inx">
          {{type.type.name}}
      </p>
    </div>
  </div>

</template>
<script setup>
import { useRoute } from 'vue-router';
import { reactive, onMounted, toRefs } from 'vue';
import axios from 'axios';

const state = reactive({
  pokemon: null,
  loading: false,
  data: {
    text: "iejiejfiejfieeij"
  }
  
  })

const route = useRoute();

onMounted(()=>{
  try {
    state.loading = true;
    axios.get(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}/`).then(
 res => {
   console.log(res.data);
   state.pokemon = res.data
 }
)
  } catch (error) {
    console.error(error);
  } finally {
    state.loading = false;
  }

return { ... toRefs(state)}

})


</script>

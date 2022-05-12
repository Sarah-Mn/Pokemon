<template>
<div class="w-full flex justify-center">
  <input type="text" v-model="state.text" placeholder="Enter pokemon here" class="mt-10 pr-10 pl-2 outline-none py-2 border-blue-500 rounded">
</div>
<div class="mt-10 p-4 flex flex-wrap justify-center">
  <div v-for="(pokemon, inx) in state.filteredPokemon" :key="inx" class="ml-4 text-2x text-blue-500">
  <router-link :to="`/about/${state.urlIdLookup[pokemon.name]}`">
  
  {{pokemon.name}}
  </router-link>
  </div>
</div>
</template>

<script setup>
import { reactive, toRefs, computed } from '@vue/reactivity';
import axios from 'axios';
import { onMounted } from 'vue';

const state = reactive({
  pokemons: [],
  urlIdLookup: {},
  text: "",
  filteredPokemon: computed(() => updatePokemon())
})

const updatePokemon = () => {
  if(!state.text) {
    return [];
  }

  return state.pokemons.filter((pokemon) => 
    pokemon.name.includes(state.text)
  );
}

onMounted(()=>{
  axios.get("https://pokeapi.co/api/v2/pokemon?offset=0").then(
 res => {
   console.log(res.data);
   state.pokemons = res.data.results;
   state.urlIdLookup = res.data.results.reduce((acc, curr, inx) => 
   acc = {...acc, [curr.name] : inx + 1}
   ,{});
 }
)

return { ... toRefs(state)}

})


</script>

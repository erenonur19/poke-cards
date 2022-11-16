<template>
  
<PokemonCards
:pokemons="pokemons"
:selectedId="selectedId"
@chosen="fetchEvolutions"
/>
<PokemonCards
:pokemons="evolutions"
/>
 
</template>


<script>

import PokemonCards from './components/PokemonCards.vue'
import { ref,onMounted} from 'vue';


export default{
components:{
  PokemonCards
},
setup(){

  const api='https://pokeapi.co/api/v2/pokemon'
  const ids=[1,4,7]
  const pokemons=ref([])
  const evolutions=ref([])
  const selectedId=ref(null)

 onMounted(async()=>{
    pokemons.value=await fetchData(ids)
  })
  async function fetchData(ids){
      const responses=await Promise.all(
        ids.map(id=> window.fetch(`${api}/${id}`))
      )
      const data=await Promise.all(
        responses.map(response=>response.json())
      )
      
      return data.map(datum=>({
        id:datum.id,
        name:datum.name,
        sprite:datum.sprites.other['official-artwork'].front_default,
        types:datum.types.map(type=>{
          return{
            name:type.type.name
          }
        })
      }))

    }

    async function fetchEvolutions(pokemon){
      evolutions.value=await fetchData(
        [pokemon.id+1, pokemon.id+2]
      )
      selectedId.value=pokemon.id
      console.log(evolutions);
      console.log(selectedId.value);
    }



  return{
    pokemons,
    fetchEvolutions,
    selectedId,
    evolutions
    
  }
}

}




</script>


<style scoped>


</style>

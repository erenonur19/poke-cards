<template>
  <div class="cards">

    <card
    v-for="pokemon in pokemons"
    :key="pokemon.id"
    @click="fetchEvolutions(pokemon)"
    :class="{opace:pokemon.id!==selectedId}"
    class="card"
     >
     <template v-slot:title>
      {{pokemon.name}} #{{pokemon.id}}
     </template>
     <template v-slot:content>
      <img :src="pokemon.sprite"/>
     </template>
     <template v-slot:description>
     <div
     v-for="type in pokemon.types" 
     :key="type" >
     {{type.name}}

     </div>
     
    

    
     
     </template>
     
    </card>
  
  </div>

  <div class="cards">

    <card
    v-for="pokemon in evolutions"
    :key="pokemon.id"
    @click="fetchEvolutions(pokemon)"
     >
     <template v-slot:title>
      {{pokemon.name}} #{{pokemon.id}}
     </template>
     <template v-slot:content>
      <img :src="pokemon.sprite"/>
     </template>
     <template v-slot:description>
     <div
     v-for="type in pokemon.types" 
     :key="type" >
     {{type.name}}

     </div>
     
    

    
     
     </template>
     
    </card>
  
  </div>
 
</template>


<script>
import Card from './components/Card.vue'
import { ref,onMounted} from 'vue';


export default{
components:{
  Card
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

.cards {
  display: flex;

  justify-content: center;
  margin-top: 15px;
}

img {
  width: 100%;
  display: block;
}
.opace{
  opacity:0.5;
}
.card:hover{
  opacity: 1;
}

</style>
